# video-manage-platform

前端页面 http://10.30.143.73:18031

OS系统主要技术与规范：

1、前后端接口字段采用驼峰命名法，例如 appId

2、数据库采用mysql数据库，结构化存储信息

3、开发语言主要采用java，开发框架主要使用springboot，mybatis,由于目前没有跨服务调用，目前没有开放spring cloud服务，只采用SLB代理服务，前端通过代理访问后端接口。。

4、文件服务，目前主要使用了oss，用户也可以实现开放接口，采用其他的文件存储平台。。。

5、权限管理模块部分采用了shiro开源组件

6、对外api采用RSA非对称加密的方式

7、动态定时器采用开源quartz组件

8、登录的分布式session放在redis上，支持集群部署

9、mqtt服务主要使用的是emqtt



