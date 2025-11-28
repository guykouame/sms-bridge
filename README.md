# 🌐 SMS-Bridge (短信通)

---

## 🌗 README 主题切换（Light / Dark）

> **把几十块钱的 USB 上网卡插到玩客云 / 树莓派 / 任意 Linux 设备，让它变成一台“云手机”。自动收短信、转发短信、网页发短信保号。**

---

## 🚀 项目简介

**SMS-Bridge（短信通）** 是一个轻量级的“云手机”方案：

- 使用二手 USB 4G 上网卡（10–20 元）。
  
- 插入任意 SIM 卡（Giffgaff / Helium / 中国移动保号卡皆可）。
  
- 利用 Python + AT 指令读取短信并转发。
  
- 提供现代化 Web UI，可在线发送短信保号。
  
- 支持 Docker / systemd 部署，适配玩客云、树莓派等设备。
  

**核心目标：** 不再需要备用机，不需要给手机一直插着 SIM 卡。  
直接在电脑 / 服务器 / Telegram 里收短信！

---

## ✨ 功能亮点

- 📥 **短信自动接收**：通过 USB 4G Dongle 读取 SIM 卡短信。
  
- 📤 **自动转发**：
  
  - Telegram Bot
    
  - PushPlus（转发到微信）
    
- 📟 **Web 控制面板**：
  
  - 查看所有短信
    
  - 在线发送短信（保号）
    
  - 一键启动 / 停止监听
    
- 💾 **SQLite 持久化**：所有短信保存到本地数据库。
  
- 🐳 **Docker 支持**：开箱即用，适配玩客云。
  
- 🔧 **systemd 支持**：适合直接跑在 Linux 主机。
  
- 🔍 **增强型 Modem 解析**：兼容更多型号 USB 上网卡，稳定解析 AT 短信。
  

---

## 📂 仓库结构

```
SMS-Bridge/
├─ README.md
├─ LICENSE
├─ requirements.txt
├─ Dockerfile
├─ docker-compose.yml
├─ config.example.yaml
├─ app/
│  ├─ config.py
│  ├─ db.py
│  ├─ modem.py
│  ├─ forwarder.py
│  ├─ worker.py
│  ├─ keepalive.py
│  ├─ web.py
│  └─ templates/index.html
└─ scripts/
   └─ install_systemd.sh
```

---

## ⚡ 快速开始

### 1. 插上 USB 上网卡

获取设备名称：

```bash
dmesg | grep tty
```

常见：`/dev/ttyUSB0` 或 `/dev/ttyACM0`

### 2. 配置文件

复制：

```bash
cp config.example.yaml config.yaml
```

填入串口、Telegram token、推送配置等。

### 3. Docker 方式运行（推荐玩客云）

```bash
docker build -t sms-bridge .
docker run \
  --device=/dev/ttyUSB0:/dev/ttyUSB0 \
  -p 8080:8080 \
  -v $(pwd)/data:/data \
  sms-bridge
```

访问：`http://设备IP:8080`

### 4. 直接运行（无 Docker）

```bash
pip install -r requirements.txt
python app/web.py
```

---

## 🖥️ Web UI 预览

- 左侧：短信记录（带方向区分）
  
- 右侧：发送短信、保号按钮
  
- 顶部：启动 / 停止监听
  
- 自动刷新最新短信
  

前端已使用轻量现代风格美化。

---

## 🔧 高级功能

### 🌐 Telegram 转发

收到短信后自动发送到你的 Telegram 机器人：

```yaml
telegram:
  enabled: true
  bot_token: "xxxx"
  chat_id: "123456"
```

### 📡 微信（PushPlus）转发

```yaml
wechat_pushplus:
  enabled: true
  token: "xxxx"
```

### 🔄 一键保号

点击网页按钮即可发一条短信完成运营商“保号任务”。

### 🧠 Modem 解析增强

- 多设备兼容
  
- 更 robust 的 AT 输出解析
  
- 支持不规范换行、不同厂商格式
  
- 避免漏掉短信
  

---

## 🐳 docker-compose 示例

```yaml
version: '3.8'
services:
  smsbridge:
    build: .
    devices:
      - "/dev/ttyUSB0:/dev/ttyUSB0"
    ports:
      - "8080:8080"
    volumes:
      - ./data:/data
    restart: unless-stopped
```

---

## 📝 License

MIT

---
