> 登录文档

## 1.对接LDAP服务
进入/conf/linkis-spring-cloud-services/linkis-mg-gateway目录，执行命令：

```bash
    vim linkis-server.properties
```    

添加LDAP相关配置：
```bash
wds.linkis.ldap.proxy.url=ldap://127.0.0.1:389/#您的LDAP服务URL
wds.linkis.ldap.proxy.baseDN=dc=webank,dc=com#您的LDAP服务的配置    
```    
    
## 2.如何实现免登录

进入/conf/linkis-spring-cloud-services/linkis-mg-gateway目录，执行命令：

```bash
    vim linkis-server.properties
```
    
    
将测试模式打开，参数如下：

```properties
    wds.linkis.test.mode=true   # 打开测试模式
    wds.linkis.test.user=hadoop  # 指定测试模式下，所有请求都代理给哪个用户
```

## 3.登录接口汇总

我们提供以下几个与登录相关的接口：

 - [登录]

 - [登出]

 - [心跳]
 

## 4.接口详解

### 1).登录

- 接口 `/api/rest_j/v1/user/login`

- 提交方式 `POST`

```json
      {
        "userName": "",
        "password": ""
      }
```

- 返回示例

```json
    {
        "method": null,
        "status": 0,
        "message": "login successful(登录成功)！",
        "data": {
            "isAdmin": false,
            "userName": ""
        }
     }
```

### 2).登出

- 接口 `/api/rest_j/v1/user/logout`

- 提交方式 `POST`

  无参数

- 返回示例

```json
    {
        "method": "/api/rest_j/v1/user/logout",
        "status": 0,
        "message": "退出登录成功！"
    }
```

### 3).心跳

- 接口 `/api/rest_j/v1/user/heartbeat`

- 提交方式 `POST`

  无参数

- 返回示例

```json
    {
         "method": "/api/rest_j/v1/user/heartbeat",
         "status": 0,
         "message": "维系心跳成功！"
    }
```