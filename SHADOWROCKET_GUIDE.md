# 📱 Shadowrocket 导入指南

专为 Shadowrocket 优化的 Clash 配置导入步骤。

---

## ⚡ 一键导入（推荐）

### 方法 1：直接使用 Shadowrocket URL Scheme

复制以下链接到浏览器或 Safari：

```
shadowrocket://import-profile/?url=https://raw.githubusercontent.com/Joseph19820124/clash-mobile-config/master/config-shadowrocket.yaml
```

或者在 Shadowrocket 内：

```
https://raw.githubusercontent.com/Joseph19820124/clash-mobile-config/master/config-shadowrocket.yaml
```

---

## 📲 分步导入指南

### 步骤 1：打开 Shadowrocket

在 iPhone 主屏幕找到 **Shadowrocket** 应用，点击打开。

### 步骤 2：进入配置管理

点击左下角 **"风车"** 图标 → **"配置"** 选项卡

（或直接点击主页面的配置列表区域）

### 步骤 3：添加新配置

点击右上角 **"+"** 按钮，选择 **"添加配置文件"**

### 步骤 4：粘贴配置 URL

选择 **"从 URL 导入"** 选项，粘贴以下链接：

```
https://raw.githubusercontent.com/Joseph19820124/clash-mobile-config/master/config-shadowrocket.yaml
```

### 步骤 5：确认导入

- 输入配置名称（默认可以接受）
- 点击 **"导入"** 或 **"保存"**
- 等待导入完成

### 步骤 6：激活配置

- 返回配置列表
- 点击导入的配置，使其变为 **"活跃"** 状态（绿色）
- 或向左滑动配置，点击 **"使用"**

### 步骤 7：启用 VPN

点击主屏幕的 **"未连接"** 按钮启用代理

iPhone 状态栏会显示 VPN 图标 🔒（表示连接成功）

---

## 🔧 Shadowrocket 配置说明

### 代理节点类型

#### Shadowsocks (SS)
```yaml
- name: "SS-Node"
  type: ss
  server: 1.2.3.4
  port: 443
  cipher: aes-256-gcm
  password: "your-password"
```

支持的加密方式：
- `aes-256-gcm` ✅ 推荐
- `aes-128-gcm`
- `chacha20-poly1305`
- `aes-256-cbc`

#### VMess
```yaml
- name: "VMess-Node"
  type: vmess
  server: 1.2.3.4
  port: 443
  uuid: "xxxx-xxxx-xxxx"
  alterId: 0
  cipher: auto
```

#### Trojan
```yaml
- name: "Trojan-Node"
  type: trojan
  server: 1.2.3.4
  port: 443
  password: "your-password"
  skip-cert-verify: false
```

#### VLESS（部分支持）
```yaml
- name: "VLESS-Node"
  type: vless
  server: 1.2.3.4
  port: 443
  uuid: "xxxx-xxxx-xxxx"
  tls: true
```

---

## 📋 规则系统说明

### Shadowrocket 支持的规则类型

| 规则类型 | 示例 | 说明 |
|---------|------|------|
| `DOMAIN-SUFFIX` | `DOMAIN-SUFFIX,google.com,Proxy` | 域名后缀匹配 |
| `DOMAIN` | `DOMAIN,www.google.com,Proxy` | 完整域名匹配 |
| `DOMAIN-KEYWORD` | `DOMAIN-KEYWORD,google,Proxy` | 域名关键字匹配 |
| `IP-CIDR` | `IP-CIDR,1.2.3.0/24,DIRECT` | IP 段匹配 |
| `GEOIP` | `GEOIP,US,Proxy` | 地理位置匹配 |
| `MATCH` | `MATCH,Proxy` | 兜底规则 |

### 规则执行顺序

```
1. 规则列表中从上到下依次匹配
2. 第一个匹配的规则生效
3. 如果都不匹配，执行 MATCH 规则
```

**因此，顺序很重要！** 把更具体的规则放在前面。

### 规则优化建议

```yaml
rules:
  # 1. 最具体的规则（完整域名）放最前
  - DOMAIN,very-specific.example.com,DIRECT

  # 2. 其次是关键字匹配
  - DOMAIN-KEYWORD,example,DIRECT

  # 3. 最后是通用规则（后缀匹配）
  - DOMAIN-SUFFIX,example.com,DIRECT

  # 4. IP 段和地理位置
  - GEOIP,CN,DIRECT

  # 5. 兜底规则
  - MATCH,Proxy
```

---

## 🎛️ Shadowrocket 高级设置

