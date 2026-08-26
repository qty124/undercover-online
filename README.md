# 🕵️ 卧底风云 · 三人联机版

> 三人跨设备实时同步的卧底游戏，基于 Firebase Realtime Database。

## 🎮 玩法

1. 三人输入**相同的房间号**（如 `8888`）加入房间
2. 第一个进入的玩家自动创建房间，随机分配词语和间谍
3. 任意玩家点击任意卡牌 → 翻牌状态会**实时同步**给所有人
4. 投票抓间谍 或 间谍盲猜词语 → 同步出胜负
5. 点 **🔄 再来一局** 重置当前房间（保留玩家位置）

> ⚠️ 第 4 个人尝试加入同一房间号会被拒绝，提示"房间已满"。

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
2. 复制 `firebaseConfig` 对象，类似：
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
打开 `index.html`，找到这段：
```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",          // ← 替换
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  databaseURL: "https://YOUR_PROJECT-default-rtdb.firebaseio.com",
  projectId: "YOUR_PROJECT",
  storageBucket: "YOUR_PROJECT.appspot.com",
  appId: "YOUR_APP_ID"
};
```
把 `YOUR_API_KEY` / `YOUR_PROJECT` / `YOUR_APP_ID` 替换成你真实的值。

### 4. 部署到 GitHub Pages
```bash
# 新建一个仓库，比如 undercover-online
git init
git add index.html README.md
git commit -m "init: undercover multiplayer"
git branch -M main
git remote add origin https://github.com/<你的用户名>/undercover-online.git
git push -u origin main
```
然后 GitHub 仓库 → Settings → Pages → Source 选 `main` 分支根目录 → 保存。

访问地址：`https://<你的用户名>.github.io/undercover-online/`

### 5. 测试
1. 用电脑打开你的 GitHub Pages 链接，输入房间号 `8888` → 加入
2. 用手机（不同浏览器/无痕窗口）打开同一链接，输入相同房间号 → 加入
3. 三人都到位后即可开始游戏 🎉

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

**生产环境建议**（防止被恶意清空）：
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
- 📚 24 个内置词库（食物 / 物品 / 动物 / 日常）
- 🔄 实时多端同步（任意玩家操作，其余设备立即刷新）
- 💾 刷新页面自动恢复房间（localStorage 记住玩家编号）

## 📄 License

MIT