## 如何定义新异常？

- 自定义的异常都必须继承自 WarnException、ErrorException 或 FatalException 之一

- 自定义的异常必须包含错误码和错误描述，如有必要，也可将发生异常的ip地址和进程端口封装到异常当中

- 慎用WarnException！WarnException抛出来的异常，如果在Restful和RPC的Receiver端被捕获，不会给前端或sender端抛出执行失败，而是只返回一条警告信息！

- WARNException的异常级别为1，ErrorException的异常级别为2，FatalException的异常级别为3

|异常类|	所在服务|   错误码|    错误描述|
|:----  |:---   |:---   |:---   |
|DWCException|	common| 无|	顶级父类，继承自Exception,不允许直接继承|
|DWCRuntimeException|	common|	无|	顶级父类，继承自RuntimeException，不允许直接继承|
|WarnException|	common|	无|	次级父类，继承自DWCRuntimeException。提示级的异常，必须直接或间接继承该类|
|ErrorException|	common|	无|	次级父类，继承自DWCException。错误级的异常，必须直接或间接继承该类|
|FatalException|	common|	无|	次级父类，继承自DWCException。致命级的异常，必须直接或间接继承该类|


## 模块异常规范

- Linkis架构错误码以10000开始

- 资源管理服务错误码以11000开始

- 统一入口错误码以20000开始

- 引擎管理器错误码以30000开始

- 引擎错误码以40000开始

- Storage错误码以50000开始
