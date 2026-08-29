# 🕵️ 卧底风云 · 多人联机版

> **v3** — 3-8 人跨设备实时同步的卧底游戏，基于 Firebase Realtime Database。

## 🎯 玩法（v3 规则速览）

- **3-8 人动态开局**：每人输入昵称 + 房间号加入，房主点「🚀 开始游戏」
- **卧底数 = ceil(n/5)**：5 人 1 个卧底、8 人 2 个卧底… 至少 1 个
- **翻牌隐私**：永远只有「自己」能翻自己的卡牌，他人卡牌对其他人都显示 🔒 ???
- **累计投票**：每玩家 1 票，可改投，得票 **超过** N/2 的人被票出（卧底 → 平民胜；冤票平民 → 间谍胜）
- **平票 / 没过半**：平票自动清空重投；都投完但没人过半 → 间谍胜
- **仅卧底可猜词**：平民点猜词按钮被拒；卧底随时输入猜测平民的词 → 猜对 / 猜错即决胜负
- **退出房间**：顶部 🚪 按钮释放槽位；所有人走完则删房间

## 🎮 完整流程

1. 任意数量（3-8）玩家输入**相同的房间号**（如 `8888`）加入房间
2. 第一个进入的玩家成为"房主"，满 3 人后可点「🚀 开始游戏」
3. 系统按人数随机分配词语 + 卧底，每玩家点开自己的卡牌查看身份
4. 进入投票轮 — 点对应玩家按钮投票（每人 1 票，可改）
5. 所有人都投完后自动结算（如有玩家中途不想点，结算仍按"全员投完"触发）
6. 或卧底在「间谍猜词」面板输入猜测词 → 同步判定胜负
7. 弹窗点「🔄 再来一局」可保留人数 + 昵称，重置词和卧底

---

## 📦 部署步骤（5 分钟搞定）

### 1. 创建 Firebase 项目
1. 访问 [Firebase Console](https://console.firebase.google.com/)
2. 点击 **「添加项目」** → 随便起个名字（如 `undercover-game`）→ 完成
3. 在项目里点击 **「Build」→「Realtime Database」→「Create Database」**
4. 选择位置（推荐 `asia-southeast1` 新加坡，国内延迟低）→ 下一步
5. **安全规则选「测试模式」**（允许读写 30 天）→ 启用

### 2. 获取 Web 应用配置
1. 项目首页点击 **「</>」**（Web 应用图标）→ 起个昵称 → 注册应用
2. 复制 `firebaseConfig` 对象：
   ```javascript
   const firebaseConfig = {
     apiKey: "AIzaSy...",
     authDomain: "your-project.firebaseapp.com",
     databaseURL: "https://your-project-default-rtdb.firebaseio.com",
     projectId: "your-project",
     storageBucket: "your-project.appspot.com",
     appId: "1:1234567890:web:abc123"
   };
   ```

### 3. 替换代码里的占位符
打开 `index.html`，找到这段 `const firebaseConfig = { ... }`，把对应的字段替换成你真实的值。

### 4. 部署到 GitHub Pages
```bash
git init
git add index.html README.md
git commit -m "init: undercover multiplayer v3"
git branch -M main
git remote add origin https://github.com/<你的用户名>/undercover-online.git
git push -u origin main
```
然后 GitHub 仓库 → Settings → Pages → Source 选 `main` 分支根目录 → 保存。

访问地址：`https://<你的用户名>.github.io/undercover-online/`

### 5. 测试
1. 用电脑打开你的 GitHub Pages 链接，输入房间号 `8888` + 昵称 → 加入
2. 用手机（不同浏览器/无痕窗口）打开同一链接，输入相同房间号 + 不同昵称 → 加入
3. 满 3 人后点「🚀 开始游戏」即可开始 🎉

---

## 🔓 Firebase 安全规则

测试模式（默认 30 天后过期）：
```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

**生产环境建议**：
```json
{
  "rules": {
    "rooms": {
      "$roomId": {
        ".read": true,
        ".write": true,
        ".indexOn": ["gameOver"]
      }
    }
  }
}
```
在 Firebase Console → Realtime Database → Rules 标签页替换。

---

## 🛠️ 技术栈

- **前端**：原生 HTML + CSS + JavaScript（无任何框架）
- **数据库**：Firebase Realtime Database（实时同步）
- **CDN**：Firebase JS SDK 10.13.0 compat 版本
- **部署**：任意静态托管（GitHub Pages / Netlify / Vercel / 本地服务器）

## ✨ 特性

- 🎴 3D 翻转卡牌 + 毛玻璃 + 发光特效
- 📱 手机 / 电脑双端自适应
- 🌌 深色渐变背景 + 紫蓝光斑装饰
- 📚 60+ 内置词库（食物 / 物品 / 动物 / 日常）
- 🔄 实时多端同步（任意玩家操作，其余设备立即刷新）
- 💾 刷新页面自动恢复房间（localStorage 记住房间号 + 昵称 + 玩家编号）
- 🚪 退出房间 + 自动清空

## 📄 License

MIT
