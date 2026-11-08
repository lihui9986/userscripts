# userscripts

油猴（Tampermonkey / Violentmonkey / Greasemonkey）脚本仓库，用于给脚本提供**自动更新源**。

仓库必须保持 **public**，否则 `raw.githubusercontent.com` 无法匿名访问，油猴检查更新会失败。

## 目录

```
scripts/
├─ template.user.js   完整脚本（用户实际安装 / 更新下载的文件）
└─ template.meta.js   仅元数据块（油猴检查更新时只拉这个小文件，省流量）
```

每个脚本一对文件：`名字.user.js` + `名字.meta.js`。新增脚本时复制 template 这一对并改名。

## 脚本头部必须有的三行

```js
// @version      0.1.0
// @updateURL    https://raw.githubusercontent.com/lihui9986/userscripts/main/scripts/template.meta.js
// @downloadURL  https://raw.githubusercontent.com/lihui9986/userscripts/main/scripts/template.user.js
```

- `@updateURL` → 指向 `.meta.js`，油猴定期请求它，只比较 `@version`。
- `@downloadURL` → 指向 `.user.js`，发现新版本时下载完整脚本。
- 安装入口（浏览器打开会弹出安装）：<https://raw.githubusercontent.com/lihui9986/userscripts/main/scripts/template.user.js>

> 国内访问 `raw.githubusercontent.com` 可能不稳定。备用 CDN（jsDelivr）：
> `https://cdn.jsdelivr.net/gh/lihui9986/userscripts@main/scripts/template.meta.js`
> `https://cdn.jsdelivr.net/gh/lihui9986/userscripts@main/scripts/template.user.js`
> jsDelivr 有缓存，发版后建议打 tag（如 `v0.1.0`）并把 URL 里的 `@main` 换成 `@v0.1.0`，避免更新延迟。

## 发一次更新的流程

1. 改脚本代码。
2. **同时**把 `template.user.js` 和 `template.meta.js` 里的 `@version` 改成更大的号（如 `0.1.1`）。两处不一致会导致油猴反复提示更新或检测不到。
3. `git add -A && git commit -m "release 0.1.1" && git push`。
4. 油猴会在下次检查周期拉取 `@updateURL`；也可在管理页对脚本点「检查更新」立即验证。

## 常见坑

| 现象 | 原因 |
|------|------|
| 一直不更新 | `@version` 没递增，或只改了 `.user.js` 忘了改 `.meta.js` |
| 404 | 仓库是 private、分支名不是 `main`、或文件名/路径写错 |
| 更新有延迟 | GitHub raw 有约 5 分钟缓存；jsDelivr 缓存更久 |
| 打开 raw 链接是下载而非安装 | 文件名必须以 `.user.js` 结尾，且油猴扩展已启用 |
| 改了名字后旧脚本失联 | `@namespace` + `@name` 是脚本身份，改了会被当成新脚本 |

## 本地与远程

- 本地仓库：`D:\shopee\userscripts`
- 远程：<https://github.com/lihui9986/userscripts>
- 默认分支：`main`
