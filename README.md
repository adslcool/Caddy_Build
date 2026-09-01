Debian 13 安装 Caddy 命令：
=======================================================================================================
caddy_arch="$(dpkg --print-architecture)"

curl -fL \
  "https://github.com/adslcool/Caddy_Build/releases/latest/download/caddy-linux-${caddy_arch}.deb" \
  -o "/tmp/caddy-linux-${caddy_arch}.deb"

sudo apt install -y "/tmp/caddy-linux-${caddy_arch}.deb"
=======================================================================================================
安装完成后：systemctl status caddy
