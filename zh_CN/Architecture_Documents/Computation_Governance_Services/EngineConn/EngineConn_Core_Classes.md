## EngineConn核心类图

![EngineConn_Core_Class](../../../Images/Architecture/EngineConn/EngineConn_Core_Class.png)

EngineConn为连接器，包含引擎与具体集群的会话信息；Service是引擎需要的具体能力。其中，EngineServer为Engine微服务启动类，先通过EnginePlugin创建EngineConn，再初始化相关Executions，在Executions里面会通过EnginePlugin创建默认的Executor并执行。