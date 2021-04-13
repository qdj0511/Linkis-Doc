安装目录结构
============

Linkis1.0的目录结构与0.X版本相差巨大，0.X的每个微服务都时独立存在的一个根目录，这种目录结构带来的主要的好处是易于区分微服务，便于单个的微服务进行管理，但也存在着一些很明显的问题：

1.  微服务目录过于繁杂，切换目录管理不够方便

2.  没有统一的启动脚本，导致微服务启停比较麻烦

3.  存在大量重复的服务配置，同一个配置经常需要修改多处

4.  存在大量重复的Lib依赖，增大了安装包的体积和依赖冲突的风险

因此Linkis1.0中，我们对安装目录结构做了极大程度的优化和调整，减少了微服务目录的数量，降低了重复依赖的jar包，尽可能复用了配置文件和微服务管理脚本，主要体现在以下几个方面：

1.  不再为每个微服务提供bin文件夹，修改为所有微服务共用。

>   Bin文件夹修改为安装目录，主要用于安装Linkis1.0和检查环境状态，新增sbin目录，为Linkis提供一键启停，依靠变更参数的方式为所有微服务提供单独启停。

1.  不再为每个微服务单独提供conf目录，修改为所有微服务共用。

>   Conf文件夹包含两个方面的内容，一方面是所有微服务共用的配置信息，这类配置信息里面包含了用户根据自身环境自定义配置的信息；另一方面是各个微服务的各自的特殊配置，一般情况下用户不需要自行更改。

1.  不再为每个微服务提供lib文件夹，修改为所有微服务共用

>   Lib文件夹中同样包含了两个方面的内容，一方面是所有微服务都需要的公共依赖；另一方面是各个微服务各自需要的特殊依赖。

1.  不再为每个微服务提供log目录，修改为所有微服务共用

>   Log目录中包含了所有微服务的日志文件。

Linkis1.0简化后的目录结构如下，其中蓝色标注地为用户安装使用时的必定会使用的目录项其他目录项初次使用无特殊情况无需关心：

├── bin 安装目录  
│   ├── checkEnv.sh ── 环境变量检测  
│   ├── checkServices.sh ── 微服务状态检测  
│   ├── common.sh ── 部分公共shell函数  
│   ├── install-io.sh ── 用于安装时的依赖替换  
│   └── **install.sh** ── **Linkis安装的主脚本**  
├── conf 配置目录  
│   ├── **db.sh** ──**mysql数据库配置**  
│   ├── linkis-computation-governance ──计算治理模块配置  
│   ├── **linkis-env.sh** ──**Linkis依赖环境配置**  
│   ├── linkis.properties ──Linkis执行环境配置  
│   ├── linkis-public-enhancements ──公共增强服务模块配置  
│   └── linkis-spring-cloud-services ──SpringCloud环境配置  
├── db 数据库DML和DDL文件目录  
│   ├── linkis\_ddl.sql ──数据库表定义SQL  
│   ├── linkis\_dml.sql ──数据库表初始化SQL  
│   └── module ──包含各个微服务的DML和DDL文件  
├── lib lib目录  
│   ├── linkis-commons ──公共依赖包  
│   ├── linkis-computation-governance ──计算治理模块的lib目录  
│   ├── linkis-engineconn-plugins ──所有引擎插件的lib目录  
│   ├── linkis-public-enhancements ──公共增强服务的lib目录  
│   └── linkis-spring-cloud-services ──SpringCloud的lib目录  
├── logs 日志目录  
│   ├── linkis-computation-governance ──计算治理模块所有微服务日志  
│   ├── linkis-public-enhancements ──公共增强模块所有微服务日志  
│   └── linkis-spring-cloud-services ──SpringCloud模块微服务日志  
├── pid 所有微服务的进程ID  
│   ├── linkis\_cg-engineconnmanager.pid ──引擎管理器微服务  
│   ├── linkis\_cg-engineplugin.pid ──引擎插件微服务  
│   ├── linkis\_cg-entrance.pid ──引擎入口微服务  
│   ├── linkis\_cg-linkismanager.pid ──linkis管理器微服务  
│   ├── linkis\_mg-eureka.pid ──eureka微服务  
│   ├── linkis\_mg-gateway.pid ──gateway微服务  
│   ├── linkis\_ps-bml.pid ──物料库微服务  
│   ├── linkis\_ps-cs.pid ──上下文微服务  
│   ├── linkis\_ps-datasource.pid ──数据源微服务  
│   └── linkis\_ps-publicservice.pid ──公共微服务  
└── sbin 微服务启停脚本目录  
     ├── ext ──各个微服务的启停脚本目录  
     ├── **linkis-daemon.sh** ── **快捷启停、重启单个微服务脚本**  
     ├── **linkis-start-all.sh** ── **一键启动全部微服务脚本**  
     └── **linkis-stop-all.sh** ── **一键停止全部微服务脚本**
