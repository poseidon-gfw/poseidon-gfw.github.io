每个入口都有一个 tag，下面我配置的两个入口的 tag 分别为 `proxy` 和 `vless_proxy`，然后在 `poseidon.user.inboundTag` 这里将这两个 tag 用英文逗号连接起来。

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
                        "streamSettings": {
                                "network": "ws",
                                "wsSettings": {
                                        "path": "/"
                                }
                        },
                        "tag": "proxy"
                }, {
                        "port": 8443,
                        "protocol": "vless",
                        "tag": "vless_proxy"
                        "streamSettings": {
                                "network": "ws",
                                "wsSettings": {
                                        "path": "/"
                                }
                        },
                }
        ],

        // 你的其它配置
}


```
