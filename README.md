# Dubbo-Zookeepr-Spring 本教程是在windows7下运行
由于Zookeeper太大，我把zookeeper压缩了，解压后，修改<code>conf</code>下的配置文件下的
<code>zoo.cfg</code>文件，在zookeeper官网下载到zookeeper时是没有这个文件的，将<code>zoo_sample.cfg</code>复制一个为<code>conf</code>的文件即可。
'''config
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

'''
修改<code>dataDir</code>和<code>dataLogDir</code>即可，修改后需要创建相应的文件夹，及存储日志和数据，否则启动zookeeper会报错，在zookeeper文件夹下的bin文件夹下双击<code>zkServer.cmd</code>可以启动zookeeper，启动成功后可以看到
'''log
ent:java.io.tmpdir=C:\Users\WANGGU~1\AppData\Local\Temp\
2017-08-31 19:29:30,589 [myid:] - INFO  [main:Environment@100] - Server environm
ent:java.compiler=<NA>
2017-08-31 19:29:30,590 [myid:] - INFO  [main:Environment@100] - Server environm
ent:os.name=Windows 7
2017-08-31 19:29:30,590 [myid:] - INFO  [main:Environment@100] - Server environm
ent:os.arch=amd64
2017-08-31 19:29:30,591 [myid:] - INFO  [main:Environment@100] - Server environm
ent:os.version=6.1
2017-08-31 19:29:30,592 [myid:] - INFO  [main:Environment@100] - Server environm
ent:user.name=wangguoqi
2017-08-31 19:29:30,592 [myid:] - INFO  [main:Environment@100] - Server environm
ent:user.home=C:\Users\wangguoqi
2017-08-31 19:29:30,592 [myid:] - INFO  [main:Environment@100] - Server environm
ent:user.dir=D:\WorkSpace\EclipseWorkspace\Dubbo+Zookeepr+Spring\zookeeper\bin
2017-08-31 19:29:30,599 [myid:] - INFO  [main:ZooKeeperServer@755] - tickTime se
t to 2000
2017-08-31 19:29:30,599 [myid:] - INFO  [main:ZooKeeperServer@764] - minSessionT
imeout set to -1
2017-08-31 19:29:30,599 [myid:] - INFO  [main:ZooKeeperServer@773] - maxSessionT
imeout set to -1
2017-08-31 19:29:30,618 [myid:] - INFO  [main:NIOServerCnxnFactory@94] - binding
 to port 0.0.0.0/0.0.0.0:2181
'''
这就证明zookeeper启动成功