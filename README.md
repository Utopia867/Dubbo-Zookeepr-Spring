# Dubbo-Zookeepr-Spring 

本例子是在windows7下运行，Linux的运行效果未展示。

本例子使用单例模式，未配置Zookeeper集群，方便入门学习。

学习本教材的你应该有一定的java基础，为了方便我在例子中使用Maven构建工具，不需要找jar包的痛苦了

由于Zookeeper太大，我把zookeeper压缩了，解压后，修改<code>conf</code>下的配置文件下的
<code>zoo.cfg</code>文件，在zookeeper官网下载到zookeeper时是没有这个文件的，将<code>zoo_sample.cfg</code>复制一个为<code>zoo.cfg</code>的文件即可。

```config
# The number of milliseconds of each tick
tickTime=2000
# The number of ticks that the initial 
# synchronization phase can take
initLimit=10
# The number of ticks that can pass between 
# sending a request and getting an acknowledgement
syncLimit=5
# the directory where the snapshot is stored.
# do not use /tmp for storage, /tmp here is just 
# example sakes.
dataDir=D:\\zookeeper\\data
dataLogDir=D:\\zookeeper\\log
#dataDir=/tmp/zookeeper
# the port at which the clients will connect
clientPort=2181
# the maximum number of client connections.
# increase this if you need to handle more clients
#maxClientCnxns=60
#
# Be sure to read the maintenance section of the 
# administrator guide before turning on autopurge.
#
# http://zookeeper.apache.org/doc/current/zookeeperAdmin.html#sc_maintenance
#
# The number of snapshots to retain in dataDir
#autopurge.snapRetainCount=3
# Purge task interval in hours
# Set to "0" to disable auto purge feature
#autopurge.purgeInterval=1
```


修改<code>dataDir</code>和<code>dataLogDir</code>即可，修改后需要创建相应的文件夹，及存储日志和数据，否则启动zookeeper会报错，在zookeeper文件夹下的bin文件夹下双击<code>zkServer.cmd</code>可以启动zookeeper，启动成功后可以看到


```log
ent:java.io.tmpdir=C:\Users\WANGGU~1\AppData\Local\Temp\
2017-08-31 19:29:30,589 [myid:] - INFO  [main:Environment@100] - Server environment:java.compiler=<NA>
2017-08-31 19:29:30,590 [myid:] - INFO  [main:Environment@100] - Server environment:os.name=Windows 7
2017-08-31 19:29:30,590 [myid:] - INFO  [main:Environment@100] - Server environment:os.arch=amd64
2017-08-31 19:29:30,591 [myid:] - INFO  [main:Environment@100] - Server environment:os.version=6.1
2017-08-31 19:29:30,592 [myid:] - INFO  [main:Environment@100] - Server environment:user.name=wangguoqi
2017-08-31 19:29:30,592 [myid:] - INFO  [main:Environment@100] - Server environment:user.home=C:\Users\wangguoqi
2017-08-31 19:29:30,592 [myid:] - INFO  [main:Environment@100] - Server environment:user.dir=D:\WorkSpace\EclipseWorkspace\Dubbo+Zookeepr+Spring\zookeeper\bin
2017-08-31 19:29:30,599 [myid:] - INFO  [main:ZooKeeperServer@755] - tickTime set to 2000
2017-08-31 19:29:30,599 [myid:] - INFO  [main:ZooKeeperServer@764] - minSessionTimeout set to -1
2017-08-31 19:29:30,599 [myid:] - INFO  [main:ZooKeeperServer@773] - maxSessionTimeout set to -1
2017-08-31 19:29:30,618 [myid:] - INFO  [main:NIOServerCnxnFactory@94] - binding to port 0.0.0.0/0.0.0.0:2181
```


这就证明zookeeper启动成功，不要关闭，进行下一步


下面开始进如生产者和消费者的步骤，首先启动生产者也就是<code>Provider</code>的<code>main</code>方法,
```java
System.in.read(); // 按任意键退出
```
是为了防止程序运行后直接退出

