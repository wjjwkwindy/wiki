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

## Step 3 - 测试 web 服务器

测试 Nginx 是否正常运行：

```
systemctl status nginx
```

访问 ip 地址查看是否能正常访问

## Step 4 - 管理 Nginx 进程

要停止服务，请输入：

```bash
sudo systemctl stop nginx
```

要在服务停止时启动它，请输入：

```bash
sudo systemctl start nginx
```

要停止并重新启动服务，请输入：

```bash
sudo systemctl restart nginx
```

如果你只是修改了配置，Nginx 可以在不断开链接的情况下重新加载。要这样操作，请输入：

```bash
sudo systemctl reload nginx
```

默认情况下，Nginx 会在服务器启动时自动启动。如果你不希望这样，输入下面的命令来禁止此行为：

```bash
sudo systemctl disable nginx
```

要重新启用自动启动，请输入：

```bash
sudo systemctl enable nginx
```

## Step 5 - 设置 Server Blocks（推荐）

为 **example.com** 创建一个文件夹，输入 `-p` 参数来创建必要的父文件夹。

```bash
sudo mkdir -p /var/www/example.com/html
```

// TODO