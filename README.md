# 🔧 Clash Mobile Config

iPhone 友好的 Clash 配置文件，支持 Stash、Shadowrocket、Surge 等客户端。

## 📱 支持的客户端

| 客户端 | iOS | 推荐度 | 说明 |
|--------|-----|--------|------|
| **Stash** | ✅ | ⭐⭐⭐⭐⭐ | 最稳定的 Clash 实现 |
| **Shadowrocket** | ✅ | ⭐⭐⭐⭐ | 功能完整，价格便宜 |
| **Surge** | ✅ | ⭐⭐⭐⭐ | 功能最完整，价格较贵 |
| **Quantumult X** | ✅ | ⭐⭐⭐ | 支持订阅和规则集 |

## 🚀 快速开始

### 方法 1：GitHub 导入（推荐）

1. **打开 Stash / Shadowrocket**
2. **点击 "配置" → "从 URL 导入"**
3. **复制以下链接：**
   ```
   https://raw.githubusercontent.com/Joseph19820124/clash-mobile-config/main/config.yaml
   ```
4. **粘贴 → 导入 → 完成**

### 方法 2：直接编辑配置文件

1. **下载此仓库**
   ```bash
   git clone https://github.com/Joseph19820124/clash-mobile-config.git
   ```

2. **编辑 `config.yaml`**
   - 修改代理节点信息
   - 调整规则和策略组

3. **导入到 iPhone**
   ```bash
   # 使用 AirDrop 发送给 iPhone
   # 或复制到 iCloud Drive，在 Stash 中打开
   ```

---

## 📝 配置说明

### 核心配置项

| 配置项 | 说明 | iPhone 推荐值 |
|--------|------|--------------|
| `port` | HTTP 代理端口 | 7890 |
| `socks-port` | SOCKS5 代理端口 | 7891 |
| `allow-lan` | 允许 LAN 访问 | `true`（iPhone 本地用） |
| `mode` | 模式：rule/global/direct | `rule`（规则模式） |
| `log-level` | 日志级别 | `info`（iPhone 省电） |

### 代理节点类型

```yaml
# Shadowsocks
- name: "SS"
  type: ss
  server: 1.2.3.4
  port: 443
  cipher: aes-256-gcm
  password: "password"

# VMess
- name: "VMess"
  type: vmess
  server: 1.2.3.4
  port: 443
  uuid: "xxxx-xxxx-xxxx-xxxx"
  alterId: 0
  cipher: auto

# Trojan
- name: "Trojan"
  type: trojan
  server: 1.2.3.4
  port: 443
  password: "password"
  skip-cert-verify: false
```

### 规则优先级

```
1. 广告过滤（REJECT）
2. 国内 IP / 域名（DIRECT）
3. 特定服务（Apple / Microsoft 等）
4. 国际网站（代理）
5. 兜底规则（MATCH）
```

---

## 🎯 配置自定义

### 1️⃣ 替换代理节点

编辑 `config.yaml` 中的 `proxies` 部分：

```yaml
proxies:
  - name: "你的节点名称"
    type: ss  # 或 vmess / trojan
    server: your.proxy.server
    port: 443
    password: "your-password"  # 或 uuid
    cipher: aes-256-gcm
```

**获取节点信息的方式：**
- 订阅链接（推荐）→ 导入为订阅
- 从代理提供商复制节点信息
- 手动添加个人服务器

### 2️⃣ 修改规则组

```yaml
proxy-groups:
  - name: "🔀 Select"
    type: select
    proxies:
      - "你的节点1"
      - "你的节点2"
      - "DIRECT"
```

### 3️⃣ 使用订阅链接

如果有现成的订阅链接，在客户端中导入：

**Stash：**
- 点击 "配置" → "订阅"
- 添加订阅链接
- 自动更新代理节点列表

---

## 💡 最佳实践

### ✅ DO

- ✅ **定期更新规则集**（自动更新间隔 86400s = 1 天）
- ✅ **使用 URL 导入**（配置更新自动同步）
- ✅ **启用 DNS**（提高解析速度）
- ✅ **测试延迟**（使用 "⚡ Auto" 自动选择）
- ✅ **加密敏感信息**（不要上传包含密码的配置）

### ❌ DON'T

- ❌ **不要在 GitHub 上公开完整的代理节点密码**
- ❌ **不要启用 "allow-lan" 在不安全的 WiFi**
- ❌ **不要频繁更改模式**（影响网络体验）
- ❌ **不要禁用 DNS**（可能导致无法解析）

---

## 🔧 故障排除

### 问题 1：导入后无法上网

**原因：** 代理节点配置不正确

**解决：**
1. 检查代理服务器地址是否正确
2. 检查端口和密码
3. 在 "延迟测试" 中查看节点是否可用

### 问题 2：某些网站访问慢

**原因：** 规则或代理选择不优化

**解决：**
1. 使用 "⚡ Auto" 自动选择最快节点
2. 检查规则集是否有冲突
3. 增加 DNS 超时时间

### 问题 3：规则集无法更新

**原因：** 网络问题或 CDN 被屏蔽

**解决：**
1. 检查网络连接
2. 修改规则集 URL（使用备用 CDN）
3. 手动刷新配置

---

## 📊 配置示例

### 场景 1：只有 1 个代理节点

```yaml
proxies:
  - name: "My-Proxy"
    type: ss
    server: proxy.example.com
    port: 443
    cipher: aes-256-gcm
    password: "password123"

proxy-groups:
  - name: "🔀 Select"
    type: select
    proxies:
      - "My-Proxy"
      - "DIRECT"

rules:
  - MATCH,🔀 Select
```

### 场景 2：多个代理节点 + 自动切换

```yaml
proxy-groups:
  - name: "⚡ Auto"
    type: url-test
    proxies:
      - "Proxy-1"
      - "Proxy-2"
      - "Proxy-3"
    url: "http://www.gstatic.com/generate_204"
    interval: 300

rules:
  - MATCH,⚡ Auto
```

---

## 📚 参考资源

- [Clash 官方文档](https://github.com/Dreamacro/clash)
- [Clash for Windows 文档](https://docs.cfw.lbyczf.com/)
- [Stash 文档](https://stash.wiki/)
- [Shadowrocket 指南](https://merlinblog.com/)

---

## 🔐 安全提示

- 不要在公开仓库中存储包含密码的完整配置
- 不要在 iPhone 上运行不信任的配置
- 定期检查已安装的规则集来源
- 使用强密码保护代理账户

---

## 📞 支持

有问题或建议？

- 提交 Issue：https://github.com/Joseph19820124/clash-mobile-config/issues
- 讨论：https://github.com/Joseph19820124/clash-mobile-config/discussions

---

**最后更新：** 2026-06-06  
**维护者：** Joseph Chen
