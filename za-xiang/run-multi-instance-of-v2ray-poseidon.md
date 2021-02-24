# 使用 systemd 多开 V2Ray 实例

## Choose a name for your instance

The name which I choosen in this tutorial is `v2cc`, it SHOULD be changed to your own instance name. the name is the key to manage your V2Ray instance.

_YOU ARE ABLE TO CREATE ANY AMOUNT OF INSTANCES YOU DESIRED BY REPEATING THESE STEPS_

Config file name format is `config-NAME.json` in `/etc/v2ray/` directory.

### Config your v2ray

```text
cp /etc/v2ray/config.json /etc/v2ray/config-v2cc.json
vim /etc/v2ray/config-v2cc.json
```

### Auto start after reboot

```text
systemctl enable v2ray@v2cc
```

### Start

```text
systemctl start v2ray@v2cc
```

### Stop

```text
systemctl stop v2ray@v2cc
```

### Status

```text
systemctl status v2ray@v2cc
```

### Logs

```text
systemctl status -l v2ray@v2cc
```

## List all your instances

```text
systemctl status 'v2ray@*'
```

