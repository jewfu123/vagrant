
## Centos5.9 install on virtualbox6.1.8
#### Env:
####   win11 + virtual6.1.8 + vagrant + Centos5.9 Final
####   install from iso disk 1,2

#### Network Setting commands: Net Briage Type:---->
Set for Net Card:
```shell
vi /etc/sysconfig/network-scripts/ifcfg-enp0s3
```
-->
```shell
# Intel Corporation 82540EM Gigabit Ethernet Controller
TYPE="Ethernet"
DEVICE=enp0s3
HWADDR=08:00:27:48:2C:E4
BOOTPROTO=yes
DEFROUTE=yes
BROWSER_ONLY=no

IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy

PEERDNS=yes
PEERROUTES=yes
NAME=enp0s3
UUID="f4d787cc-0eb5-44a3-a95f-307056eb6138"
IPADDR=192.168.50.114
GATEWAY=192.168.50.104
#IPADDR=192.168.0.6
#DNS1=192.168.0.1
NETMASK=255.255.255.0
NM_CONTROLLED=no
#DNS1=8.8.8.8
#DNS2=8.8.4.4
#USERCTL=no
ONBOOT=yes

---------
service network restart
ip addr
ip route
```
------------------------------------------------------------
Set for DNS:
vi /etc/resolv.conf
-->
```shell
# Generated by NetworkManager

# No nameservers found; try putting DNS servers into your
# ifcfg files in /etc/sysconfig/network-scripts like so:
#
# DNS1=xxx.xxx.xxx.xxx
# DNS2=xxx.xxx.xxx.xxx
# DOMAIN=lab.foo.com bar.foo.com
nameserver 8.8.8.8
nameserver 8.8.4.4
```
------------------------------------------------------------
-->
vi /etc/yum.repos.d/CentOS-Base.repo
--> update source urls:
```shell
# CentOS-Base.repo
#
# The mirror system uses the connecting IP address of the client and the
# update status of each mirror to pick mirrors that are updated to and
# geographically close to the client.  You should use this for CentOS updates
# unless you are manually picking other mirrors.
#
# If the mirrorlist= does not work for you, as a fall back you can try the
# remarked out baseurl= line instead.
#
#
[base]
name=CentOS-$releasever - Base
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os
#baseurl=http://mirror.centos.org/centos/$releasever/os/$basearch/
baseurl=http://archive.kernel.org/centos-vault/5.11/os/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-5

#released updates
[updates]
name=CentOS-$releasever - Updates
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates
#baseurl=http://mirror.centos.org/centos/$releasever/updates/$basearch/
baseurl=http://archive.kernel.org/centos-vault/5.11/updates/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-5

#additional packages that may be useful
[extras]
name=CentOS-$releasever - Extras
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras
#baseurl=http://mirror.centos.org/centos/$releasever/extras/$basearch/
baseurl=http://archive.kernel.org/centos-vault/5.11/extras/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-5

#additional packages that extend functionality of existing packages
[centosplus]
name=CentOS-$releasever - Plus
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus
#baseurl=http://mirror.centos.org/centos/$releasever/centosplus/$basearch/
baseurl=http://archive.kernel.org/centos-vault/5.11/centosplus/$basearch/
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-5

#contrib - packages by Centos Users
[contrib]
name=CentOS-$releasever - Contrib
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=contrib
#baseurl=http://mirror.centos.org/centos/$releasever/contrib/$basearch/
baseurl=http://archive.kernel.org/centos-vault/5.11/contrib/$basearch/
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-5
```
//---------------------------------------------------------

----> update: --->
```shell
yum clean all
yum makecache
yum update
```
-------------->
[root@centos5 ~]# ifconfig
```shell
enp0s3    Link encap:Ethernet  HWaddr 08:00:27:48:2C:E4
          inet addr:192.168.50.114  Bcast:192.168.50.255  Mask:255.255.255.0
          inet6 addr: 240b:c010:4a0:60ee:a00:27ff:fe48:2ce4/64 Scope:Global
          inet6 addr: fe80::a00:27ff:fe48:2ce4/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:507198 errors:0 dropped:0 overruns:0 frame:0
          TX packets:199358 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:666295608 (635.4 MiB)  TX bytes:15939337 (15.2 MiB)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:16436  Metric:1
          RX packets:237 errors:0 dropped:0 overruns:0 frame:0
          TX packets:237 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:25285 (24.6 KiB)  TX bytes:25285 (24.6 KiB)
```
-------------->
```shell
ls /etc/sysconfig/network-scripts/ifcfg-*
ip addr show
service NetworkManager status

nmtui

service NetworkManager status

netstat -rn

cat /etc/resolv.conf

route add default gw 192.168.128.2

grep hosts /etc/nsswitch.conf

vi /etc/sysconfig/network:
-->
NETWORKING=yes
HOSTNAME=centos7
GATEWAY=10.0.0.1

/etc/resolv.conf:
-->
nameserver 8.8.8.8
nameserver 8.8.4.4
```
#### ----> get uuid command:
```shell
blkid -s UUID
```
## open and close net card command :
```shell
ifup eth0
ifdown eth1
```

