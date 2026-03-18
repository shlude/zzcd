# caddy使用
Pointfocus注册码：  17566FT6xwer310

### 配置文件

默认：/etc/caddy/Caddyfile

### 启动（后台运行）

```bash
sudo caddy start --config /etc/caddy/Caddyfile
```

### 启动（前台运行）

```bash
sudo caddy run --config /etc/caddy/Caddyfile
```

### 停止

```bash
sudo caddy stop
```

# 利用CF自建隧道https访问

### 第一步：安装 Cloudflare 连接器

```bash
# 下载官方安装包
curl -L --output cloudflared.deb https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb

# 安装
sudo dpkg -i cloudflared.deb
```

### 第二步：登录并建立隧道

```bash
cloudflared tunnel login
```

终端会给出一个 **URL 链接**。复制它到 Windows 浏览器打开。

在网页上选择你的域名，点击 **授权 (Authorize)**。

### 第三步：创建隧道

授权完成后，回到终端创建一个隧道（名字可以随便起，比如叫 `my-wsl-web`）：

```bash
cloudflared tunnel create my-wsl-web
```

### 第四步：配置域名映射

```bash
# 将域名绑定到隧道
# 格式：cloudflared tunnel route dns <隧道名> <你的域名>
cloudflared tunnel route dns my-wsl-web web.yourdomain.com
```

### 第五步：运行隧道

```bash
# 运行隧道并转发到 Caddy 端口
cloudflared tunnel run --url http://localhost:8080 my-wsl-web
```

### 最后验证

现在，拿出你的手机，关闭 WiFi（使用 4G/5G 网络），在浏览器输入你的域名 `http://web.yourdomain.com`。
