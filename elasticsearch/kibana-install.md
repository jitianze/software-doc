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

登录界面如下：

![kibana](https://github.com/jitianze/software-doc/blob/master/picture/kibana.png)
