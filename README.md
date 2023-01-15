## [TUIC](https://github.com/EAimTY/tuic) 安装指南

1. 下载程序（linux-amd64）
新建默认文件保存目录 /root/tuic
```
mkdir /root/tuic
```
下载 tuic 程序至目录 /root/tuic 并保存为 tuic 文件。
```
curl -Lo /root/tuic/tuic https://github.com/EAimTY/tuic/releases/download/0.8.5/tuic-server-0.8.5-x86_64-linux-musl && chmod +x /root/tuic/tuic
```

2. 下载配置文件至目录 /root/tuic

```
curl -Lo /root/tuic/tuic_config.json https://raw.githubusercontent.com/feodorren/tuic-install/main/config_server.json
```

3. 下载systemctl配置

```
curl -Lo /etc/systemd/system/tuic.service https://raw.githubusercontent.com/feodorren/tuic-install/main/tuic.service
```

4. 获取证书和私钥

- 申请证书
```
certbot certonly \
--standalone \
--agree-tos \
--no-eff-email \
--email tuic@example.com \
-d tuic.example.com
```
- 将证书保存到tuic配置文件内配置的位置：
```
cat /etc/letsencrypt/live/tuic.example.com/fullchain.pem > /root/tuic/fullchain.pem
cat /etc/letsencrypt/live/tuic.example.com/privkey.pem > /root/tuic/privkey.pem

```
- 新建一个certbot的hook脚本文件，用于让tuic重新加载续期后的新证书：
```
nano /etc/letsencrypt/renewal-hooks/post/tuic.sh
```
```
cat /etc/letsencrypt/live/tuic.example.com/fullchain.pem > /root/tuic/fullchain.pem
cat /etc/letsencrypt/live/tuic.example.com/privkey.pem > /root/tuic/privkey.pem
systemctl restart tuic
```

- 给予脚本执行权限
```
chmod +x /etc/letsencrypt/renewal-hooks/post/tuic.sh
```

- 测试续期的情况以及脚本能否正常运行：
```
certbot renew --cert-name tuic.example.com --dry-run
```
5. 启动程序

```
systemctl daemon-reload && systemctl enable --now tuic
```

```
systemctl status tuic
```

| 项目 | |
| :--- | :--- |
| 程序 | /root/tuic/tuic |
| 配置 | /root/tuic/tuic_config.json |
| 检查 | /root/tuic/tuic -c /root/tuic/tuic_config.json |
| 查看日志 | journalctl -u tuic --output cat -e |
| 实时日志 | journalctl -u tuic --output cat -f |

## v2rayN 5.x 配置指南

1. 下载Windows客户端程序[tuic-client-0.8.5-x86_64-windows-msvc.exe](https://github.com/EAimTY/tuic/releases/download/0.8.5/tuic-client-0.8.5-x86_64-windows-msvc.exe)，重命令为`tuic.exe`，复制到v2rayN文件夹。

2. 下载客户端配置[config_client.json](https://raw.githubusercontent.com/chika0801/tuic-install/main/config_client.json)，修改`chika.example.com`为证书中包含的域名，修改`10.0.0.1`为VPS的IP。

3. `服务器` ——> `添加自定义配置服务器` ——> `浏览(B)` ——> `选择客户端配置` ——> `Core类型 tuic` ——> `Socks端口 50001`

![1](https://user-images.githubusercontent.com/88967758/195763590-f035f90f-f228-4022-b318-770791c63b92.jpg)

小技巧：只要证书在有效期内，证书中包含的域名不用解析到VPS的IP。一份证书，在多个VPS上使用。

## SagerNet 配置指南

<details><summary>点击查看</summary>

| 选项 | 值 |
| :--- | :--- |
| 配置名称 |  |
| 服务器 | VPS的IP 或 域名 |
| 服务器端口 | 8443 |
| Token | chika |
| 应用层协议协商 | h3 |
| 证书（链） | 留空 |
| UDP Relay Mode | NATIVE |
| Congestion Controller | BBR |
| Disable SNI |不勾 |
| 服务器名称指示 | 证书中包含的域名 |
| Reduce RTT | 勾上 |

</details>

## Shadowrocket 配置指南

<details><summary>点击查看</summary>

| 选项 | 值 |
| :--- | :--- |
| 配置名称 |  |
| 类型 | TUIC |
| 地址 | VPS的IP 或 域名 |
| 端口 | 8443 |
| 密码 | chika |
| 模式 | bbr |
| 允许不安全 | 不选 |
| UDP转发 | 选上 |
| SNI | 证书中包含的域名 |
| ALPN | h3 |
| 备注 | 留空 |

</details>

## Surge 配置指南

<details><summary>点击查看</summary>

ProxyName = tuic, 域名, 8443, token=chika, alpn=h3

</details>


## 鸣谢
https://github.com/EAimTY/tuic  

https://github.com/chika0801/tuic-install  

https://lala.im/8424.html
