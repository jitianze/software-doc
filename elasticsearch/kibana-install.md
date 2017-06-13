```shell
#!/usr/bin/env python 
# -*-coding:utf-8-*-
#下载kibana bin文件压缩包
wget https://artifacts.elastic.co/downloads/kibana/kibana-5.4.1-linux-x86_64.tar.gz
#解压放到opt文件夹下，重命名
tar -zxvf kibana-5.4.1-linux-x86_64.tar.gz -C /opt
mv /opt/kibana-5.4.1-linux-x86_64 /opt/kibana-5.4.1

#修改配置文件

#添加环境变量
echo "export KIBANA_HOME=/opt/kibana-5.4.1
      export PATH=$PATH:$KIBANA_HOME/bin" >> /etc/profile
# 使环境变量生效
source /etc/profile



```
