
**使用已有证书**

```json
{"provider": "local", "certPath": "/etc/v2ray/v2ray.crt", "keyPath": "/etc/v2ray/v2ray.key"}
```

使用 DNS 验证，自动生成 TLS 证书

通过 DNS 验证来生成证书，这样不需要端口占用，较为方便和通用

> CloudFlare  
{"provider": "cloudflare", "email": "", "api_key": ""}

> Alibaba Cloud DNS  
{"provider": "alidns", "email": "", "access_key": "", "secret_key": ""}

> Gandi  
{"provider": "gandi", "email": "", "api_key": ""}

> Go Daddy  
{"provider": "godaddy", "email": "", "api_key": "", "api_secret": ""}
