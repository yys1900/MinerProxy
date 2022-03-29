# MinerProxy
最新ETH/ETC矿池代理中转程序hellominer，可免费定制软件内置抽水账号，打造专属自己的版本，有需要进群找群主。 Web界面操作，简单易用，一键安装，小白可以轻松上手。可以自定义抽水，独创PID抽水算法，稳定精准，秒杀一切市面上随机抽水算法。 采用Golang语言开发，性能稳定优异。无视CC，自动CC防护，自动封IP。支持币地址白名单，支持统一币地址，支持 TLS/SSL/WS 加密、支持前置CDN/NGINX一切反向代理， 支持自签名证书或者正规证书，支持安装为系统服务，开机自启动，支持进程守护运行，程序自动调整连接数限制
功能特色
采用Golang语言开发，网络性能优异。
抽水稳定，创新PID算法，不像市面上所谓的随机方法，抽水不稳定不准确。
支持设置币地址白名单，矿机的提交地址，只有在白名单里面中才能连接中转端口，全方位保护中转服务。
程序做了动态调整上报算力，抽水不影响矿池的统计图，抽水前后矿池算力图不会出现偏差。
无视CC，自动CC防护，自动封IP，还支持手动封IP，解封IP。
支持统一矿机提交币地址，有效拦截矿机内核抽水。
全部web界面操作，简单易用，小白也能轻松驾驭，同时web界面还适配手机，手机上也能轻松操作。
单机4核，4g，稳定带5000+矿机。
中转端口可以开启ws加密模式，可以前置CDN/Nginx等任意的web反向代理，矿机端只需要运行加密隧道客户端 minernat 即可连接ws中转端口，全程加密，防止被监控。
中转端口可以开启ssl/tls加密模式，配置域名证书和密钥，全程加密，防止被监控。
支持一切基于Stratum v1 挖矿协议的ETH/ETC矿池，ssl/tls加密协议和tcp协议都支持。
程序支持注册为系统服务，开机自启动，管理端口可以通过配置文件自由修改。
程序还支持手动普通方式运行，此种方式支持后台守护参数运行。
程序自动调整ulimit打开文件数限制，无需手动修改系统配置。
系统要求
系统类型：Linux: Debian 9及以后, Centos 7及以后, Ubuntu 12及以后。
依赖命令：iptables，ipset。
一台国外VPS，不要用国内VPS！
安装方式
重要提示：因为会用到iptables，ipset，自动调整系统ulimit连接数限制，所有安装方式都需要root账号权限。

下面针对不同人群，提供了2种安装方式，选择其中一种进行安装即可。

方式一：一键安装
如果是小白，可以执行下面的一键安装脚本，就把hellominer安装为了系统服务。

bash -c "$(curl -s -L https://github.com/hellominer/hellominer/raw/main/install.sh)" @ install
具体程序的启动，停止，重启，状态命令如下：

程序启动：systemctl start hellominer
程序停止：systemctl stop hellominer
程序重启：systemctl restart hellominer
程序状态：systemctl status hellominer
启动日志：journalctl -u hellominer
程序卸载：/etc/hellominer/hellominer uninstall
程序配置文件路径：/etc/hellominer/conf，可以通过修改/etc/hellominer/conf/app.toml里面的配置修改程序web管理端口。
默认管理端口是51301，假设你的vps的IP是，192.168.1.1，那么访问：http://192.168.1.1:51301 就可以进入管理登录页面，默认密码是：123456 。进入后台后，点击右上角头像可以修改密码。
更新程序
更新程序只需要执行：

bash -c "$(curl -s -L https://github.com/hellominer/hellominer/raw/main/install.sh)" @ update

修改程序配置
hellominer提供了一键配置脚本只需运行：

bash -c "$(curl -s -L https://github.com/hellominer/hellominer/raw/main/tools.sh)"

方式二：手动安装
点击下载 hellominer.tar.gz 。
执行：mkdir /etc/hellominer，创建安装目录。
把文件hellominer.tar.gz放在目录/etc/hellominer下面。
执行：cd /etc/hellominer && tar zxfv hellominer.tar.gz && ./hellominer init
执行：cd /etc/hellominer && ./hellominer 即可启动，此时是前台运行，关闭ssh后，程序会被关闭，如果一切正常可以加上后台守护参数。
步骤5没问题后，建议后台守护方式运行：cd /etc/hellominer && ./hellominer --daemon --forever --flog null
重启程序执行：pkill hellominer && cd /etc/hellominer && ./hellominer --daemon --forever --flog null
配置文件目录位于：/etc/hellominer/conf,可以通过修改/etc/hellominer/conf/app.toml里面的配置，改变程序web管理端口。
默认管理端口是51301，假设你的vps的IP是，192.168.1.1，那么访问：http://192.168.1.1:51301 就可以进入管理登录页面，默认密码是：123456 。进入后台后，点击右上角头像可以修改密码。
更新程序
更新程序只需要复制下面命令执行即可：

cd /etc/hellominer && rm -rf hellominer hellominer.tar.gz && curl -o hellominer.tar.gz -s -L https://github.com/hellominer/hellominer/raw/main/releases/hellominer.tar.gz && tar zxfv hellominer.tar.gz

更新完毕，需要程序重启，执行：pkill hellominer && cd /etc/hellominer && ./hellominer --daemon --forever --flog null

使用SSL/TLS加密
准备证书文件：
程序默认自带了自签名证书，位于/etc/hellominer/conf目录下面，分别是证书文件server.crt和私钥server.key, 如果需要用自己的正规证书，只需要把你的证书改名成server.crt，私钥文件改成server.key。 覆盖/etc/hellominer/conf目录下面的同名文件即可。

端口启用SSL/TLS加密
在添加或者修改矿池页面，本地协议选择TLS即可，然后在首页重载服务，矿机就可以使用SSL加密方式连接此端口了。
