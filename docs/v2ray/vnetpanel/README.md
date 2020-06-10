# VNetPanel & Poseidon V2Ray

## 部署准备

> Linux服务器（操作系统Centos7）  
域名  
Shell终端  
脑子  
VNetPanel程序

## 域名解析

> 本教程示例域名 conse.xyz

### 注册并登录 CloudFlare

![](assets/2020-05-17-18-18-14.png)

### 添加你在别的服务商购买的域名

![](assets/2020-05-17-18-19-31.png)

### 选择Free

![](assets/2020-05-17-18-20-15.png)

### 暂时跳过添加DNS记录

![](assets/2020-05-17-18-20-57.png)

### 更改名称服务器

![](assets/2020-05-17-18-39-31.png)

### 域名配置，默认即可

![](assets/2020-05-17-18-47-45.png)

### 如图配置完成

![](assets/2020-05-17-18-51-36.png)

## 前端部署

> 通过SSH工具访问服务器  
PS：推荐使用
[FinallShell](http://www.hostbuf.com/t/988.html)
丨[Terminus](https://www.termius.com/)

### 更新系统

```
yum update -y
```

![](assets/2020-05-18-09-27-38.png)

### 安装开发工具包

```
yum -y groupinstall "Development Tools"
```

![](assets/2020-05-18-09-28-11.png)

### 宝塔面板

```
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && bash install.sh
```

![](assets/2020-05-17-19-07-53.png)

> 输入 y 确认

![](assets/2020-05-17-19-10-12.png)

#### 安装完成输出登录信息，建议单独保存

| Bt-Panel	| 登录地址 |
|-----------|---------|
| username  | 账户     |
| password  | 密码     |

### 浏览器访问宝塔面板登录

![](assets/2020-05-17-19-11-33.png)

### 安装如图LNMP环境

> 强烈建议选择编译安装，安装时间较长，环境较为稳定

![](assets/2020-05-17-19-13-37.png)

### 进入软件商店安装Redis

![](assets/2020-05-17-20-24-52.png)

> 安装速度取决于服务器性能，静待安装完成  
完成后请确保所有可升级的软件升到**最新版本**

### 配置PHP

#### 安装扩展 ionCube fileinfo redis opache sg11

![](assets/2020-05-17-20-30-19.png)

#### 禁用函数 proc_open putenv pcntl_alarm pcntl_signal getenv

![](assets/2020-05-18-10-29-57.png)

### 添加网站

> 本教程根目录为/www/wwwroot/vnetpanel

![](assets/2020-05-18-15-03-32.png)

### 域名 DNS A 记录解析到服务器 IP

![](assets/2020-05-17-20-29-09.png)

### 进入网站根目录添删除默认文件

![](assets/2020-05-18-15-03-55.png)

### 伪静态 选择 laravel5

![](assets/2020-05-18-15-04-16.png)

### 上传源码到网站根目录并解压

![](assets/2020-05-18-15-07-53.png)

### 创建数据库 编码 utf8mb4

![](assets/2020-05-18-15-05-37.png)


### 安装依赖

```
cd /www/wwwroot/vnetpanel
php composer.phar install
```

![](assets/2020-05-18-15-10-22.png)

### 网站目录

> 站点管理-网站目录-运行目录 /pubilc,取消 防跨站攻击

![](assets/2020-05-18-15-12-47.png)

### SSL

![](assets/2020-05-17-20-36-19.png)

### 此时网站进入安装界面，填入对应数据库信息

![](assets/2020-05-18-15-13-05.png)

> 默认登陆信息 **admin**丨**123456**

![](assets/2020-05-18-15-13-47.png)

### 定时任务

> 在 **/var/spool/cron** 目录下新建 **www** 文件，写入下面内容并设置权限为 **root 600**

```
* * * * * /www/server/php/73/bin/php /www/wwwroot/vnetpanel/artisan schedule:run >> /dev/null 2>&1
```

> 注意底部需要保留一行空行

![](assets/2020-05-18-15-15-55.png)

![](assets/2020-05-18-15-16-03.png)

### 设置推送

> 软件商店 安装并打开 **Supervisor管理器** > 添加守护进程

| 启动用户 | www |
|---------|-----|
| 运行目录	| /www/wwwroot/vnetpanel/ |
| 启动命令	| /www/server/php/73/bin/php /www/wwwroot/vnetpanel/artisan queue:work redis --daemon --queue=default --timeout=60 --tries=3 -vvv |

![](assets/2020-05-18-15-20-06.png)

> 确保状态为 **RUNNING**，无法正常启动请检查PHP扩展及函数，网站文件权限等。

![](assets/2020-05-18-15-21-44.png)

## 完成安装

### 前台界面

![](assets/2020-05-18-15-17-55.png)

### 后台界面（地址 你的域名/admin）

![](assets/2020-05-18-15-16-35.png)

> 后台-工具箱-系统运行日志  
正常显示日志表示正常运行

![](assets/2020-05-18-16-59-57.png)

## 邮件

### 发件配置

> 示例采用SMTP的QQ邮箱发件

登录网页版QQ邮箱 设置-账户

开启SMTP获取授权码

![](assets/2020-05-18-07-34-13.png)

![](assets/2020-05-18-07-35-28.png)

> 编辑网站根目录下的**.env**配置文件

![](assets/2020-05-18-15-23-51.png)

| MAIL_HOST | SMTP服务器 |
|-----------|-----------|
| MAIL_PORT | SMTP端口   |
| MAIL_USERNAME | 邮箱地址     |
| MAIL_PASSWORD | SMTP授权码   |
| MAIL_ENCRYPTION | ssl |
| MAIL_FROM_ADDRESS | 邮箱地址  |
| MAIL_FROM_NAME | 发件人名称   |

**转到Supervisor管理器，重启守护进程生效**

### 消息推送

> 进入后台管理 获取**Server酱**的 **SCKEY** 填入

[Server酱](http://sc.ftqq.com/3.version)

![](assets/2020-05-18-16-32-52.png)

### 商业授权

**填入Poseidon授权码及TLS配置，授权码需要购买，无试用。**

> TLS配置（以CloudFlare为例）

```json
{"provider": "cloudflare", "email": "", "api_key": ""}
```

**获取 CloudFlare api_key**

> CloudFlare -> 右上角头像 -> 我的个人资料 -> API令牌 -> Global API Key -> 查看

![](assets/2020-05-18-16-31-22.png)

## 节点部署（以WebSocket-TLS 模式为例）

> 节点服务器 Centos7 X64

通过SSH工具访问服务器

### 安装加速

> 推荐使用BBR2或BBRPlus

```bash
yum install wget
wget -N --no-check-certificate "https://github.000060000.xyz/tcp.sh" && chmod +x tcp.sh && ./tcp.sh
```
![](assets/2020-05-17-22-41-00.png)

**reboot** 重启完成后 `./tcp.sh` 进入管理脚本，开启加速

![](assets/2020-05-17-22-48-18.png)
![](assets/2020-05-17-22-49-57.png)

#### 同步时间

```bash
yum -y install ntpdate
timedatectl set-timezone Asia/Shanghai
ntpdate ntp1.aliyun.com
```

![](assets/2020-05-17-22-50-43.png)

#### 关闭防火墙

```bash
systemctl disable firewalld
systemctl stop firewalld
```

![](assets/2020-05-17-22-51-17.png)

### 添加伪装域名

> 节点管理 -> 证书列表 -> 添加域名证书

![](assets/2020-05-18-17-03-21.png)

> **填写伪装域名，KEY和PEM为证书，可填可不填**

![](assets/2020-05-18-17-04-49.png)

#### CloudFlare 节点域名解析，将节点地址通过A记录指向节点服务器 IP，云朵<span style="color: grey">灰色</span>

![](assets/2020-05-18-17-22-15.png)

#### 新增节点

##### TLS+WebSocket模式

> 域名与伪装域名相同

![](assets/2020-05-18-17-28-02.png)

##### WebSocket模式

> 连接端口和服务端口可任意更换  
域名与伪装域名相同

![](assets/2020-05-18-17-29-53.png)

#### 节点授权

> 在节点授权界面找到刚添加的节点 点击部署后端

![](assets/2020-05-18-17-32-05.png)

> 弹出的V2Ray-Poseidon命令为节点部署命令

![](assets/2020-05-18-17-35-07.png)

### 部署节点

> 将上面的部署命令直接复制到节点服务器终端，运行即可

![](assets/2020-05-18-17-36-56.png)

![](assets/2020-05-18-17-37-48.png)

#### 查看运行状态

```bash
systemctl status v2ray
```

> 状态显示 **Active: active (running)** 表示正常运行，非正常状态请查看日志（journalctl -x -n 300 --no-pager -u v2ray ）排查原因

![](assets/2020-05-18-17-38-33.png)

#### 此时节点列表显示节点状态及运行时间

![](assets/2020-05-18-17-40-30.png)

> 无法正常使用请获取运行日志到 [V2Ray Poseidon](https://t.me/v2ray_poseidon) 提问

### 更多命令

> 更新 **curl -L -s https://bit.ly/2VhvcPz | bash**  
卸载 **curl -L -s https://bit.ly/2SGFMMY | bash**

|日志	| journalctl -x -n 300 --no-pager -u v2ray |
| ---|----------------------|
|停止	| systemctl stop v2ray |
|状态	| systemctl status v2ray|
|启动	| systemctl start v2ray |
|重启	| systemctl restart v2ray |

### 更多配置模式

先介绍下 VNetPanel 节点设置的几个参数，明白之后就可以自由组合了。

|参数 |	说明 |
|----|-----|
|服务端口 |	V2Ray 所监听的端口 |
|连接端口 |	用户连接时的端口，直连 V2Ray 的话和服务端口一致，如果不是直连比如通过了中转或使用了反向代理进行转发，则填写中转或反向代理的端口 |
|服务端 | TLS	是否启用 V2Ray TLS，原始的 V2Ray 需要自己去处理证书，V2Ray-Poseidon可以自动帮你处理 TLS 证书，请参阅《配置 TLS 证书》提供相应的信息|
|连接 | TLS	用户连接时是否启用 TLS|
|TLS | 服务商授权	当服务端 TLS为启用时，必须配置该参数|

#### TCP 模式

> 传输协议 => TCP  
服务端 TLS => 禁用  
服务端口 => 10086 ( 你可以设置为你可用的任何端口 )  
连接端口 => 10086

#### 给 TCP 套上 TLS 证书

> 准备一个域名比如 v2node-01.vnetpanel.com，并解析到该节点地址  
域名和伪装域名都设置为 v2node-01.vnetpanel.com  
传输协议改成 TCP  
启用 TLS  
TLS服务商授权 参照《配置 TLS 证书》规则，填上你域名提供商的信息

#### WebSocket 模式 (直连)

> 传输协议改成 WebSocket  
禁用 TLS  
保证路径不为空，由 VNetPanel 自动生成的就非常安全，例如：/gahutaodoy8dcs21，每个节点都不一样

#### WebSocket + 百度 CDN

> 据 @vft9ness 大佬所说：使用百度 CDN 都不建议开启 TLS，所以直接按《WebSocket 模式》配置，然后把端口改为 80即可。

#### WebSocket + TLS 模式 (直连)

> 准备一个域名比如 v2node-01.vnetpanel.com，并解析到该节点地址，保证 CF 后面的小云朵为灰色的  
域名和伪装域名都设置为 v2node-01.vnetpanel.com  
传输协议改成 WebSocket  
保证路径不为空，由 VNetPanel 自动生成的就非常安全，例如：/gahutaodoy8dcs21，每个节点都不一样  
服务端口 => 443 ( 你可以设置为你可用的任何端口 )  
连接端口 => 443  
服务端 TLS => 启用  
连接 TLS => 启用  
TLS服务商授权 参照《配置 TLS 证书》规则，填上你域名提供商的信息

#### WebSocket + CloudFlare CDN = WebSocket + CF CDN + TLS(这个证书是由 CF 自动生成的）

> 准备一个域名比如 v2node-01.vnetpanel.com，并解析到该节点地址，并点亮 CF 后面的小云朵，CF 的 SSL/TLS 改为 Flexible  
域名和伪装域名都设置为v2node-01.vnetpanel.com  
服务端口 => 80  
连接端口 => 443  
传输协议 => WebSocket  
保证路径不为空，由 VNetPanel 自动生成的就非常安全，例如：/gahutaodoy8dcs21，每个节点都不一样  
服务端 TLS => 关闭  
连接 TLS => 启用

### 配置 TLS 证书

[tls_json](../_include/tls_json.md ':include')

[Licensing](../_include/_licensing.md ':include')