### 启用/禁用功能

**主屏幕 → 左下角齿轮 → 设置**

#### 常用设置

| 功能 | 推荐值 | 说明 |
|------|--------|------|
| **Proxyless CN** | ON | 直连国内流量（省流量） |
| **VPN Mode** | Packet Tunnel | 更稳定（推荐） |
| **Enhanced Mode** | Auto | 自动选择最优模式 |
| **Auto Connect** | ON | 自动连接代理 |
| **Show Icon** | ON | 显示状态栏图标 |

#### DNS 设置

点击 **"DNS"** → 修改 DNS 服务器

```
推荐使用：
- 8.8.8.8（Google DNS）
- 1.1.1.1（Cloudflare DNS）
- 8.8.4.4（Google 备用）
```

---

## 🔄 更新配置

### 自动更新

Shadowrocket 可以设置自动更新间隔：

1. 配置列表 → 向左滑动配置
2. 点击 **"更新"** → 选择 **"自动更新"** 间隔
3. 可选：1小时、6小时、12小时、24小时、手动

### 手动更新

1. 配置列表 → 向左滑动配置
2. 点击 **"更新"** 或 **"刷新"** 按钮

---

## 🧪 测试连接

### 测试代理节点延迟

1. 配置列表 → 点击 **"延迟测试"** 或 **"速度测试"**
2. 应用会测试所有节点的延迟（ms）
3. 红色表示无法连接
4. 绿色表示可用，数字越小越好

### 验证代理是否生效

打开浏览器访问：

```
https://ipinfo.io
```

或

```
https://whoer.net
```

应该看到代理节点的 IP 地址，而不是你的本地 IP。

---

## ❌ 常见问题排查

### Q1: 导入失败 - "无效的配置"

**原因：** URL 格式错误或网络问题

**解决方案：**
1. 检查 URL 是否完整（不要有多余空格）
2. 确保 iPhone 有网络连接
3. 尝试在 Safari 中打开 URL 看是否能下载
4. 等待几秒后重试

### Q2: 导入成功但无法连接

**原因：** 代理节点信息不正确或节点已失效

**解决方案：**
1. 运行 "延迟测试"，检查节点是否可用
2. 确认代理节点参数正确（服务器、端口、密码、uuid 等）
3. 尝试手动添加一个已知可用的节点测试
4. 检查防火墙或ISP是否阻止了连接

### Q3: 某些网站打不开

**原因：** 规则配置不合理

**解决方案：**
1. 检查该网站的域名是否在规则中
2. 尝试添加特定规则：`DOMAIN-SUFFIX,example.com,🌍 Global`
3. 临时切换到 `DIRECT`（直连）测试
4. 检查是否有拼写错误

### Q4: 连接后网速很慢

**原因：** 可能选择的代理节点距离远或节点负载高

**解决方案：**
1. 使用 "⚡ Auto" 自动选择延迟最低的节点
2. 尝试其他可用节点
3. 检查是否启用了代理的日志（会影响性能）
4. 减少同时连接数

### Q5: 无法导入订阅

**原因：** Shadowrocket 的订阅功能可能不支持

**解决方案：**
1. 使用配置文件导入（而不是订阅）
2. 手动添加节点
3. 考虑升级到最新版本

---

## 📊 配置文件对比

### 两个版本的区别

| 特性 | config.yaml | config-shadowrocket.yaml |
|------|------------|------------------------|
| **规则集支持** | 完整（Rule Providers） | 简化版（手写规则） |
| **DNS 配置** | 高级 | 简化 |
| **兼容性** | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **规则数量** | 多（自动更新） | 少（手写）|
| **推荐客户端** | Stash | **Shadowrocket** |

**建议：** 用 `config-shadowrocket.yaml` 导入 Shadowrocket

---

## 🎯 快速参考

### 必改项

```yaml
proxies:
  - name: "Your Node Name"
    type: ss  # 改为你的节点类型
    server: your.proxy.server  # ← 改这里
    port: 443
    password: "your-password"  # ← 改这里
```

### 可选优化

```yaml
# 调整自动选择延迟测试间隔（秒）
interval: 300  # 每 5 分钟测试一次

# 调整允许的延迟波动（毫秒）
tolerance: 50  # 超过基础延迟 50ms 就切换
```

---

## 📞 需要帮助？

- **GitHub Issues：** https://github.com/Joseph19820124/clash-mobile-config/issues
- **Shadowrocket 文档：** App 内帮助菜单
- **社区讨论：** GitHub Discussions

---

**祝你使用愉快！** 🚀

