# 使用 docker 多开 V2Ray 实例

## 1. 复制目录

_注意下面的 sspanel 可替换为 v2board/ssrpanel 等_ _注意下面的 ws-tls 可替换为 tcp/ws_

```text
cd /root/v2ray-poseidon/docker/sspanel
cp -a ws-tls ws-tls-2
cd ws-tls-2
```

## 2. 修改 config.json

## 3. 修改 docker-compose.yml

* container\_name: v2ray-ws-tls-2
* ports 下面的端口改成其它端口，不然会和之前的 v2ray-poseidon 出现端口冲突

## 4. 启动

```text
dc up -d
```

