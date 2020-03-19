# Vultr 使用密钥登录服务器

## 1.客户端生成密钥对

```shell
ssh-keygen -t rsa
```

## 2.将公钥复制到服务器上

```shell
scp -P 22 /.ssh/id_rsa.pub root@xxx.xxx.xxx.xxx
```

## 3.新建用户并添加和授权 .ssh 文件夹

```shell
adduser username
mkdir /home/username/.ssh
chown -R username:username /home/username/.ssh
```

## 4.复制公钥到 .ssh 文件夹中

```shell
cp -rf id_rsa.pub /home/username/.ssh/
```

## 5.修改服务器的 ssh config 文件

```txt
#修改端口号
Port xxxx

#禁止 root 登录（但是在 Vultr web 端的 console 中还是可以登录）
PermitRootLogin no

#RSA 验证
RSAAuthentication yes

#开启公钥验证
PubkeyAuthentication yes

#验证文件路径
AuthorizedKeysFile ./ssh/id_rsa.pub

#禁止密码认证
PasswordAuthentication no

#禁止空密码
PermitEmptyPasswords no

#禁用 PAM
UsePAM no
```

## 6.授予 user su 权限

```shell
```

## 7.使用密钥进行登录

```shell
ssh -i .ssh/id_rsa username@xxx.xxx.xxx.xxx -p 22
```
