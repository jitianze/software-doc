                                                     Elasticsearch使用说明



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


4、启动、关闭：

`shell
/usr/local/elasticsearch-5.4.0/bin/elasticsearch #前台启动

/usr/local/elasticsearch-5.4.0/bin/elasticsearch -d #后台启动

#查看启动进程：

jps | grep Elasticsearch

#关闭：

kill -15 pid
`

此时应该在终端得到如下内容（中间有省略）：

`
[2016-09-16T14:17:51,251][INFO ][o.e.n.Node               ] [] initializing ...
[2016-09-16T14:17:51,329][INFO ][o.e.e.NodeEnvironment    ] [6-bjhwl] using [1] data paths, mounts [[/ (/dev/sda1)]], net usable_space [317.7gb], net total_space [453.6gb], spins? [no], types [ext4]
[2016-09-16T14:17:51,330][INFO ][o.e.e.NodeEnvironment    ] [6-bjhwl] heap size [1.9gb], compressed ordinary object pointers [true]
[2016-09-16T14:17:51,333][INFO ][o.e.n.Node               ] [6-bjhwl] node name [6-bjhwl] derived from node ID; set [node.name] to

[2016-09-16T14:17:53,671][INFO ][o.e.t.TransportService   ] [6-bjhwl] publish_address {192.168.8.112:9300}, bound_addresses {{192.168.8.112:9300}
[2016-09-16T14:17:53,676][WARN ][o.e.b.BootstrapCheck     ] [6-bjhwl] max virtual memory areas vm.max_map_count [65530] likely too low, increase to at least [262144]
[2016-09-16T14:17:56,731][INFO ][o.e.h.HttpServer         ] [6-bjhwl] publish_address {192.168.8.112:9200}, bound_addresses {[::1]:9200}, {192.168.8.112:9200}
[2016-09-16T14:17:56,732][INFO ][o.e.g.GatewayService     ] [6-bjhwl] recovered [0] indices into cluster_state
[2016-09-16T14:17:56,748][INFO ][o.e.n.Node               ] [6-bjhwl] started
`
6-bjhwl  就时所谓的节点名，可以通过以下命令修改：


./elasticsearch -Ecluster.name=my_cluster_name -Enode.name=my_node_name


`shell
cluster.name: elasticsearch       #集群名称
node.name: es-node-01            #节点名称
path.data: /usr/local/elasticsearch-2.4.5/data   #数据文件存储路径
path.logs: /usr/local/elasticsearch-2.4.5/logs    #log文件存储路径
network.host: 192.168.1.102                           #默认网络连接地址
http.port: 9200                                                #连接端口
discovery.zen.minimum_master_nodes: 2     #这个参数来保证集群中的节点可以知道其它N个有master资格的节点。默认为1，对于大的集群来说，可以设置大一点的值（2-4）
# discovery.zen.ping.multicast.enabled: false   #禁用多播 
discovery.zen.ping.unicast.hosts: ["192.168.1.102", "192.168.1.103", "192.168.1.104"]   #集群中master节点的初始列表，可以通过这些节点来自动发现新加入集群的节点。
discovery.zen.ping_timeout: 120s     #存活超时时间
bootstrap.system_call_filter: false    # 因centos6不支持SecComp而默认bootstrap.system_call_filter为true进行检测，所以，要设置为 false。注：SecComp为secure computing mode简写
http.cors.enabled: true  #是否支持跨域，默认为false
http.cors.allow-origin: "*"   #当设置允许跨域，默认为*,表示支持所有域名
`


启动后，如果只有本地可以访问，尝试修改配置文件 elasticsearch.yml

中network.host(注意配置文件格式不是以#开头的要空一格， ：后要空一格) 为network.host: 0.0.0.0

如果想在后台以守护进程模式运行，添加-d参数。

打开另一个终端进行测试：

curl 'http://localhost:9200/?pretty'

你能看到以下返回信息：

{
   "status": 200,
   "name": "Shrunken Bones",
   "version": {
      "number": "1.4.0",
      "lucene_version": "4.10"
   },
   "tagline": "You Know, for Search"
}

这说明你的ELasticsearch集群已经启动并且正常运行，接下来我们可以开始各种实验了。





















