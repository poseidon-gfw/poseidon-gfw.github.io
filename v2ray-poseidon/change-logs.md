### 公告

有 V2ray 定制需求，请入群联系管理员

[加入我们](https://t.me/v2ray_poseidon)

当前支持的面板：*SSRPanel*, *V2Board*, *SSPanel-v3-Uim*, *VNetPanel*, *WHMCS*

### 更新日志

#### v1.6.1
- 支持多个入口
- docker 内可使用已有证书
- docker 内部从 caddy 换成 acme.sh
- 支持更多的 dns 服务商获取 tls 证书
- 支持使用 http 验证的方式获取 tls 证书


#### v1.6.0
- 升级 v2ray 核心到 v4.27.4
- 支持 VLESS 协议
- 支持 ProxyProtocol 可在中转情况下获取用户真实 IP 地址（即使使用了 TLS

#### v1.5.8
- 增加新的 DOH 的方式来判断域名解析的真实性

#### v1.5.5
- 升级 v2ray 核心到 v4.23.4

#### v1.5.4
- VNetPanel TLS 自动生成支持使用 `http` 和 `tls` 的验证方式，使用方式见文档[0300 配置自动生成管理 TLS 证书
](https://github.com/ColetteContreras/v2ray-poseidon/wiki/0300-%E9%85%8D%E7%BD%AE%E8%87%AA%E5%8A%A8%E7%94%9F%E6%88%90%E7%AE%A1%E7%90%86-TLS-%E8%AF%81%E4%B9%A6)

#### v1.5.3
- 简化给 VNetPanel 面板生成的 V2Ray 配置文件
- 修复 VNetPanel TCP+HTTP 伪装
- 修复 SSPanel IP 上报错误

#### v1.5.2
- 修复当对接面板为 v2board 时后端会一直重启

#### v1.5.1
- 修复当配置文件中未开启 stats 字段就无法统计流量

#### v1.5.0
- 移除 API 端口占用，配置更简单
- 后面授权一律为在线授权，以前离线授权同样支持
- 支持 SSPanel V3 Uim 审计规则
- 触发规则后上传到 VNetPanel 
- 默认开启统计
- 简化配置
  - 配置中不需要创建 `stats`
  - 配置中不需要 tag 为 api 的入站规则
  - 所以也不需要单独为 api 配置路由
  - 配置策略也不需要，现在默认开启统计

#### v1.4.7

- 新增[淘宝时间 API](http://api.m.taobao.com/rest/api3.do?api=mtop.common.getTimestamp)
- 修复 [Timezone](https://timezonedb.com/api) API 解析错误

#### v1.4.6

- 新增 VNetPanel webapi 对接，支持审计（审计上报还在开发中）
- 新增系统时间检测，如果与格林威治时间差60秒，则报错。防止因时间问题造成的 invalid user 错误
- 其它一些小修改
- 这版跳了这么多版本的原因是版本管理未做好导致的。。。所以这个的上一版就是 v1.3.3 了，没有中间版本

#### v1.3.3
- 适配 V2Board v1.2

#### v1.3.2
- 修复 SSRPanel 同步用户的 bug

#### v1.3.1
- 修复无日志输出的 bug
- 更新在线 IP 限制的机制，一个 IP 会在活动的 10 钟之内被标记为在线，如果 10 分钟之内未有任何数据传输或连接，则会被标记为离线

#### v1.3.0

- 修复 SSPanel 在线设备数为 0 的 Bug
- 更精确的用户在线 IP 统计，之前只在建立连接的时候统计 IP，现在在传输的过程中也统计，将会更加精确

#### v1.2.3

- 支持 WHMCS 系统
- 适配 V2board v1.1.2
- 社区版用户数超过 50，直接退出，避免歧义
- 给 v2board 加上限制在线IP数和限速的配置

#### v1.2.1

- 修复：SSRPanel 新版 VNetPanel 无 `user_label` 表的错误

#### v1.2.0

##### 支持 SSPanel-v3-Uim WebAPI
- *更好的同时在线IP功能限制*，看了下 SSPanel 的策略是，若在同一段时间内，客户端连接的 IP 超过设定的阈值，该帐户就会被禁止一段时间（从用户使用超过我们设定的阈值，和实际被限制，中间可能会隔很长时间）。这里有许多潜在的问题：1. 影响客户的体验, 2. 之前已经建立的链接是不受影响的（比如：之前在下载的内容，并不会被强制断开，而是会继续下载）。
- 后端支持的同时在线 IP 限制功能，将使用户无法使用超过设定的 IP 数，如设定该用户只能同时 5 个 IP 在线，此时他分享的另一个朋友又想用第 6 个 IP 连接使用，v2ray 会直接拒绝他的这个新的请求。
- 精准的限速功能，可以自行测试一下 1Mps = 128 KB/s

因为 WebAPI 并没有将限制同时在线的 IP 数返回，所以要让 v2ray-poseidon 支持此功能的话，得在 SSPanel 代码的目录里执行以下命令：

（最新 dev 分支的无需执行下面这行命令，因为 dev 分支的代码已经返回了对应的值）

```
$ sed -i "s/'u', 'd')/'u', 'd', 'node_connector')/" $(find ./ -iname 'UserController.php' | grep Mod_Mu)
```

##### v2ray 的配置将从配置文件读取

```
$ cat /etc/v2ray/config.json
```

```
{
  // 你的 v2ray 配置文件写在这里
  // ... ... 
  // 以下是 v2ray-poseidon 的配置项
  "poseidon": {
    "panel": "sspanel-webapi", // 这里必须是这个，不能修改
    // Node id on your SSPanel
    "nodeId": 1,
    "license_key": "", // 无效，或空的 license key 都会被当成社区版
    // every N seconds
    "checkRate": 120,
    "panelUrl": "http 或 https://你的 sspanel 域名",
    "panelKey": "你的 panel key",
    "user": {
      // inbound tag, which inbound you would like add user to
      "inboundTag": "proxy",
      "level": 0,
      "alterId": 16,
      "security": "none"
    }
  }
}
```


#### v1.1.1

- v2board 面板支持使用本地配置

v2board 的面板可以支持相对简单的配置，如果需要较复杂的配置，可以在节点上使用本地配置的方案。如果使用本地配置方案，则 `poseidon.localPort` 选项将不再生效.

```
$ cat /etc/v2ray/config.json
```

```
{
  // your can put your v2ray config here
  "poseidon": {
    // ... ...
    "useLocalConfig": true,
  }
}
```

#### v1.1.0

- 支持 v2board 面板

首先感谢 [v2board](https://github.com/v2board/v2board) 的作者，该面板真的非常漂亮，代码也写的很扎实，许多功能非常地实用，比如在**面板中配置 v2ray，这样用户省了许多麻烦，只需要在面板中配置一次即可**。作者正在考虑，是否需要将 license key 加入到面板，这样用户就不需要在每个节点上配置 v2ray 的 license key，方便地设置和更新，而不需要去操作 v2ray 的节点。

因为 v2board 支持生成节点配置，所以在我们的节点上，仅需要添加一些 v2board 相关的配置，即可。v2ray-poseidon 会自动地去获取配置文件，并启动服务，若配置文件修改了，将会在 *checkRate* 秒之后自动重新加载配置文件。

把以下内容复制到 */etc/v2ray/config.json* ，然后修改相应的字段
```
{
  "poseidon": {
    "panel": "v2board",  // 这一行必须存在，且不能更改
    "nodeId": 1, // 你的节点 ID 和 v2board 里的一致
    "license_key": "PUT YOU LICENSE KEY HERE", // 你获得的 v2ray-poseidon 的授权码写到这里，期待后面直接在 v2board 里配置该项内容
    "checkRate": 60, // 每隔多长时间同步一次配置文件、用户、上报服务器信息
    "webapi": "http or https://YOUR V2BOARD DOMAIN", // v2board 的域名信息
    "token": "v2board token", // v2board 后台配置的 token 
    "localPort": 10084          // **本地 api, dokodemo-door,　监听在哪个端口，不能和服务端口相同**
  }
}
```

因为这是第一版，可能会有许多问题，[加入我们](https://t.me/v2ray_poseidon)并反馈您的问题。

#### v1.0.0

- 配置文件结构的更改

```diff
{
-  "ssrpanel": {
+  "poseidon": {
    ... ... 
  }
}
```

- 限制用户同时在线 IP 数
  - 类似 Shadowsocks 的设备限制功能（V2ray 只能实现成这样了）
  - 用户表的 `protocol_param` 字段为限制用户同时在线的 IP 数
- 用户限速
  - 用户表的 `speed_limit_per_user` 字段为限制用户的速度，单位为`字节/s`
  - 用户表的 `speed_limit_per_conn` 字段为限制用户的每个链接的速度，单位为`字节/s`

