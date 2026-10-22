[README.md](https://github.com/user-attachments/files/33816706/README.md)
# Caddy 自定义构建

集成常用插件，支持 Windows AMD64、Linux AMD64 / ARM64，提供压缩包和 Debian / Ubuntu 安装包。

**[下载最新版本](https://github.com/adslcool/Caddy_Build/releases/latest)**（附版本号及 SHA-256 校验文件）

## 集成插件

`caddy-l4`、`caddy-security`、`caddy-dns/cloudflare`、`caddy-cloudflare-ip`、`caddy-trusted-cloudfront`、`caddy-webdav`、`caddy-maxmind-geolocation`。

## Debian / Ubuntu 安装

自动识别 AMD64 / ARM64，下载并安装最新版：

```bash
ARCH="$(dpkg --print-architecture)" && \
REPO="https://github.com/adslcool/Caddy_Build" && \
RELEASE_URL="$(curl -fsSL -o /dev/null -w '%{url_effective}' "$REPO/releases/latest")" && \
CADDY_VERSION="${RELEASE_URL##*/}" && \
curl -fL "$REPO/releases/download/$CADDY_VERSION/caddy-$CADDY_VERSION-linux-$ARCH.deb" \
  -o /tmp/caddy.deb && \
sudo apt install -y /tmp/caddy.deb
```

安装后自动启用并启动服务，以 root 用户运行。配置文件：`/etc/caddy/Caddyfile`。

```bash
sudo systemctl restart caddy  # 修改配置后重启
systemctl status caddy       # 查看服务状态
journalctl -u caddy -f        # 查看日志
```

## 自动构建

- 首次上传工作流到默认分支时自动检查并构建。
- 每天北京时间 **08:00** 检查最新正式版，已发布且附件完整时跳过，否则编译发布。
- 手动运行：**Actions → Build and release Caddy → Run workflow**，版本填写 `latest` 或 `v2.x.x`；推送 `v*` 标签也可触发构建。
- 文件仅保存到 **GitHub Release**，不使用 Artifacts；构建日志在 Actions 查看。