运行后可以在控制台看到
```log
2017-09-01 12:52:06,195 [main] INFO org.springframework.context.support.ClassPathXmlApplicationContext - Refreshing org.springframework.context.support.ClassPathXmlApplicationContext@7862d875: startup date [Fri Sep 01 12:52:06 CST 2017]; root of context hierarchy
2017-09-01 12:52:06,316 [main] INFO org.springframework.beans.factory.xml.XmlBeanDefinitionReader - Loading XML bean definitions from class path resource [dubbo-demo-provider.xml]
2017-09-01 12:52:06,531 [main] INFO com.alibaba.dubbo.common.logger.LoggerFactory - using logger: com.alibaba.dubbo.common.logger.log4j.Log4jLoggerAdapter
2017-09-01 12:52:06,673 [main] INFO org.springframework.beans.factory.support.DefaultListableBeanFactory - Pre-instantiating singletons in org.springframework.beans.factory.support.DefaultListableBeanFactory@6af4bbde: defining beans [demo-provider,com.alibaba.dubbo.config.RegistryConfig,dubbo,com.alibaba.dubbo.demo.DemoService,demoService]; root of factory hierarchy
2017-09-01 12:52:07,075 [main] INFO com.alibaba.dubbo.config.AbstractConfig -  [DUBBO] The service ready on spring started. service: com.alibaba.dubbo.demo.DemoService, dubbo version: 2.5.3, current host: 127.0.0.1
2017-09-01 12:52:07,194 [main] INFO com.alibaba.dubbo.config.AbstractConfig -  [DUBBO] Export dubbo service com.alibaba.dubbo.demo.DemoService to local registry, dubbo version: 2.5.3, current host: 127.0.0.1
2017-09-01 12:52:07,194 [main] INFO com.alibaba.dubbo.config.AbstractConfig -  [DUBBO] Export dubbo service com.alibaba.dubbo.demo.DemoService to url dubbo://169.254.41.206:20880/com.alibaba.dubbo.demo.DemoService?anyhost=true&application=demo-provider&dubbo=2.5.3&interface=com.alibaba.dubbo.demo.DemoService&methods=sayHello&pid=10132&side=provider&timestamp=1504241527110, dubbo version: 2.5.3, current host: 127.0.0.1
2017-09-01 12:52:07,195 [main] INFO com.alibaba.dubbo.config.AbstractConfig -  [DUBBO] Register dubbo service com.alibaba.dubbo.demo.DemoService url dubbo://169.254.41.206:20880/com.alibaba.dubbo.demo.DemoService?anyhost=true&application=demo-provider&dubbo=2.5.3&interface=com.alibaba.dubbo.demo.DemoService&methods=sayHello&pid=10132&side=provider&timestamp=1504241527110 to registry registry://127.0.0.1:2181/com.alibaba.dubbo.registry.RegistryService?application=demo-provider&dubbo=2.5.3&pid=10132&registry=zookeeper&timestamp=1504241527094, dubbo version: 2.5.3, current host: 127.0.0.1
2017-09-01 12:52:07,396 [main] INFO com.alibaba.dubbo.remoting.transport.AbstractServer -  [DUBBO] Start NettyServer bind /0.0.0.0:20880, export /169.254.41.206:20880, dubbo version: 2.5.3, current host: 127.0.0.1
2017-09-01 12:52:07,421 [main] INFO com.alibaba.dubbo.registry.zookeeper.ZookeeperRegistry -  [DUBBO] Load registry store file C:\Users\wangguoqi\.dubbo\dubbo-registry-127.0.0.1.cache, data: {com.alibaba.dubbo.demo.DemoService=dubbo://169.254.45.136:20880/com.alibaba.dubbo.demo.DemoService?anyhost=true&application=demo-provider&dubbo=2.5.3&interface=com.alibaba.dubbo.demo.DemoService&methods=sayHello&pid=11652&side=provider&timestamp=1504184044772 empty://169.254.45.136/com.alibaba.dubbo.demo.DemoService?application=demo-consumer&category=configurators&dubbo=2.5.3&interface=com.alibaba.dubbo.demo.DemoService&methods=sayHello&pid=11508&side=consumer&timestamp=1504184060532 empty://169.254.45.136/com.alibaba.dubbo.demo.DemoService?application=demo-consumer&category=routers&dubbo=2.5.3&interface=com.alibaba.dubbo.demo.DemoService&methods=sayHello&pid=11508&side=consumer&timestamp=1504184060532, com.unj.dubbotest.provider.DemoService=dubbo://169.254.45.136:20880/com.unj.dubbotest.provider.DemoService?anyhost=true&application=xixi_provider&dubbo=2.5.2&interface=com.unj.dubbotest.provider.DemoService&methods=sayHello,getUsers&pid=11552&side=provider&timestamp=1504174505764 empty://169.254.45.136/com.unj.dubbotest.provider.DemoService?application=hehe_consumer&category=configurators&dubbo=2.5.2&interface=com.unj.dubbotest.provider.DemoService&methods=sayHello,getUsers&pid=10344&side=consumer&timestamp=1504174515360 empty://169.254.45.136/com.unj.dubbotest.provider.DemoService?application=hehe_consumer&category=routers&dubbo=2.5.2&interface=com.unj.dubbotest.provider.DemoService&methods=sayHello,getUsers&pid=10344&side=consumer&timestamp=1504174515360}, dubbo version: 2.5.3, current host: 127.0.0.1
2017-09-01 12:52:07,439 [ZkClient-EventThread-19-127.0.0.1:2181] INFO org.I0Itec.zkclient.ZkEventThread - Starting ZkClient event thread.
2017-09-01 12:52:07,460 [main] INFO org.apache.zookeeper.ZooKeeper - Client environment:zookeeper.version=3.4.6-1569965, built on 02/20/2014 09:09 GMT
2017-09-01 12:52:07,460 [main] INFO org.apache.zookeeper.ZooKeeper - Client environment:host.name=wangguoqi-PC.HDDOMAIN.CN
2017-09-01 12:52:07,460 [main] INFO org.apache.zookeeper.ZooKeeper - Client environment:java.version=1.7.0_67
2017-09-01 12:52:07,460 [main] INFO org.apache.zookeeper.ZooKeeper - Client environment:java.vendor=Oracle Corporation
2017-09-01 12:52:07,460 [main] INFO org.apache.zookeeper.ZooKeeper - Client environment:java.home=D:\WorkSoftware\JAVA\jdk1.7\jre
2017-09-01 12:52:07,460 [main] INFO org.apache.zookeeper.ZooKeeper - Client environment:java.class.path=D:\WorkSpace\EclipseWorkspace\Dubbo+Zookeepr+Spring\provider\target\classes;D:\WorkSoftware\JAVA\Maven\Maven\Repository\org\springframework\spring-context\3.2.9.RELEASE\spring-context-3.2.9.RELEASE.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\org\springframework\spring-aop\3.2.9.RELEASE\spring-aop-3.2.9.RELEASE.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\aopalliance\aopalliance\1.0\aopalliance-1.0.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\org\springframework\spring-expression\3.2.9.RELEASE\spring-expression-3.2.9.RELEASE.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\org\springframework\spring-core\3.2.9.RELEASE\spring-core-3.2.9.RELEASE.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\commons-logging\commons-logging\1.1.3\commons-logging-1.1.3.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\org\springframework\spring-beans\3.2.9.RELEASE\spring-beans-3.2.9.RELEASE.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\log4j\log4j\1.2.17\log4j-1.2.17.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\org\apache\zookeeper\zookeeper\3.4.6\zookeeper-3.4.6.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\org\slf4j\slf4j-api\1.6.1\slf4j-api-1.6.1.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\org\slf4j\slf4j-log4j12\1.6.1\slf4j-log4j12-1.6.1.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\jline\jline\0.9.94\jline-0.9.94.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\junit\junit\3.8.1\junit-3.8.1.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\io\netty\netty\3.7.0.Final\netty-3.7.0.Final.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\com\alibaba\dubbo\2.5.3\dubbo-2.5.3.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\org\springframework\spring\2.5.6.SEC03\spring-2.5.6.SEC03.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\org\javassist\javassist\3.15.0-GA\javassist-3.15.0-GA.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\org\jboss\netty\netty\3.2.5.Final\netty-3.2.5.Final.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\com\101tec\zkclient\0.2\zkclient-0.2.jar
2017-09-01 12:52:07,460 [main] INFO org.apache.zookeeper.ZooKeeper - Client environment:java.library.path=D:\WorkSoftware\JAVA\jdk1.7\bin;C:\Windows\Sun\Java\bin;C:\Windows\system32;C:\Windows;D:/WorkSoftware/JAVA/jdk1.7/bin/../jre/bin/server;D:/WorkSoftware/JAVA/jdk1.7/bin/../jre/bin;D:/WorkSoftware/JAVA/jdk1.7/bin/../jre/lib/amd64;D:\WorkSoftware\Oracle\bin;C:\Windows\system32;C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\;D:\WorkSoftware\Git\cmd;D:\WorkSoftware\JAVA\jdk1.7\bin;D:\WorkSoftware\JAVA\jdk1.7\jre\bin;D:\WorkSoftware\JAVA\Maven\apache-maven-3.0.5\bin;D:\WorkSoftware\TortoiseSVN\bin;D:\WorkSoftware\SQLServer\Microsoft SQL Server Share\100\Tools\Binn\;D:\WorkSoftware\SQLServer\Microsoft SQL Server\100\Tools\Binn\;D:\WorkSoftware\SQLServer\Microsoft SQL Server\100\DTS\Binn\;D:\WorkSoftware\SQLServer\Microsoft SQL Server Share\100\Tools\Binn\VSShell\Common7\IDE\;C:\Program Files (x86)\Microsoft Visual Studio 9.0\Common7\IDE\PrivateAssemblies\;D:\WorkSoftware\SQLServer\Microsoft SQL Server Share\100\DTS\Binn\;D:\WorkSoftware\JAVA\Ant\apache-ant-1.8.4\bin;D:\WorkSoftware\qampp\Data_lib\perl\bin;D:\WorkSoftware\qampp\apache\bin;D:\WorkSoftware\qampp\php;D:\WorkSoftware\qampp\svnserver\Subversion\bin;D:\WorkSoftware\qampp\mysql\bin;D:\WorkSoftware\Nodejs\nodejs\;D:\WorkSoftware\SenchaCmd\Cmd;D:\WorkSoftware\TortoiseGit\bin;D:\WorkSoftware\Python\Python36\Scripts\;D:\WorkSoftware\Python\Python36\;C:\Users\wangguoqi\AppData\Roaming\npm;D:\WorkSoftware\VSCode\Microsoft VS Code\bin;D:\WorkSoftware\DockerToolbox;C:\Users\wangguoqi\AppData\Local\Programs\Fiddler;D:\WorkSoftware\Eclipse\jee-neon\eclipse;;.
2017-09-01 12:52:07,460 [main] INFO org.apache.zookeeper.ZooKeeper - Client environment:java.io.tmpdir=C:\Users\WANGGU~1\AppData\Local\Temp\
2017-09-01 12:52:07,460 [main] INFO org.apache.zookeeper.ZooKeeper - Client environment:java.compiler=<NA>
2017-09-01 12:52:07,460 [main] INFO org.apache.zookeeper.ZooKeeper - Client environment:os.name=Windows 7
2017-09-01 12:52:07,460 [main] INFO org.apache.zookeeper.ZooKeeper - Client environment:os.arch=amd64
2017-09-01 12:52:07,460 [main] INFO org.apache.zookeeper.ZooKeeper - Client environment:os.version=6.1
2017-09-01 12:52:07,460 [main] INFO org.apache.zookeeper.ZooKeeper - Client environment:user.name=wangguoqi
2017-09-01 12:52:07,460 [main] INFO org.apache.zookeeper.ZooKeeper - Client environment:user.home=C:\Users\wangguoqi
2017-09-01 12:52:07,460 [main] INFO org.apache.zookeeper.ZooKeeper - Client environment:user.dir=D:\WorkSpace\EclipseWorkspace\Dubbo+Zookeepr+Spring\provider
2017-09-01 12:52:07,461 [main] INFO org.apache.zookeeper.ZooKeeper - Initiating client connection, connectString=127.0.0.1:2181 sessionTimeout=30000 watcher=org.I0Itec.zkclient.ZkClient@28e0a7ea
2017-09-01 12:52:07,500 [main-SendThread(127.0.0.1:2181)] INFO org.apache.zookeeper.ClientCnxn - Opening socket connection to server 127.0.0.1/127.0.0.1:2181. Will not attempt to authenticate using SASL (unknown error)
2017-09-01 12:52:07,501 [main-SendThread(127.0.0.1:2181)] INFO org.apache.zookeeper.ClientCnxn - Socket connection established to 127.0.0.1/127.0.0.1:2181, initiating session
2017-09-01 12:52:07,581 [main-SendThread(127.0.0.1:2181)] INFO org.apache.zookeeper.ClientCnxn - Session establishment complete on server 127.0.0.1/127.0.0.1:2181, sessionid = 0x15e3bc801b10000, negotiated timeout = 30000
2017-09-01 12:52:07,586 [main-EventThread] INFO org.I0Itec.zkclient.ZkClient - zookeeper state changed (SyncConnected)
2017-09-01 12:52:07,589 [main] INFO com.alibaba.dubbo.registry.zookeeper.ZookeeperRegistry -  [DUBBO] Register: dubbo://169.254.41.206:20880/com.alibaba.dubbo.demo.DemoService?anyhost=true&application=demo-provider&dubbo=2.5.3&interface=com.alibaba.dubbo.demo.DemoService&methods=sayHello&pid=10132&side=provider&timestamp=1504241527110, dubbo version: 2.5.3, current host: 127.0.0.1
2017-09-01 12:52:07,705 [main] INFO com.alibaba.dubbo.registry.zookeeper.ZookeeperRegistry -  [DUBBO] Subscribe: provider://169.254.41.206:20880/com.alibaba.dubbo.demo.DemoService?anyhost=true&application=demo-provider&category=configurators&check=false&dubbo=2.5.3&interface=com.alibaba.dubbo.demo.DemoService&methods=sayHello&pid=10132&side=provider&timestamp=1504241527110, dubbo version: 2.5.3, current host: 127.0.0.1
2017-09-01 12:52:07,773 [main] INFO com.alibaba.dubbo.registry.zookeeper.ZookeeperRegistry -  [DUBBO] Notify urls for subscribe url provider://169.254.41.206:20880/com.alibaba.dubbo.demo.DemoService?anyhost=true&application=demo-provider&category=configurators&check=false&dubbo=2.5.3&interface=com.alibaba.dubbo.demo.DemoService&methods=sayHello&pid=10132&side=provider&timestamp=1504241527110, urls: [empty://169.254.41.206:20880/com.alibaba.dubbo.demo.DemoService?anyhost=true&application=demo-provider&category=configurators&check=false&dubbo=2.5.3&interface=com.alibaba.dubbo.demo.DemoService&methods=sayHello&pid=10132&side=provider&timestamp=1504241527110], dubbo version: 2.5.3, current host: 127.0.0.1
```
此时证明<code>Provider</code>已经注册到zookeeper中

