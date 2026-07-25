# 退休计划 APP

一个为退休生活量身打造的轻量工作台。四大模块，莫兰迪配色，简洁实用。

## 功能

| 模块 | 图标 | 说明 |
|---|---|---|
| 工作日程 | 📋 | 按日期记录任务，可勾选完成、删除；前一日 17:00 提醒次日日程 |
| 排便记录 | 🚽 | 月视图每日打卡；每天 21:00 提醒打卡；统计当月次数与连续天数 |
| 收支记录 | 💰 | 按月账单，每月自动归零；可查看历史月份；当月 >1000 元时提醒 |
| 学术进度 | 📚 | 论文列表管理，含标题、期刊、状态、日期、链接；支持状态流转 |

## 设计

- **莫兰迪低饱和配色**：蓝灰 / 绿 / 粉灰 / 紫灰 区分四大模块
- **主体白色**：白底卡片 + 圆角 + 留白
- **响应式**：手机底部 Tab + 桌面侧边栏切换
- **emoji 图标**：通用、直观、轻松

## 数据存储

所有数据存储在浏览器 localStorage（按域隔离），无需后端、无需登录、清缓存会丢数据。建议定期截图或导出备份。

## 提醒机制

应用通过 Web Notifications API 在以下时点触发系统通知：
- **17:00-23:59**：若次日有未完成任务，提醒"明日日程"
- **21:00-23:59**：若当日未打卡，提醒"排便打卡"
- **任意时间**：当月支出累计 >1000 元时提醒"月度超额"

> ⚠️ 浏览器限制：通知需要应用在前台或最近打开过才能触发。若关闭浏览器，通知将不会弹出。建议将应用添加到主屏幕长期使用。

## 添加到手机桌面

### iOS Safari
1. 在 Safari 中打开应用 URL
2. 点击底部分享按钮（方框带向上箭头）
3. 选择 **"添加到主屏幕"**
4. 名称默认 "退休计划"，点击右上角 **添加**
5. 主屏幕出现 🍵 图标，点击即可全屏打开

### Android Chrome
1. 在 Chrome 中打开应用 URL
2. 浏览器右上角菜单 → **"添加到主屏幕"** 或 **"安装应用"**
3. 确认安装，主屏幕出现图标

> 添加到桌面后，应用以独立窗口运行（无浏览器栏），体验接近原生 APP。

## 本地预览（开发者）

```bash
cd retirement-app
python3 -m http.server 8000
# 浏览器访问 http://localhost:8000
```

直接双击 `index.html` 也可以打开，但 PWA（添加到主屏幕、Service Worker 缓存）功能需要 HTTP 服务。

## 文件结构

```
retirement-app/
├── index.html            # 主应用（HTML + CSS + JS 单文件）
├── manifest.json         # PWA 清单
├── sw.js                 # Service Worker（离线缓存）
├── icon-192.png          # 应用图标 192×192
├── icon-512.png          # 应用图标 512×512
├── icon-maskable-192.png # maskable 图标 192×192
├── icon-maskable-512.png # maskable 图标 512×512
└── README.md             # 本文件
```

## 浏览器兼容性

- iOS Safari 14+
- Android Chrome 90+
- Desktop Chrome / Edge / Safari / Firefox 现代版

## 数据导出建议

未来若需要数据迁移/备份，可在浏览器控制台运行：
```js
const data = {};
for (let i = 0; i < localStorage.length; i++) {
  const k = localStorage.key(i);
  if (k.startsWith('rp_')) data[k] = JSON.parse(localStorage.getItem(k));
}
console.log(JSON.stringify(data, null, 2));
// 复制输出作为备份
```