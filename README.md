# Caddy Custom Build

基于 **Caddy + xcaddy** 的 GitHub Actions 自动构建项目，集成常用第三方插件，并自动发布 Windows / Linux 二进制文件及 Debian 安装包。

## 集成插件

* `caddy-l4` — Layer 4 TCP/UDP 代理
* `caddy-security` — 身份认证
* `caddy-dns/cloudflare` — Cloudflare DNS
* `caddy-cloudflare-ip` — Cloudflare 真实 IP
* `caddy-trusted-cloudfront` — CloudFront Trusted Proxy
* `caddy-webdav` — WebDAV
* `caddy-maxmind-geolocation` — MaxMind GeoIP

## 支持平台

* Windows AMD64
* Linux AMD64
* Linux ARM64
* Debian / Ubuntu AMD64 `.deb`
* Debian / Ubuntu ARM64 `.deb`

## 下载

最新版：

https://github.com/adslcool/Caddy_Build/releases/latest

Release 包含：

```text
caddy-windows-amd64.zip
caddy-linux-amd64.tar.gz
caddy-linux-amd64.deb
caddy-linux-arm64.tar.gz
caddy-linux-arm64.deb
SHA256SUMS
```

## Debian / Ubuntu 一键安装

自动识别 AMD64 / ARM64：

```bash
ARCH="$(dpkg --print-architecture)" && \
curl -fL \
"https://github.com/adslcool/Caddy_Build/releases/latest/download/caddy-linux-${ARCH}.deb" \
-o /tmp/caddy.deb && \
sudo apt install -y /tmp/caddy.deb
```

安装完成后：

```bash
caddy version
systemctl status caddy
```

配置文件：

```text
/etc/caddy/Caddyfile
```

服务管理：

```bash
systemctl restart caddy
systemctl reload caddy
journalctl -u caddy -f
```

## 构建

进入：

```text
GitHub → Actions → Build and release Caddy → Run workflow
```

可指定：

```text
latest
```

自动构建最新版 Caddy，也可以填写指定版本：

```text
v2.x.x
```

推送 `v*` Tag 时同样会自动构建并发布 Release。

## 查看已编译模块

```bash
caddy list-modules --skip-standard --packages --versions
```

## 项目地址

https://github.com/adslcool/Caddy_Build
