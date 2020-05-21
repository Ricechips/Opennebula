# Opennebula
网上的资料也太少了吧！

## 环境
> 主控端服务器：Centos7  192.168.106.201 
> 节点服务器（虚拟机）：Centos7  192.168.106.2
> 虚拟机里面的虚拟机：ttylinux (chop wood,carry wa/etc/selinux/configter)

## 主控端配置
> 第一种方法：脚本安装
> *wget 'https://github.com/OpenNebula/minione/releases/latest/download/minione'*
> *bash minione*
> 官方给出的一键安装脚本，因为服务器原因，速度慢

> 第二种方法：正常流程安装
> *vim /etc/selinux/config*将*SELINUX=disabled*，reboot
> 加源
> cat << "EOT" > /etc/yum.repos.d/opennebula.repo
<br> [opennebula]
<br> name=opennebula
<br> baseurl=https://downloads.opennebula.org/repo/5.10/CentOS/7/$basearch
<br> enabled=1
<br> gpgkey=https://downloads.opennebula.org/repo/repo.key
<br> gpgcheck=1
<br> repo_gpgcheck=1
<br> EOT
> *yum install epel-release*
> *yum install opennebula-server opennebula-sunstone opennebula-ruby opennebula-gate opennebula-flow*安装依赖
> *echo "oneadmin:mypassword" > ~/.one/one_auth*设置管理平台帐号密码
> *systemctl start opennebula* *systemctl start opennebula-sunstone*开启服务
> *oneuser show*验证是否正常安装
> *http://ip:9869*浏览器访问控制面板

## 节点服务器配置
> 创建虚拟机并配置网络
> 打通主控和节点服务器网络，例：*ip route add 192.168.106.0/24 via 192.168.106.1 dev ens3*
> 加源并安装依赖*yum install opennebula-node-kvm* 重启服务*systemctl restart libvirtd*
> 禁用SElinux
> 配置ssh无密码连接 *vim /etc/hosts*修改主机名如：192.168.106.201 host <br> 192.168.106.2 node
<br> *ssh-keyscan host node  >> /var/lib/one/.ssh/known_hosts* <br> *scp /etc/hosts root@node:/etc/hosts* <br> 此时主控端ssh node可直接登陆节点

## sunstone面板
> 设置可以切换中文
> 基础设施->主机 添加node节点(kvm类型)
> 从opennebula public下载镜像创建虚机进行管理


