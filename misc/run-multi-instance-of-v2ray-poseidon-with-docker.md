### 使用 docker 多开 v2ray-poseidon

```
cd /root/v2ray-poseidon/docker/sspanel
cp -a ws-tls ws-tls-2
cd ws-tls-2

# 1. 修改 config.json

# 2. 修改 docker-compose.yml
#   2.1 container_name: v2ray-ws-tls-2
#   2.2 ports 下面的端口改成其它端口，不然会和之前的 v2ray-poseidon 出现端口冲突
```
