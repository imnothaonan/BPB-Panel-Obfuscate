> [!WARNING]
> **锁定 v4.2.3，不跟进 v5。**
> BPB Panel 从 v5.0.0 起禁止手动部署 —— 上游 `RELEASE.md` 原文：*"manual deployment and BPB Wizard client deployments do not work on this release"*，只能通过 [BPB Wizard v3+](https://wizard.bpb-panel.workers.dev) 一键安装。
> 因此凡是把 `worker.js` 手动贴进 Cloudflare 的用法（也就是本仓库的用途），**只能用 v4 线**，v4.2.3 是最后一个 v4 版本。

# BPB Panel Obfuscated Worker (v4.2.3)

自动构建 [BPB Worker Panel](https://github.com/bia-pain-bache/BPB-Worker-Panel) **v4.2.3** 的混淆版 `_worker.js`，供手动部署到 Cloudflare Workers 使用。

## 工作方式

`.github/workflows/Obfuscate.yml` 每天 01:00 UTC（北京时间 09:00）以及每次向 `main` 分支推送时自动执行：

1. 拉取上游 **v4.2.3** release 产物 `worker.js`（`releases/download/v4.2.3/worker.js`，约 257KB）
2. 用 `javascript-obfuscator` 做高强度混淆（控制流平坦化、死代码注入、RC4 字符串数组、十六进制标识符等）
3. 把 `origin.js`（未混淆原件）和 `_worker.js`（混淆产物）一起自动提交回 `main`

> 地址是**写死 v4.2.3** 的，不能用 `releases/latest` —— 那个现在指向 v5.1.1，产出的文件装不上。

## 部署（手动，四步）

1. Cloudflare Dashboard → **Workers & Pages** → Create → Worker，把 `_worker.js` 内容整体粘贴
2. **KV 绑定**：新建 KV Namespace，绑定到该 Worker，变量名必须是 `kv`（v4 用 `env.kv` 存 `proxySettings` / `warpAccounts` / `pwd`）
3. **环境变量**：
   - `UUID` —— VLESS UUID（必填）
   - `TR_PASS` —— Trojan 密码（必填）
   - `SUB_PATH`、`FALLBACK` —— 可选
4. 访问 `https://<你的worker域名>/panel` 进入面板（v4 是 `/panel`，不是 v5 的 `/SECURE_PATH/panel`）

> 面板密码存在 KV 的 `pwd` 键里，不在环境变量。

## 手动触发

仓库页面 → Actions → `Build Obfuscate BPB Panel` → Run workflow。

## 说明

混淆仅降低源码可读性，不改变功能。上游 v5 的新特性（面板内自动更新、Telegram Bot、自定义域名、SECURE PATH 等）本仓库产出的文件**都没有**，想要这些只能走官方 Wizard。
