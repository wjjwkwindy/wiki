# Ubuntu 安装 Nginx

> Ubuntu: 18.04  
> 参考链接：https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-18-04

## Step 1 - 安装 Nginx

```bash
sudo apt update
sudo apt upgrade
sudo apt install nginx
```

## Step 2 - 调整防火墙

列出应用程序的配置文件

```bash
sudo ufw app list
```

输出的配置文件：

```bash
Available applications:
  Nginx Full
  Nginx HTTP
  Nginx HTTPS
  OpenSSH
```

Nginx 提供了 3 个配置文件
- **Nginx Full**：此配置文件同时打开端口80（正常，未加密的网络流量）和端口443（TLS / SSL 加密的流量）
- **Nginx HTTP**：此配置文件仅打开端口80（正常，未加密的网络流量）
- **Nginx HTTPS**：此配置文件仅打开端口443（TLS / SSL 加密流量）

将需要的端口规则添加到 UFW 防火墙：

```bash
sudo ufw allow 'Nginx Full'
```

通过输入以下内容来验证更改：

```bash
sudo ufw status
```

可以在输出中看到允许的 HTTP 流量，并且状态为 active 就没问题。

```
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere                  
Nginx HTTP                 ALLOW       Anywhere                  
OpenSSH (v6)               ALLOW       Anywhere (v6)             
Nginx HTTP (v6)            ALLOW       Anywhere (v6)
```