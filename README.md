# doc-transfer-h5

「文档互传」小程序 · **从手机文件夹选择**用的 H5 页面（web-view 导入）。

微信小程序原生读不到手机/电脑的文件目录，因此用 `web-view` 打开这个页面，
页面里用 `<input type="file">` 选文件 → 上传到云函数 `uploadFromWeb` → 回到小程序转换。

## 这个仓库为什么独立存在

它只是为了给**云开发静态网站托管**提供一个可部署的站点源：
托管支持「从 Git 仓库部署」，仓库根目录即站点根。单独放一个仓库，
避免和别的项目混在一起导致部署错内容。

## 内容

| 文件 | 说明 |
|---|---|
| `index.html` | 站点入口，内容等同于主项目的 `h5/web-import.html` |

> 页面**不需要在文件里写上传地址**：小程序会通过 URL 参数 `?upload=...` 注入，
> 具体地址来自主项目的 `miniprogram/config/env.js` 的 `uploadFromWebUrl`。

## 部署

云开发控制台 → 静态网站托管 → 从 Git 仓库部署：
- 仓库：本仓库，分支 `master`
- 部署目录：`/`（仓库根）
- 构建命令 / 安装命令：**留空**（纯静态文件，无需构建）

部署后站点地址形如 `https://<默认域名>/`，把它填进主项目的
`miniprogram/config/env.js` 的 `webImportUrl`。

## 修改流程

主项目里改完 `h5/web-import.html` 后，把它同步到这里：

```powershell
Copy-Item "..\miniprogram-doc-transfer\h5\web-import.html" .\index.html -Force
git add index.html
git commit -m "sync: 同步 H5 页面"
git push
```
