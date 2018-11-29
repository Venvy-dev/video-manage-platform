# video-manage-platform


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



后端部署指南

一、准备工作
1.1 Java
OS服务端：1.8+
必须选用Java 1.8+。

建议1.8.0_111以上版本

在配置好后，可以通过如下命令检查：

java -version
 

1.2 MySQL
版本要求：5.6.5+
1.3、redis
由于用户token管理采用了统一分布式管理，存储在redis上，因此依赖 redis缓存，此处不再详述redis的搭建，可以自行搜索相关教程。

1.4、emq
emq是基于Erlang/OTP语言平台开发,百万级分布式开源物联网MQTT消息服务器,可以自行搭建，但是由于服务负荷很高，video++会提供相应的服务，具体可以联系我司，联系方式：400-8089-578

1.5、oss
阿里云对象存储服务(Object Storage Service,简称OSS),是阿里云对外提供的海量、安全、低成本、高可靠的云存储服务。主要是文件存储使用，用户可以自行实现CommonFileServer接口，使用自己公司内部的文件服务器接口。

1.6、环境
分布式部署需要事先确定部署的环境以及部署方式。

OS目前支持以下环境：

DEV
开发环境
TEST
测试环境
PRE
预发环境
PRO
生产环境
二、部署步骤

部署步骤共两步：

创建数据库
OS服务端依赖于MySQL数据库，所以需要事先创建并完成初始化
获取源码，部署OS服务端
获取代码，打包之后，就可以部署到公司的测试和生产环境了
2.1 创建数据库
附件中OS所有的脚本DDL.sql以及文件OS所有的脚本DML.sql，两个脚本文件。

2.2 获取源代码和配置部署
1、获取源码地址：http://****.com

2、首先更改一下源码中的数据库的配置，该项目的所有的配置都在videoservice模块的resources目录下，

更改

spring:
   datasource:
 url: 
 username:
 password:
更新为部署好的数据库的配置。

3、部署emqtt，emqtt下载地址是：http://www.emqtt.com/，目前最新版本号是3.0，直接部署即可。

部署完了之后，需要更新项目的配置文件中

mqtt:
 username:
 password:
 hostUrl: 
这几个配置项。
4、部署redis(具体部署步骤此处不再详述）

部署完成之后，需要更新

redis:
 shiro:
 host: 
    port: 
    timeout: 
 password:
这几个配置项，根据具体的配置，更改这些配置文件。

5、部署oss和CDN

oss是阿里的文件服务，如果用户不使用，也可以实现文件服务接口，另外还需要配置CDN，否则移动端下载文件可能会比较缓慢。

以上服务开通之后，需要更改一下配置：

video:
 oss:
 endpoint: 
    key: 
    secret: 
    bucketName:
 cdn: 
CDN服务，我司video++会提供相应的服务，具体可以联系我司，联系方式：400-8089-578

6、打包

打包方式可以使用maven，主要两个jar包，一个是videoportal-1.0.jar（后端控制台），还有一个是videopublicapi-1.0.jar（提供给移动端的接口）。。。。

 

三、运行参数

java -Xms4096m -Xmx4096m -Xss256k -XX:MetaspaceSize=128m -XX:MaxMetaspaceSize=384m -XX:NewSize=1536m -XX:MaxNewSize=1536m -XX:SurvivorRatio=22 –Denv = dev –jar xxx.jar




