# cloud-eureka 
    服务注册中心，所有的服务都将在注册中心进行注册，由注册中心进行管理，其他服务
    进行服务间调用的时候也需要到服务注册中心进行查询可用服务，然后再进行访问
##  这只是一个简单的注册中心示例
    我们首先要在我们的系统中配置主机名映射：
    windows ： C:/windows/system32/drivers/etc 下有一个hosts文件，需要管理员权限打开
    linux/unix： 编辑 /etc/hosts 文件
    
    在文件中加入这样一句话：
    127.0.0.1   eureka7001.com  
    然后保存  
    
    这样做的目的是为了模拟分布式（后面我们会用到），当我们在hosts文件中配置这样一句话以后，我们访问eureka7001.com
    时会自动映射ip地址为 127.0.0.1  （当然如果你有多台服务器或电脑的话，也可以不配置hosts，使用真实的分布式模式，
    利用ip地址进行访问）
    
    哦对了，不要忘记在启动类上标注@EnableEurekaServer注解来启动Eureka服务
    
    若启动成功的话  访问 eureka7001.com:7001 可以看到Eureka的界面
    （仅限本示例项目，你应该访问的是Eureka所在的服务器+端口）
    
>   配置好Eureka的Server端，我们就需要将服务注册进来，所以我们还要对我们的服务进行一些修改

    由于Eureka是 C/S 架构，所以我们需要在服务端加入Eureka客户端的依赖并启动
    若启动成功，我们在Eureka的界面应该可以看到我们注册的服务
    
>   至此，我们的Eureka服务端已经搭建好，并且服务端通过Eureka Client可以进行注册
    其实Eureka中的知识还有很多 例如自我保护模式等 具体请参考  [官方文档](https://cloud.spring.io/spring-cloud-netflix/spring-cloud-netflix.html)

####  关于服务注册的说明
      每一个服务都会通过Eureka客户端注册到Eureka注册中心，我们在注册中心可以通过服务的实例名称
      获取到一个服务实例。但是在实际中并不是仅仅只有一个服务实例，Eureka 就是通过微服务名称来判
      断一个服务实例的（即相同服务名称才会被认为是同一个服务实例）
      所以我们在进行服务注册的时候，必须保证同一服务名称相同 不同服务名称不同。


# 第二阶段 Eureka集群
    eureka集群是为了防止单一结点的Eureka服务挂掉导致整个的系统崩溃，提高系统的可用性。
    这里我们使用模拟分布式的配置，使用三个Eureeka集群分别跑在三个端口上（实际中应该是在不同的服务器上）
    三个结点配置基本相同，这三个结点分别是：
    
   [cloud-eureka-7001](https://github.com/811105717/SpringCloud/tree/master/cloud-eureka-7001)    
   [cloud-eureka-7002](https://github.com/811105717/SpringCloud/tree/master/cloud-eureka-7002) 和 
   [eureka-cloud-7003](https://github.com/811105717/SpringCloud/tree/master/cloud-eureka-7003)
   
   具体的配置方式也很简单：
   在第一阶段的配置基础上，修改eureka.client.service-url.defaultZone
   在其中配置上除自己外的其他eureka服务结点的地址即可
 
