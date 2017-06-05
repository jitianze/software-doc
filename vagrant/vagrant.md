                                                        Vagrant 使用说明书

## 1.vagrant 安装

官网下载安装程序，我下载的是vagrant_1.9.5

windows：
[msi下载地址](https://releases.hashicorp.com/vagrant/1.9.5/vagrant_1.9.5.msi?_ga=2.147953008.697792310.1496625451-64033646.1495087346)


debian：
[deb下载地址](https://releases.hashicorp.com/vagrant/1.9.5/vagrant_1.9.5_x86_64.deb?_ga=2.51607150.697792310.1496625451-64033646.1495087346)

centos：
[rpm下载地址](https://releases.hashicorp.com/vagrant/1.9.5/vagrant_1.9.5_x86_64.rpm?_ga=2.85095134.697792310.1496625451-64033646.1495087346)

mac：
本人屌丝，买不起，break

windows下傻瓜式安装








vagrant配置与操作：

vagrant box add //添加box的操作

vagrant init 初始化box的操作

vagrant up 启动虚拟机的操作

vagrant ssh 登录拟机的操作

vagrant box list //显示当前已经添加的box列表

vagrant box remove //删除相应的box

vagrant destroy //停止当前正在运行的虚拟机并销毁所有创建的资源

vagrant halt //关机

vagrant package //打包命令，可以把当前的运行的虚拟机环境进行打包

vagrant plugin //用于安装卸载插件

vagrant reload //重新启动虚拟机，主要用于重新载入配置文件

vagrant ssh-config //输出用于ssh连接的一些信息

vagrant status //获取当前虚拟机的状态

vagrant suspend //挂起当前的虚拟机

vagrant resume //恢复前面被挂起的状态

Vagrantfile配置文件详解

在我们的开发目录下有一个文件Vagrantfile，里面包含有大量的配置信息，主要包括三个方面的配置，虚拟机的配置、SSH配置、Vagrant的一些基础配置。Vagrant是使用Ruby开发的，所以它的配置语法也是Ruby的，但是我们没有学过Ruby的人还是可以跟着它的注释知道怎么配置一些基本项的配置。

box设置

config.vm.box = "base"

上面这配置展示了Vagrant要去启用那个box作为系统，也就是上面我们输入vagrant init Box名称时所指定的box，如果沒有输入box名称的話，那么默认就是base，VirtualBox提供了VBoxManage这个命令行工具，可以让我们设定VM，用modifyvm这个命令让我们可以设定VM的名称和内存大小等等，这里说的名称指的是在VirtualBox中显示的名称，我们也可以在Vagrantfile中进行设定，在Vagrantfile中加入如下这行就可以设定了：

config.vm.provider "virtualbox" do |v|

v.customize ["modifyvm", :id, "--name", "astaxie", "--memory", "512"]

end

这行设置的意思是调用VBoxManage的modifyvm的命令，设置VM的名称为astaxie，内存为512MB。你可以类似的通过定制其它VM属性来定制你自己的VM。

网络设置

Vagrant有两种方式来进行网络连接，一种是host-only(主机模式)，意思是主机和虚拟机之间的网络互访，而不是虚拟机访问internet的技术，也就是只有你一個人自High，其他人访问不到你的虚拟机。另一种是Bridge(桥接模式)，该模式下的VM就像是局域网中的一台独立的主机，也就是说需要VM到你的路由器要IP，这样的话局域网里面其他机器就可以访问它了，一般我们设置虚拟机都是自high为主，所以我们的设置一般如下：

config.vm.network :private_network, ip: "11.11.11.11"

这里我们虚拟机设置为hostonly，并且指定了一个IP，IP的话建议最好不要用192.168..这个网段，因为很有可能和你局域网里面的其它机器IP冲突，所以最好使用类似11.11..这样的IP地址。

hostname设置

hostna

me的设置非常简单，Vagrantfile中加入下面这行就可以了：

config.vm.hostname = "go-app"

设置hostname非常重要，因为当我们有很多台虚拟服务器的时候，都是依靠hostname來做识别的，例如Puppet或是Chef，都是通过hostname來做识别的，既然设置那么简单，所以我们就別偷懒，设置一个。

同步目录

我们上面介绍过/vagrant目录默认就是当前的开发目录，这是在虚拟机开启的时候默认挂载同步的。我们还可以通过配置来设置额外的同步目录：

config.vm.synced_folder "/Users/astaxie/data", "/vagrant_data"

win7如： config.vm.synced_folder "D:/www", "/home/wwwroot/default"

上面这个设定，第一个参数是主机的目录，第二个参数是虚拟机挂载的目录

端口转发

config.vm.network :forwarded_port, guest: 80, host: 8080

上面这句配置可厉害了，这一行的意思是把对host机器上8080端口的访问请求forward到虚拟机的80端口的服务上，例如你在你的虚拟机上使用nginx跑了一个Go应用，那么你在host机器上的浏览器中打开http://localhost:8080时，Vagrant就会把这个请求转发到VM里面跑在80端口的nginx服务上，因此我们可以通过这个设置来帮助我们去设定host和VM之间，或是VM和VM之间的信息交互。

修改完Vagrantfile的配置后，记得要用vagrant reload命令来重启VM之后才能使用VM更新后的配置

安装web服务器时，如果无法访问，检查下linux开没开启80端口

vi /etc/sysconfig/iptables

-A INPUT -m state –state NEW -m tcp -p tcp –dport 80 -j ACCEPT（允许80端口通过防火墙）

-A INPUT -m state –state NEW -m tcp -p tcp –dport 3306 -j ACCEPT（允许3306端口通过防火墙）

特别提示：很多网友把这两条规则添加到防火墙配置的最后一行，导致防火墙启动失败，正确的应该是添加到默认的22端口这条规则的下面

/etc/init.d/iptables restart

#最后重启防火墙使配置生效 



Vagrant 就是使用 Ruby 写成的, 所以在这里的配置文件也需要满足 Ruby 语法。

    config.vm.box = "ubuntu/trusty64"

可以看到，这两行就是我们在vagrant init中后面所指定的参数。由此可以看出，vagrant init只是帮我们生成了配置文件而已，换句话说，如果我们写好了Vagrantfile，就不需要vagrant init，只需将准备好的配置文件放入到所需目录中，然后直接执行vagrant up即可。

还有很多注释掉的配置，那些都是一些常用的配置，包括网卡设置、IP地址、绑定目录，甚至可以指定内存大小、CPU个数、是否启动界面等等。如果需要，可以根据注释文本进行配置。

下面列出一些常用的配置：

config.vm.hostname：配置虚拟机主机名

config.vm.network：这是配置虚拟机网络，由于比较复杂，我们其后单独讨论

config.vm.synced_folder：除了默认的目录绑定外，还可以手动指定绑定

config.ssh.username：默认的用户是vagrant，从官方下载的box往往使用的是这个用户名。如果是自定制的box，所使用的用户名可能会有所不同，通过这个配置设定所用的用户名。

config.vm.provision：我们可以通过这个配置在虚拟机第一次启动的时候进行一些安装配置，后面我们将专门介绍。

需要注意的是，Vagrantfile文件只会在第一次执行vagrant up时调用执行，其后如果不明确使用vagrant reload，则不会被强制重新加载。

    config.vm.network"forwarded_port",guest:80,host:8080

这将让物理机的8080端口映射到虚拟机的80端口。因此，可以在物理机上直接访问虚拟机所建立的网站。这在虚拟机使用不是桥接的时候尤为重要，因为很多时候默认会禁止物理机通过网络直接访问虚拟机，通过端口映射可以依旧能够访问所需端口。

我们还可以通过config.vm.network来指定所在网络以及IP地址。一般来说，分为private_network还是public_network。

    private_network： 私有网络

位于私有网络的主机之间可以互相访问，但是外界无法访问私有网络的主机，对于某些虚拟机而言，连物理机都无法通过网络直接访问虚拟机，此时如果需要使用物理机访问虚拟机，就需要前面所提及的端口映射。例如：

    config.vm.network"private_network",ip:"192.168.33.10"

    public_network：公有网络

在绝大多数虚拟机上，这等同于桥接网络。如果虚拟机选择了桥接方式连接，也就是相当于虚拟机和物理机在网络上处于平等的位置，同样的接在了网卡所在的网络。因此从外界看来，会感觉这是两台独立的计算机。

配置方式和私有网络接近，只需将其中的private_network换成public_network。如要使用 DHCP：

    config.vm.network"public_network"

如果物理机存在多块网卡，需要指定某一块作为桥接用，那么可以参考如下配置：

    config.vm.network"public_network",:bridge=>'en1: Wi-Fi (AirPort)'

如果是在启动了虚拟机之后，需要重新加载配置，只需要执行：

    vagrant reload
