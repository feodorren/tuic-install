# [TUIC](https://github.com/EAimTY/tuic) 安装指南

## 1. 下载程序（linux-amd64）
新建默认文件保存目录 /root/tuic
```
mkdir /root/tuic
```
用于aarch64架构, 下载 tuic 程序至目录 /root/tuic 并保存为 tuic 文件。
```
curl -Lo /root/tuic/tuic https://github.com/EAimTY/tuic/releases/download/tuic-server-1.0.0/tuic-server-1.0.0-aarch64-unknown-linux-musl && chmod +x /root/tuic/tuic
```
用于x86_64架构, 下载 tuic 程序至目录 /root/tuic 并保存为 tuic 文件。
```
curl -Lo /root/tuic/tuic https://github.com/EAimTY/tuic/releases/download/tuic-server-1.0.0/tuic-server-1.0.0-x86_64-unknown-linux-musl && chmod +x /root/tuic/tuic
```

## 2. 下载配置文件至目录 /root/tuic

```
curl -Lo /root/tuic/tuic_config.json https://raw.githubusercontent.com/feodorren/tuic-install/main/config_server.json
```

## 3. 下载systemctl配置

```
curl -Lo /etc/systemd/system/tuic.service https://raw.githubusercontent.com/feodorren/tuic-install/main/tuic.service
```

## 4. 获取证书和私钥

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
## 5. 启动程序

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



## Surge 配置指南

<details><summary>点击查看</summary>

TUIC = tuic, domain, 38886, sni=domain, server-cert-fingerprint-sha256=XX:XX:XXX......  , uuid=b1b269fc-3371-4a69-b812-765030db1638, alpn=h3, password=password0, version=5

</details>


## 鸣谢
https://github.com/EAimTY/tuic  

https://github.com/chika0801/tuic-install  

https://lala.im/8424.html
