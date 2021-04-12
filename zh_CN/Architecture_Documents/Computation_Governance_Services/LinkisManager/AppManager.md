## 1. 背景
1. 新增AM模块将Entrance之前做的管理Engine的功能移动到AM模块
2. AM需要支持操作Engine，包括：新增、复用、回收、预热、切换等功能
3. 需要对接Manager模块对外提供Engine的管理功能：包括Engine状态维护、引擎列表维护、引擎信息等
4. AM需要管理EM服务，需要完成EM的注册并将资源注册转发给RM进行EM的资源注册
5. AM需要对接Label模块，包括EM/Engine的增删需要通知标签管理器进行标签更新
6. AM另外需要对接标签模块进行标签解析，并需要通过一系列标签获取一些列打好分的serverInstance列表（？EM和Engine怎么区分，1、标签完全不一样）
7. 需要对外提供基础接口：包括引擎和引擎管理器的增删改，提供metric查询等

## 2. 架构图

![](../../../Images/Architecture/AppManager-01.png)

如上图所示：AM在LinkisMaster中属于AppManager模块，作为一个Service提供服务

新引擎申请流程图：
![](../../../Images/Architecture/AppManager-02.png)

## 3. Job执行流程的变化
从上面的引擎生命周期流程图可知，Entrance已经不在做Engine的管理工作，engine的启动和管理都由AM控制，那么在这一过程中。Entrance需要去掉部分代码包括：
```
EngineManager：引擎管理功能去除
EntranceExecutorManager:Executor申请逻辑修改，修改去AM进行申请
EngineRequester：修改为请求AM
EntranceReceiver：去掉和EM的交互逻辑
EntranceBroadcastListener：
```
Job执行流程变化，主要体现在前后端传输部分：
```JSON
##以前的逻辑：
   {
	"method": "/api/rest_j/v1/entrance/execute",
	"websocketTag": "3335b043-81b2-419d-a6b3-3a9739de74ed",
	"data": {
		"executeApplicationName": "spark",//执行的引擎类型，指定对应的计算引擎如Hive、Spark等
		"requestApplicationName":"dss",//发起调用的上层系统（Creator）,比如Scriptis，dss等
		"executionCode": "select \"${testvar}\"\n",//提交的脚本
		"runType": "sql",
		"params": {
			"variable": {//自定义变量
				"testvar": "hello"
			},
			"configuration": {
				 "special":{ //特殊配置参数 如日志路径，结果集路径等
                    "k2":"v2"
                },
                "runtime":{ //引擎/执行配置参数 引擎执行时参数
                    "k3":"v3"
                },
                "startup":{ //启动参数,如果启动engine的内存设置
                    "k4":"v4"
                }
			}
		},
		"source": {
			"scriptPath": "sql"//脚本来源
		}
	}
}


##之后的逻辑：
	"method": "/api/rest_j/v1/entrance/execute",
	"websocketTag": "3335b043-81b2-419d-a6b3-3a9739de74ed",
	"data": {
		"executionContent": {"content":"select \"${testvar}\"\n","type":"sql"},//修改项：不一定支持脚本
		"params": {不变},
		"source": {不变},
		"labels":{//新增标签功能
			"engineType":"spark",
			"creator":"scriptis",
			"runType":"Interactive"
		}
	}
}

```

## 4. AM 具体实现逻辑：

请求引擎的步骤：
AskEngienRequest=》RPC/Rest =》 MasterEventHandler =》AskEngineService =》ReuseEngienRequest=&gt;CreateEngineService

引擎创建步骤：
CreateEngienRequest=》RPC/Rest =》 MasterEventHandler =》CreateEngineService =》
=》LabelContext/EnginePlugin/RMResourcevice=》（RcycleEngineService）EngineNodeManager=》EMNodeManager=》sender.ask(EngineLaunchRequest)=》EngineManager服务=》EngineNodeManager=》EngineLocker=》Engine=》EngineNodeManager=》EngineFactory=&gt;EngineService=&gt;ServerInstance
在创建引擎是存在和RM交互的部分，EnginePlugin应该需要通过Lables返回具体的资源类型，然后AM向RM发送资源请求

引擎复用步骤：
ReuseEngienRequest=》RPC/Rest =》 MasterEventHandler =》ReuseEngineService =》
=》abelContext=》EngineNodeManager=》EngineSelector=》EngineLocker=》Engine=》EngineNodeManager=》EngineReuser=》EngineService=&gt;ServerInstance

引擎切换步骤：
SwitchEngienRequest=》RPC/Rest =》 MasterEventHandler =》SwitchEngineService =》LabelContext/EnginePlugin/RMResourcevice=》EngineNodeManager=》EngineLocker=》Engine=》EngineNodeManager=》EngineReuser=》EngineService=&gt;ServerInstance

引擎管理器注册步骤：
RegisterEMRequest=》RPC/Rest =》 MasterEventHandler =》EMService =》
=》LabelContext=》EMManager=》Persistence=》EMService=&gt;

引擎管理器停止步骤：
StopEMRequest=》RPC/Rest =》 MasterEventHandler =》EMService =》
LabelContext=》EMManager=》Persistence=》EMService=&gt;
这里需要清理所有已经启动engine和资源记录，是直接调用AM的EngineManager进行引擎清理，还是通过Listener模式

## 锁引擎步骤
```
lockEngine：
	AM-->lockEngine-->lockerID-->Entrance


releaseEngine:
	Entrance-->unlock-->AM-->Engine
	AM-->最大持有时间 unlock-->Engine
	Engine  unlock 需要空闲才能unlock
	engine  lock 需要空闲才能lock
```

## 问题记录：
1. 锁引擎采用现有的方式在engine的内存中存储
2. IO-Entrance和Engine响应慢问题，IO-Entrance拿到Engine后不释放，一直持有锁
3. EngineManager不会有缓存，所有的状态更新和ServiceInstance都需要操作数据库
4. Engine和EM的状态，由Engine状态发生变化时自动进行上报，RM的Monitor改成向Engine主动判断Engine或者EM是否存在，如果一段时间没有上报状态（如：5分钟）
5. EnginePlugin还需要给Engine打上concurrentEngine的标签是否支持并发，在进行Selector的时候需要做判断