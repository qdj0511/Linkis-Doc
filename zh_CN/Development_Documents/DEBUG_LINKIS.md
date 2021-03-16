> 本文档用于协助用户DEBUG Linkis的某个服务

## 1. DEBUG 某个 EngineConn 服务

**什么是 EngineConn 服务？**

EngineConn 服务是指由 EngineConnManager 启动的，用于执行用户Job请求的服务。

这些服务由于是由 EngineConnManager 自动启动，所以用户一般无法设置远程DEBUG的参数。

在测试环境下，用户可以通过设置成测试模式，来开启某个 EngineConn 的远程DEBUG。

#### 1. 进入 ${LINKIS_HOME}/conf目录，执行命令：

```bash
    
    vim linkis.properties
    
```

   将测试模式打开，参数如下：

```properties
                 
    wds.linkis.test.mode=true   # 打开测试模式
                 
```

#### 2. 给 Linkis 提交任务，让 Linkis 启动新的 EngineConn 来执行用户的任务请求

#### 3. 进入 ${LINKIS_HOME}/logs，打开 linkis-engineconnManager.log，查看最新的 EngineConn 启动脚本日志，如下例子：

```
    Running ${LINKIS_HOME}/bin/rootScript.sh enjoyyin /nemo/jdk1.8.0_141/jre/bin/java -Xmx2g -Xms2g -server -XX:+UseG1GC -XX:MaxPermSize=250m -XX:PermSize=128m  -XX:+PrintGCDetails -XX:+PrintGCTimeStamps -XX:+PrintGCDateStamps -Dwds.linkis.configuration=linkis-engine.properties -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=10092 -Djava.library.path=/appcom/Install/hadoop/lib/native -cp /appcom/Install/Linkis/Linkis-ujes-hive-enginemanager/lib/dataworkcloud-HiveEngine-0.6.0.jar:/appcom/Install/Linkis/Linkis-ujes-hive-enginemanager/conf:/appcom/Install/Linkis/Linkis-ujes-hive-enginemanager/lib/*:/appcom/config/hadoop-config:/appcom/config/hbase-config:/appcom/config/spark-config:/appcom/config/hive-config com.webank.bdp.dataworkcloud.engine.DataWorkCloudEngineApplication --dwc-conf _req_entrance_instance=shellEntrance,bdpdwc010001:10087 --dwc-conf bdp.dwc.tmpfile.clean.time=10:00 --dwc-conf ticketId=38a41dc2-95bb-48db-8921-b48bbfa2ab64 --dwc-conf dwc.application.instance=bdpdwc010001:25410 --dwc-conf creator=IDE --dwc-conf bdp.dwc.preheating.time=9:00 --dwc-conf bdp.dwc.yarnqueue=q01 --dwc-conf bdp.dwc.instance=10 --dwc-conf dwc.application.name=shellEngineManager --dwc-conf user=johnnwang --spring-conf eureka.client.serviceUrl.defaultZone=http://10.255.0.70:20303/eureka/ --spring-conf logging.config=classpath:log4j2-engine.xml --spring-conf spring.profiles.active=engine --spring-conf server.port=36546 --spring-conf spring.application.name=HiveEngine
```


   上面的日志出现了远程调试端口，即10092：

```
    -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=10092
       
```

#### 4. 打开Intellij IDEA，进入Linkis工程，如下图打开DEBUG：

 ![DEBUG截图](https://github.com/WeBankFinTech/Linkis/blob/master/docs/zh_CN/images/ch5/DEBUG%E6%88%AA%E5%9B%BE.png)
 
 请注意：
 
  - Host为Engine实际启动的IP
  - Port为步骤3获取的DEBUG端口
  - Use module classpath需为实际需要DEBUG的模块
  
#### 5. 打断点，开始DEBUG。

## 2. DEBUG非Engine服务

 DEBUG非Engine服务比较简单，用户进入某个Linkis服务，修改start-*.sh脚本，加上远程调试即可。

 **例如我们想调试database服务：**
 
#### 1. 进入linkis-database/bin目录，执行命令：


```bash

    vim start-database.sh
        
```
  先加上如下一行shell脚本：
    
```bash

    export DEBUG_OPTS="-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=10092"
    
```

  请注意，这里的address为远程调试端口，且必须是没有被占用的端口
 
#### 2. 接下来修改Java启动命令，加上远程调试的命令$DEBUG_OPTS，如下：

```bash
    
    nohup java $DWS_ENGINE_MANAGER_JAVA_OPTS $DEBUG_OPTS -cp $HOME/conf:$HOME/lib/* com.webank.wedatasphere.linkis.DataWorkCloudApplication 2>&1 > $DWS_ENGINE_MANAGER_LOG_PATH/linkis-database.out &
    
```

#### 3. 重启database服务
 
#### 4. 打开Intellij IDEA，进入Linkis工程，如下图打开DEBUG：
 
  ![DEBUG截图](https://github.com/WeBankFinTech/Linkis/blob/master/docs/zh_CN/images/ch5/DEBUG%E6%88%AA%E5%9B%BE.png)
  
  请注意：
  
   - Host为Engine实际启动的IP
   - Port为步骤3获取的DEBUG端口
   - Use module classpath需为实际需要DEBUG的模块
   
#### 5. 打断点，开始DEBUG。