下一步启动消费者程序及<code>Consumer</code>的<code>main</code>方法，可以在控制台看到如下内容证明成功获取生产者中的数据
```log
2017-09-01 12:57:24,786 [main] INFO org.springframework.context.support.ClassPathXmlApplicationContext - Refreshing org.springframework.context.support.ClassPathXmlApplicationContext@32a6cf28: startup date [Fri Sep 01 12:57:24 CST 2017]; root of context hierarchy
2017-09-01 12:57:24,824 [main] INFO org.springframework.beans.factory.xml.XmlBeanDefinitionReader - Loading XML bean definitions from class path resource [dubbo-demo-consumer.xml]
2017-09-01 12:57:24,956 [main] INFO com.alibaba.dubbo.common.logger.LoggerFactory - using logger: com.alibaba.dubbo.common.logger.log4j.Log4jLoggerAdapter
2017-09-01 12:57:24,993 [main] INFO org.springframework.beans.factory.support.DefaultListableBeanFactory - Pre-instantiating singletons in org.springframework.beans.factory.support.DefaultListableBeanFactory@3d1bb20d: defining beans [demo-consumer,com.alibaba.dubbo.config.RegistryConfig,demoService]; root of factory hierarchy
2017-09-01 12:57:25,271 [main] INFO com.alibaba.dubbo.registry.zookeeper.ZookeeperRegistry -  [DUBBO] Load registry store file C:\Users\wangguoqi\.dubbo\dubbo-registry-127.0.0.1.cache, data: {com.alibaba.dubbo.demo.DemoService=empty://169.254.41.206:20880/com.alibaba.dubbo.demo.DemoService?anyhost=true&application=demo-provider&category=configurators&check=false&dubbo=2.5.3&interface=com.alibaba.dubbo.demo.DemoService&methods=sayHello&pid=10132&side=provider&timestamp=1504241527110, com.unj.dubbotest.provider.DemoService=dubbo://169.254.45.136:20880/com.unj.dubbotest.provider.DemoService?anyhost=true&application=xixi_provider&dubbo=2.5.2&interface=com.unj.dubbotest.provider.DemoService&methods=sayHello,getUsers&pid=11552&side=provider&timestamp=1504174505764 empty://169.254.45.136/com.unj.dubbotest.provider.DemoService?application=hehe_consumer&category=configurators&dubbo=2.5.2&interface=com.unj.dubbotest.provider.DemoService&methods=sayHello,getUsers&pid=10344&side=consumer&timestamp=1504174515360 empty://169.254.45.136/com.unj.dubbotest.provider.DemoService?application=hehe_consumer&category=routers&dubbo=2.5.2&interface=com.unj.dubbotest.provider.DemoService&methods=sayHello,getUsers&pid=10344&side=consumer&timestamp=1504174515360}, dubbo version: 2.5.3, current host: 127.0.0.1
2017-09-01 12:57:25,281 [ZkClient-EventThread-11-127.0.0.1:2181] INFO org.I0Itec.zkclient.ZkEventThread - Starting ZkClient event thread.
2017-09-01 12:57:25,290 [main] INFO org.apache.zookeeper.ZooKeeper - Client environment:zookeeper.version=3.4.6-1569965, built on 02/20/2014 09:09 GMT
2017-09-01 12:57:25,290 [main] INFO org.apache.zookeeper.ZooKeeper - Client environment:host.name=wangguoqi-PC.HDDOMAIN.CN
2017-09-01 12:57:25,290 [main] INFO org.apache.zookeeper.ZooKeeper - Client environment:java.version=1.7.0_67
2017-09-01 12:57:25,290 [main] INFO org.apache.zookeeper.ZooKeeper - Client environment:java.vendor=Oracle Corporation
2017-09-01 12:57:25,290 [main] INFO org.apache.zookeeper.ZooKeeper - Client environment:java.home=D:\WorkSoftware\JAVA\jdk1.7\jre
2017-09-01 12:57:25,290 [main] INFO org.apache.zookeeper.ZooKeeper - Client environment:java.class.path=D:\WorkSpace\EclipseWorkspace\Dubbo+Zookeepr+Spring\consumer\target\classes;D:\WorkSoftware\JAVA\Maven\Maven\Repository\org\springframework\spring-context\3.2.9.RELEASE\spring-context-3.2.9.RELEASE.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\org\springframework\spring-aop\3.2.9.RELEASE\spring-aop-3.2.9.RELEASE.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\aopalliance\aopalliance\1.0\aopalliance-1.0.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\org\springframework\spring-expression\3.2.9.RELEASE\spring-expression-3.2.9.RELEASE.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\org\springframework\spring-core\3.2.9.RELEASE\spring-core-3.2.9.RELEASE.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\commons-logging\commons-logging\1.1.3\commons-logging-1.1.3.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\org\springframework\spring-beans\3.2.9.RELEASE\spring-beans-3.2.9.RELEASE.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\log4j\log4j\1.2.17\log4j-1.2.17.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\org\apache\zookeeper\zookeeper\3.4.6\zookeeper-3.4.6.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\org\slf4j\slf4j-api\1.6.1\slf4j-api-1.6.1.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\org\slf4j\slf4j-log4j12\1.6.1\slf4j-log4j12-1.6.1.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\jline\jline\0.9.94\jline-0.9.94.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\junit\junit\3.8.1\junit-3.8.1.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\io\netty\netty\3.7.0.Final\netty-3.7.0.Final.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\com\alibaba\dubbo\2.5.3\dubbo-2.5.3.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\org\springframework\spring\2.5.6.SEC03\spring-2.5.6.SEC03.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\org\javassist\javassist\3.15.0-GA\javassist-3.15.0-GA.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\org\jboss\netty\netty\3.2.5.Final\netty-3.2.5.Final.jar;D:\WorkSoftware\JAVA\Maven\Maven\Repository\com\101tec\zkclient\0.2\zkclient-0.2.jar
2017-09-01 12:57:25,290 [main] INFO org.apache.zookeeper.ZooKeeper - Client environment:java.library.path=D:\WorkSoftware\JAVA\jdk1.7\bin;C:\Windows\Sun\Java\bin;C:\Windows\system32;C:\Windows;D:/WorkSoftware/JAVA/jdk1.7/bin/../jre/bin/server;D:/WorkSoftware/JAVA/jdk1.7/bin/../jre/bin;D:/WorkSoftware/JAVA/jdk1.7/bin/../jre/lib/amd64;D:\WorkSoftware\Oracle\bin;C:\Windows\system32;C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\;D:\WorkSoftware\Git\cmd;D:\WorkSoftware\JAVA\jdk1.7\bin;D:\WorkSoftware\JAVA\jdk1.7\jre\bin;D:\WorkSoftware\JAVA\Maven\apache-maven-3.0.5\bin;D:\WorkSoftware\TortoiseSVN\bin;D:\WorkSoftware\SQLServer\Microsoft SQL Server Share\100\Tools\Binn\;D:\WorkSoftware\SQLServer\Microsoft SQL Server\100\Tools\Binn\;D:\WorkSoftware\SQLServer\Microsoft SQL Server\100\DTS\Binn\;D:\WorkSoftware\SQLServer\Microsoft SQL Server Share\100\Tools\Binn\VSShell\Common7\IDE\;C:\Program Files (x86)\Microsoft Visual Studio 9.0\Common7\IDE\PrivateAssemblies\;D:\WorkSoftware\SQLServer\Microsoft SQL Server Share\100\DTS\Binn\;D:\WorkSoftware\JAVA\Ant\apache-ant-1.8.4\bin;D:\WorkSoftware\qampp\Data_lib\perl\bin;D:\WorkSoftware\qampp\apache\bin;D:\WorkSoftware\qampp\php;D:\WorkSoftware\qampp\svnserver\Subversion\bin;D:\WorkSoftware\qampp\mysql\bin;D:\WorkSoftware\Nodejs\nodejs\;D:\WorkSoftware\SenchaCmd\Cmd;D:\WorkSoftware\TortoiseGit\bin;D:\WorkSoftware\Python\Python36\Scripts\;D:\WorkSoftware\Python\Python36\;C:\Users\wangguoqi\AppData\Roaming\npm;D:\WorkSoftware\VSCode\Microsoft VS Code\bin;D:\WorkSoftware\DockerToolbox;C:\Users\wangguoqi\AppData\Local\Programs\Fiddler;D:\WorkSoftware\Eclipse\jee-neon\eclipse;;.
2017-09-01 12:57:25,290 [main] INFO org.apache.zookeeper.ZooKeeper - Client environment:java.io.tmpdir=C:\Users\WANGGU~1\AppData\Local\Temp\
2017-09-01 12:57:25,290 [main] INFO org.apache.zookeeper.ZooKeeper - Client environment:java.compiler=<NA>
2017-09-01 12:57:25,290 [main] INFO org.apache.zookeeper.ZooKeeper - Client environment:os.name=Windows 7
2017-09-01 12:57:25,290 [main] INFO org.apache.zookeeper.ZooKeeper - Client environment:os.arch=amd64
2017-09-01 12:57:25,290 [main] INFO org.apache.zookeeper.ZooKeeper - Client environment:os.version=6.1
2017-09-01 12:57:25,290 [main] INFO org.apache.zookeeper.ZooKeeper - Client environment:user.name=wangguoqi
2017-09-01 12:57:25,290 [main] INFO org.apache.zookeeper.ZooKeeper - Client environment:user.home=C:\Users\wangguoqi
2017-09-01 12:57:25,290 [main] INFO org.apache.zookeeper.ZooKeeper - Client environment:user.dir=D:\WorkSpace\EclipseWorkspace\Dubbo+Zookeepr+Spring\consumer
2017-09-01 12:57:25,291 [main] INFO org.apache.zookeeper.ZooKeeper - Initiating client connection, connectString=127.0.0.1:2181 sessionTimeout=30000 watcher=org.I0Itec.zkclient.ZkClient@5b073e42
2017-09-01 12:57:25,312 [main-SendThread(127.0.0.1:2181)] INFO org.apache.zookeeper.ClientCnxn - Opening socket connection to server 127.0.0.1/127.0.0.1:2181. Will not attempt to authenticate using SASL (unknown error)
2017-09-01 12:57:25,313 [main-SendThread(127.0.0.1:2181)] INFO org.apache.zookeeper.ClientCnxn - Socket connection established to 127.0.0.1/127.0.0.1:2181, initiating session
2017-09-01 12:57:25,360 [main-SendThread(127.0.0.1:2181)] INFO org.apache.zookeeper.ClientCnxn - Session establishment complete on server 127.0.0.1/127.0.0.1:2181, sessionid = 0x15e3bc801b10001, negotiated timeout = 30000
2017-09-01 12:57:25,363 [main-EventThread] INFO org.I0Itec.zkclient.ZkClient - zookeeper state changed (SyncConnected)
2017-09-01 12:57:25,404 [main] INFO com.alibaba.dubbo.registry.zookeeper.ZookeeperRegistry -  [DUBBO] Register: consumer://169.254.41.206/com.alibaba.dubbo.demo.DemoService?application=demo-consumer&category=consumers&check=false&dubbo=2.5.3&interface=com.alibaba.dubbo.demo.DemoService&methods=sayHello&pid=11200&side=consumer&timestamp=1504241845217, dubbo version: 2.5.3, current host: 169.254.41.206
2017-09-01 12:57:25,473 [main] INFO com.alibaba.dubbo.registry.zookeeper.ZookeeperRegistry -  [DUBBO] Subscribe: consumer://169.254.41.206/com.alibaba.dubbo.demo.DemoService?application=demo-consumer&category=providers,configurators,routers&dubbo=2.5.3&interface=com.alibaba.dubbo.demo.DemoService&methods=sayHello&pid=11200&side=consumer&timestamp=1504241845217, dubbo version: 2.5.3, current host: 169.254.41.206
2017-09-01 12:57:25,640 [main] INFO com.alibaba.dubbo.registry.zookeeper.ZookeeperRegistry -  [DUBBO] Notify urls for subscribe url consumer://169.254.41.206/com.alibaba.dubbo.demo.DemoService?application=demo-consumer&category=providers,configurators,routers&dubbo=2.5.3&interface=com.alibaba.dubbo.demo.DemoService&methods=sayHello&pid=11200&side=consumer&timestamp=1504241845217, urls: [dubbo://169.254.41.206:20880/com.alibaba.dubbo.demo.DemoService?anyhost=true&application=demo-provider&dubbo=2.5.3&interface=com.alibaba.dubbo.demo.DemoService&methods=sayHello&pid=10132&side=provider&timestamp=1504241527110, empty://169.254.41.206/com.alibaba.dubbo.demo.DemoService?application=demo-consumer&category=configurators&dubbo=2.5.3&interface=com.alibaba.dubbo.demo.DemoService&methods=sayHello&pid=11200&side=consumer&timestamp=1504241845217, empty://169.254.41.206/com.alibaba.dubbo.demo.DemoService?application=demo-consumer&category=routers&dubbo=2.5.3&interface=com.alibaba.dubbo.demo.DemoService&methods=sayHello&pid=11200&side=consumer&timestamp=1504241845217], dubbo version: 2.5.3, current host: 169.254.41.206
2017-09-01 12:57:25,852 [main] INFO com.alibaba.dubbo.remoting.transport.AbstractClient -  [DUBBO] Successed connect to server /169.254.41.206:20880 from NettyClient 169.254.41.206 using dubbo version 2.5.3, channel is NettyChannel [channel=[id: 0x8b8d8ef5, /169.254.41.206:55395 => /169.254.41.206:20880]], dubbo version: 2.5.3, current host: 169.254.41.206
2017-09-01 12:57:25,852 [main] INFO com.alibaba.dubbo.remoting.transport.AbstractClient -  [DUBBO] Start NettyClient wangguoqi-PC/169.254.41.206 connect to the server /169.254.41.206:20880, dubbo version: 2.5.3, current host: 169.254.41.206
2017-09-01 12:57:25,931 [main] INFO com.alibaba.dubbo.config.AbstractConfig -  [DUBBO] Refer dubbo service com.alibaba.dubbo.demo.DemoService from url zookeeper://127.0.0.1:2181/com.alibaba.dubbo.registry.RegistryService?anyhost=true&application=demo-consumer&check=false&dubbo=2.5.3&interface=com.alibaba.dubbo.demo.DemoService&methods=sayHello&pid=11200&side=consumer&timestamp=1504241845217, dubbo version: 2.5.3, current host: 169.254.41.206
Hello world
```
此处的<code>Hello world</code>就是从<code>Provider</code>中获取到的，也就是消费者从生产者中获取到数据