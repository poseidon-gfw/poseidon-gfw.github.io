每个入口都有一个 tag，下面配置的两个入口的 tag 分别为 `proxy` 和 `vless_proxy`，修改 `poseidon.user.inboundTag` 这里将这两个 tag 用英文逗号连接起来，具体可参照下面的配置

```
{
		"poseidon": {
			"user": {
				// inbound tag, which inbound you would like add user to
				"inboundTag": "proxy,vless_proxy",
			}
		}

        "inbounds": [
                {
                        "port": 443,
                        "protocol": "vmess",
                        "tag": "proxy"
                }, {
                        "port": 8443,
                        "protocol": "vless",
                        "tag": "vless_proxy"
                }
        ],

        // 你的其它配置
}
```

如果是使用的 docker 运行的，还需要将新的端口映射到宿主机，方法为：

```
vim docker-compose.yml 


    ports:
      - "443:443"
      - "8443:8443"
```

