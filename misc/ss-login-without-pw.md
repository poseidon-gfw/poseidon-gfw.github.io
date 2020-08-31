### Your aim

- To automate deploy v2ray to nodes
- Perform commands from Web TTY
- Call `ssh` from a script without entering any passwords

### How to do it

First log in on host `Poseidon-master` as user `root` and generate a pair of authentication keys.

```
root@poseidon-master:~> ssh-keygen -t rsa -N '' -f ~/.ssh/id_rsa <<< n > /dev/null
root@poseidon-master:~> ssh-copy-id -i ~/.ssh/id_rsa your-v2ray-node-host
root@your-v2ray-node-host's password: 
```

Now you can log into `your-v2ray-node-host` as `root` from poseidon-master-server as a without password:

```
root@poseidon-master:~> ssh your-v2ray-node-host
```

[加入我们](https://t.me/v2ray_poseidon)
