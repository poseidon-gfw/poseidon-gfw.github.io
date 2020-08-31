V2board 面板和 VNetPanel 面板对接的后端都是自动生成的配置文件，若你想自己配置 v2ray 来完成其它功能：例如免流、多个入口以及其它 V2Ray 支持的高级操作。

```
$ cat /etc/v2ray/config.json
{
  "poseidon": {
    "useLocalConfig": true, // 表示使用配置文件中的配置，而非面板生成的
  },
  // 你的 V2Ray 配置
}
```
