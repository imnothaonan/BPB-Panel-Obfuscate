# BPB Panel Obfuscated Worker

自动构建 [BPB Worker Panel](https://github.com/bia-pain-bache/BPB-Worker-Panel) 的混淆版 `_worker.js`，供 Cloudflare Workers / Pages 部署使用。

## 工作方式

`.github/workflows/Obfuscate.yml` 每天 01:00 UTC（北京时间 09:00）以及每次向 `main` 分支推送时自动执行：

1. 拉取上游 release 产物 `worker.js`（`releases/latest/download/worker.js`）
2. 用 `javascript-obfuscator` 做高强度混淆（控制流平坦化、死代码注入、RC4 字符串数组、十六进制标识符等）
3. 把产物 `_worker.js` 自动提交回 `main`

## 使用

在 Cloudflare Dashboard 新建 Worker，粘贴仓库中 `_worker.js` 的内容（或配置 CI 直接部署），然后按 BPB 面板文档设置面板密码与代理 IP。

## 手动触发

仓库页面 → Actions → `Build Obfuscate BPB Panel` → Run workflow。

## 上游更新

上游代码变动后无需任何操作，定时任务会自动跟进。混淆仅用于减小源码可读性，不改变功能。
