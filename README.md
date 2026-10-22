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

Release 标题和说明使用简体中文，所有发布附件均带 Caddy 版本号。以 `v2.x.x` 为例：

```text
caddy-v2.x.x-windows-amd64.zip
caddy-v2.x.x-linux-amd64.tar.gz
caddy-v2.x.x-linux-amd64.deb
caddy-v2.x.x-linux-arm64.tar.gz
caddy-v2.x.x-linux-arm64.deb
SHA256SUMS-v2.x.x.txt
```

## Debian / Ubuntu 一键安装

自动识别 AMD64 / ARM64：

```bash
ARCH="$(dpkg --print-architecture)" && \
RELEASE_URL="$(curl -fsSL -o /dev/null -w '%{url_effective}' \
  https://github.com/adslcool/Caddy_Build/releases/latest)" && \
CADDY_VERSION="${RELEASE_URL##*/}" && \
curl -fL \
"https://github.com/adslcool/Caddy_Build/releases/download/${CADDY_VERSION}/caddy-${CADDY_VERSION}-linux-${ARCH}.deb" \
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

首次将工作流上传到默认分支的 `.github/workflows/Caddy_Build.yml` 时自动检查并构建。
以后每天北京时间 **08:00** 检查 Caddy 最新正式版；对应版本已发布且附件完整时跳过，未发布或附件不完整时自动编译发布。
手动运行可强制重新构建。构建文件仅上传 GitHub Release，不使用 Actions Artifacts，构建日志仍保留在 Actions 页面。


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