## yum update from cdrom ==>
```shell
mount /dev/cdrom /mnt
```
vi /etc/yum.repos.d/rhel7.repo
```shell
[base]
name=rhel7.repo
baseurl=file://mnt
gpgcheck=0
enabled=1
```
centos5 yum update from cdrom
```shell
yum clean all

yum build cache

yum repolist

yum install nmap
```
## About mount
```shell
mkdir /mnt/share
#$ sudo mount -t vboxsf [你的windows共享目录] [Ubuntu共享目录]
#$ sudo mount -t vboxsf virtualbox_share /mnt/share/
mount -t vboxsf media_sf_tmp_ /mnt/share

mkdir /mnt/vagrant
mkdir /mnt/vbox_tmp
mount -t vboxsf vagrant /mnt/vagrant
```
#### 实现开机自动挂载
```shell
sudo gedit /etc/fstab
#在文件末添加一项：
#<共享名称> < Ubuntu共享目录> vboxsf defaults 0 0
virtualbox_share /mnt/share/ vboxsf defaults 0 0

#vbox-tmp-begin
media_sf_tmp_ /mnt/vbox_tmp vboxsf defaults 0 0
#vbox-tmp-end
```

## Setup httpd and setting:
```shell
/sbin/service iptables status

iptables -A RH-Firewall-1-INPUT -m state --state NEW -p tcp --dport 8044 -j ACCEPT

cp /etc/httpd/conf/httpd.conf ~/httpd.conf.backup

vi /etc/httpd/conf/httpd.conf

ls -l /etc/httpd/conf/httpd.conf
```
#### Apache Svr:
```shell
yum install httpd

service httpd restart

chkconfig httpd on

vi /etc/httpd/conf/httpd.conf
/NameVirtualHost

<VirtualHost *:80>
#    ServerAdmin webmaster@dummy-host.example.com
    DocumentRoot /var/www.html
    ServerName itsupport.db
#    ErrorLog logs/dummy-host.example.com-error_log
#    CustomLog logs/dummy-host.example.com-access_log common
</VirtualHost>
 
service httpd restart

elinks http://127.0.0.1
```

#### Laravel5.5 in Centos5.9
1. install:
```shell
$composer create-project "laravel/laravel=5.5.*" --prefer-dist {インストールフォルダ名}
```
2. edit .env
```shell
/.env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE={データベース名}
DB_USERNAME={ID}
DB_PASSWORD={Password}
```
3. storage parmition change:
```shell
$chmod 777 -R storage
```
4. DBの191文字超エラー回避
```shell
app/Providers/AppServiceProvider.php
class AppServiceProvider extends ServiceProvider
{
    public function boot()
    {
        //追記
        \Illuminate\Support\Facades\Schema::defaultStringLength(191);
    }
    ...
```
5. publicを {アプリドメイン}/ でアクセス
```shell
Php laravel 5.5 project .htaccess file
https://gist.github.com/liaotzukai/8e61a3f6dd82c267e05270b505eb6d5a

/.htaccess
//ルートディレクトリに配置
<IfModule mod_rewrite.c>
    <IfModule mod_negotiation.c>
        Options -MultiViews
    </IfModule>
    RewriteEngine On
    RewriteCond %{REQUEST_FILENAME} -d [OR]
    RewriteCond %{REQUEST_FILENAME} -f
    RewriteRule ^ ^$1 [N]
    RewriteCond %{REQUEST_URI} (\.\w+$) [NC]
    RewriteRule ^(.*)$ public/$1 
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^ server.php
</IfModule>
```
6. おまけ
```shell
$git init
$git add .
$git commit -m 'start'
//Git-Hub マイリポジトリから URL をコピーしておく
$git remote add origin '{URL}'
$git push -u origin master
```
#### vagrant access with ssh public key:
```shell
1. cmd -> vagrant ssh-config to get path of '.vagrant.d'

2. cd '.vagrant.d'

3. cmd -> ssh-keygen -y -f insecure_private_key > id_rsa.pub
convert 'insecure_private_key' to 'id_rsa.pub'

4. cat 'id_rsa.pub' and copy data.

5. paste into ~/.ssh/newfilename
    for example:
    echo 'xxxxx' >> authorized_keys

6. in Linux set file auth..:
    chmod 600 authorized_keys
    chmod 700 .ssh/

7. go to your vagrant path and cmd -> vagrant reload.

*验证成功的方法：
vagrant ssh 能否正常登录。
ssh -i C:\vagrant\ubuntu\.vagrant\machines\default\virtualbox\id_rsa.pub -p 4422 vagrant@127.0.0.1 能否正常登录
*->
vagrant ssh 利用的是服务端append的key文件（来自客户端的ssh-keygen)
ssh -i .../pub -p xxxx name@127.0.0.1 (直接利用客户端自我产生的key文件ssh登录）
```

#### vagrant access with ssh private key:
```shell
1. in ~/.ssh/ cmd -> ssh-keygen -t rsa -b 4096
    enter password > get prived key file.

2. in vagrant conf file add below line:
    Vagrant.configure("2") do |config|
        config.vm.box = "rrapid7/metasploitable3-ub1404"
        config.vm.box_version = "0.1.12-weekly"
  >>>   config.ssh.private_key_path = File.expand_path('~/.ssh/id_rsa')

3. vagrant reload
```
#### vagrant accessed can't with ssh key:
1. in vagrant config file:
   >> config.ssh.insert_key = false
2. vagrant reload

#### How can I have multiple authorized_keys files?
```shell
Simply append the name of the file next to the old one.

AuthorizedKeysFile     ~/.ssh/authorized_keys ~/.ssh/authorized_keys_generated
```

#### How to modify Vagrant VM’s Default SSH PORT

We can add a config in Vagarantfile to assign unique SSH port to each Vagrant VM.
```shell
config.vm.network :forwarded_port, guest: 22, host: 3200, id: 'ssh'
```
Setting id: ssh overwrites the default port mapping.

#### Laravel command:
```shell
composer require laravel/ui

php artisan config:publish broad casting
```
#### php + composer.phar
```shell
mv composer.phar /usr/local/bin/composer

php composer install
..
```












































