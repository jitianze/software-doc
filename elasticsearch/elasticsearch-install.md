                                                     Elasticsearch使用说明

# elasticsearch 安装

安装Elasticsearch唯一的要求是安装官方新版的Java，地址：[www.java.com](www.java.com)

安装好java好后，就可以下载安装并运行elasticsearch
下载地址：[www.elastic.co/downloads]() 
在这里你可以下载所有发行版和过去的版本，每个版本可以选择 zip 、 tar archive、 DEB 、 RPM package. 
Let’s download the Elasticsearch 5.4.1 tar as follows (Windows users should download the zip package):

`
curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.4.1.tar.gz
`


`
tar -xvf elasticsearch-5.4.1.tar.gz
`

解压后进入文件夹bin目录下

`
cd elasticsearch-5.4.1/bin
`

And now we are ready to start our node and single cluster (Windows users should run the elasticsearch.bat file):


 启动、关闭：
 
后台启动加上 -d 参数
`
/usr/local/elasticsearch-5.4.0/bin/elasticsearch 
`
`
/usr/local/elasticsearch-5.4.0/bin/elasticsearch -d 
`
查看启动进程：

`
jps | grep Elasticsearch
`

关闭：

`
kill -15 pid
`


修改集群和节点名称，也可以到配置文件中修改

./elasticsearch -Ecluster.name=my_cluster_name -Enode.name=my_node_name


``` shell
cluster.name: elasticsearch       #集群名称

node.name: es-node-01            #节点名称

path.data: /usr/local/elasticsearch-2.4.5/data   #数据文件存储路径

path.logs: /usr/local/elasticsearch-2.4.5/logs    #log文件存储路径

network.host: 192.168.1.102                           #默认网络连接地址

http.port: 9200                                                #连接端口

discovery.zen.minimum_master_nodes: 2     #这个参数来保证集群中的节点可以知道其它N个有master资格的节点。默认为1，对于大的集群来说，可以设置大一点的值（2-4）

discovery.zen.ping.multicast.enabled: false   #禁用多播 

discovery.zen.ping.unicast.hosts: ["192.168.1.102", "192.168.1.103", "192.168.1.104"]   #集群中master节点的初始列表，可以通过这些节点来自动发现新加入集群的节点。

discovery.zen.ping_timeout: 120s     #存活超时时间

bootstrap.system_call_filter: false    # 因centos6不支持SecComp而默认bootstrap.system_call_filter为true进行检测，所以，要设置为 false。注：SecComp为secure computing mode简写

http.cors.enabled: true  #是否支持跨域，默认为false

http.cors.allow-origin: "*"   #当设置允许跨域，默认为*,表示支持所有域名
```


启动后，如果只有本地可以访问，尝试修改配置文件 elasticsearch.yml

中network.host(注意配置文件格式不是以#开头的要空一格， ：后要空一格) 为network.host: 0.0.0.0

如果想在后台以守护进程模式运行，添加-d参数。

打开另一个终端进行测试：

`
curl 'http://localhost:9200/?pretty'
`

你能看到以下返回信息：

`
{
   "status": 200,
   "name": "Shrunken Bones",
   "version": {
      "number": "1.4.0",
      "lucene_version": "4.10"
   },
   "tagline": "You Know, for Search"
}
`

这说明你的ELasticsearch集群已经启动并且正常运行，接下来我们可以开始各种实验了。

安装插件

(1) git clone git://github.com/mobz/elasticsearch-head.git

(2) cd elasticsearch-head 

(3) npm install //一定要在elasticsearch-head目录下执行该命令（需要安装nodejs）

(4) 修改elasticsearch-head下Gruntfile.js文件，默认监听在127.0.0.1下9200端口，改为9100

(5) 启动服务

`
cd elasticsearch-head/node_modules/grunt/bin/
`

`
 ./grunt server
`

或者进入elasticsearch-head目录后，配置完Gruntfile.js 直接 npm run start 即可

浏览器访问 http://localhost:9100/  (localhost换成插件所在的机器的ip)


# kibana 安装脚本


```shell
#!/usr/bin/env python 
# -*-coding:utf-8-*-
#下载kibana bin文件压缩包
wget https://artifacts.elastic.co/downloads/kibana/kibana-5.4.1-linux-x86_64.tar.gz
#解压放到opt文件夹下，重命名
tar -zxvf kibana-5.4.1-linux-x86_64.tar.gz -C /opt
mv /opt/kibana-5.4.1-linux-x86_64 /opt/kibana-5.4.1

#修改配置文件config 下的kibana.yml
server.host: "0.0.0.0"
elasticsearch.url: "http://192.168.1.103:9200"

# 1. 需要将server.host修改一下，否则远程无法访问，这个配置在Kibana4.6.1（对应ES2.4.0）的时候还不需要配置，但是5.4.0就需要了
# 2. 之前ES和Kibana配合还需要使用plugin安装一些Marvel，sense等，现在都不需要了，DevTools就是之前的Sense

#添加环境变量
echo "export KIBANA_HOME=/opt/kibana-5.4.1
      export PATH=$PATH:$KIBANA_HOME/bin" >> /etc/profile
# 使环境变量生效
source /etc/profile

```
启动:(因为添加了环境变量，可直接用kibana命令启动)
```
/opt/kibana-5.4.1/kibana
```
现在可登录[http://localhost:5601](http://localhost:5601) 进行查看是否成功

![kibana](https://github.com/jitianze/software-doc/blob/master/picture/kibana.png)




