## HAProxy SNI 中转 TLS 获取用户真实 IP

####  安装 HAProxy

```
yum -y install haproxy
```

#### 修改配置文件

清空默认配置

```
echo '' > /etc/haproxy/haproxy.cfg
```

举例，现在我有个中转分配的端口为 `20001`，需要转发 `sg1-test-trojanp.poseidon-gfw.cc`

```
frontend sg1_frontend
  bind *:20001
  default_backend sg1_server

backend sg1_server
  server s1 sg1-test-trojanp.poseidon-gfw.cc:443 send-proxy
```

如果需要中转多个落地机，可以复制上面的内容，比如

```
# 这是第一个
frontend sg1_frontend
  bind *:20001
  default_backend sg1_server

backend sg1_server
  server s1 sg1-test-trojanp.poseidon-gfw.cc:443 send-proxy

# 这里是第二个
frontend jp1_frontend(这个是可以随便改的，每个中转端口一个名字，别重就行)
  bind *:20002
  default_backend jp1_server(这里必须为落地的名字， backend 后面的内容)

backend jp1_server(此处为落地的名字，可以随便改，每个落地一个名字，别重就行)
  server s1 jp1-test-trojanp.poseidon-gfw.cc:443 send-proxy

# ... ... 还可以有更多
```


重启 HAProxy

```
systemctl restart haproxy
```

#### 使用小火箭测试

| 服务器	| 中转服务器域名或IP |
|-----------|---------|
| 端口  | 20001     |
| Peer 名称  |  sg1-test-trojanp.poseidon-gfw.cc  |

