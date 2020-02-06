# V2ray_ws_tls_website
支持一键生成websocket+tls+v2ray并使用showdoc作为网站伪装
<u>本文提供两个部分</u>：
- 只想做一个伪装免封的vps。见第一部分
- 除了安装vps之外还想在服务器搭建其他网站和应用。见第二部分

【注：】
- 网站伪装默认绑定https的443端口，
- 伪装原理：每次需要进行转发的被墙流量都通过nginx的location /伪装path/捕捉。直接访问https://域名 会访问https默认443端口，被其他location或者默认index捕捉。利用的nginx绑定端口后，可以通过location进行不同的本地应用处理。
- 需要注册自己的域名，并将域名托管到cloudfare或者自己的dns服务器上。免费域名可以到freenom，或者画个几块钱买一年的。
v2ray：
- 支持v2ray所有协议及混淆方式
- 本文两个脚本install & install_bt_nginx均提供bbrplus及锐速加速设置。总之就是强。

## 第一部分
说明：这个是给纯小白用的防封的服务器vpn搭建脚本，（只会买个服务器的，此脚本会与安装的LNMP冲突)
- 服务器购买：vultr，google cloud，aliyun（香港)都可，环境安装推荐centos7
- 域名获取：godaddy花几块钱一年买个二级域名，或者freenom申请免费域名.(比如买的域名为 tt.net)
- 域名托管：将域名托管到阿里云dns解析或者cloudfare上，托管方法自行google（也就是把域名的nameserv改一下)。修改完生效时间大概要6-8分钟左右。
- 上你托管到的服务商（阿里云dns或者cloudfare）添加解析记录：
> [记录类型]:A [记录名]：www（或者别的都行）[记录值]：xxxx.xxx.xxx.xxx（你购买的搭建vpn的服务器ip)
> 配置完，到http://ping.chinaz.com/ 输入自己的www.tt.net,查看ping得结果是否是自己的服务器ip。
- 登陆到自己的服务器,进入命令行，输入执行如下命令：
```
wget -O install.sh "https://raw.githubusercontent.com/YeSei/V2ray_ws_tls_showdoc/master/install.sh" && chmod +x install.sh && bash install.sh
```
- 安装顺序：
  - 1]：选择1 Nginx+ws+tls
  - 2]：在输入域名信息时输入自己注册的：www.tt.net 。其他的都默认即可。
  - 3]：安装完会获得一个vmess地址，和一个二维码。
  - 4]：手机或电脑下载个v2ray客户端（v2rayNG，V2rayX...）复制vmess地址从剪贴板导入即可，二维码扫一下也可。
  - 5]：启动客户端服务，测试一下网速，就ok完事。
  
- 优化（以上已经可以使用,此步为优化速度，不搞也可)：
  - 客户端一般选择需要代理的应用，其他应用不走代理速度不受影响。
  - 服务端再次执行上述脚本：选择11.安装4合1 bbr。先安装bbrplus内核(需重启),再应用bbrplus加速，最后优化系统配置(需重启).
 

## 第二部分
说明：管理自己的服务器我们常常会用到一些服务器管理面板工具，比如宝塔bt面板，WDCP，LuManager。。。
而这些管理工具常常自带安装LNMP套件，并作一些优化。安装路径也不一样。比如yum安装nginx使用systemctl enable nginx.service默认在/etc/systemd/system 下生成nginx.service文件。而使用面板管理工具后会在/run/systemd/中进行设置自启服务。一般run优先级较etc目录低。
这样就会导致一个问题，由于v2ray需要配置nginx来做网站伪装
