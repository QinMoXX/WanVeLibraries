> 参考文章:
> [如何在 Ubuntu 20.04 上安装和使用 Docker - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/143156163)
> [Install Docker Engine on Ubuntu | Docker Documentation](https://docs.docker.com/engine/install/ubuntu/)

# 使用apt存储库
1. 更新 `apt` 包索引并安装包，以允许 `apt` 通过HTTPS使用存储库：
```Shell
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
```

> [!error]
> 如果遇见 apt 更新失败 :
> Some index files failed to download. They have been ignored, or old ones used instead.
> 修改 /etc/resolv.conf 文件，添加 DNS 服务器
> ```
> nameserver 8.8.8.8
> nameserver 223.6.6.6
> ```


2. 添加 Docker 的官方 GPG 密钥：
```Shell
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

3. 使用以下命令设置存储库：
```Shell
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

# 安装 Docker 引擎
1. 更新 `apt` 包索引：
```Shell
sudo apt-get update
```

2. 安装Docker Engine、containerd和Docker Compose。
```Shell
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```


