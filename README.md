# Local Drive Sync

给 Google Auth Platform 品牌验证用的静态网站。页面只使用产品名 Local Drive Sync，不含姓名、邮箱或账号标识。

调用 Google 云端硬盘的程序是用户自己电脑上的 [rclone](https://rclone.org/drive/#making-your-own-client-id)。这个仓库只提供同意屏幕要链接的首页、隐私权政策和服务条款。

## 填进品牌页

应用名称必须和网页上的标题一致，原样填写：

```text
Local Drive Sync
```

不要把应用名称写成 `rclone` 或 `Google Drive`。同意屏幕上的名字应当表示这个客户端本身，而不是 rclone 项目或 Google。

部署完成、浏览器地址栏不再跳到别的域名之后，把这三个地址原样粘贴到品牌页：

| 品牌页字段 | 地址 |
| --- | --- |
| 应用首页 | `https://<主机>/<仓库路径>/` |
| 应用隐私权政策链接 | `https://<主机>/<仓库路径>/privacy.html` |
| 应用服务条款链接 | `https://<主机>/<仓库路径>/terms.html` |
| 已获授权的网域 | `<主机>` |

用户站或自定义域名如果没有仓库路径，就去掉路径那一段。已获授权的网域只填主机名，不带 `https://`，不带路径。隐私权政策必须和首页不同 URL，但必须在同一个域名上。

徽标留空。不要上传 Google 或 rclone 的标志。

用户支持邮箱保持控制台里已经填写的那一个。不要把邮箱、姓名或账号写进这个仓库。页面上的联系方式故意写成“同意屏幕上的用户支持邮箱”，这样公开网页不出现个人资料，授权的人仍然能在同意屏幕上看到同一个地址。

## 发布到 GitHub Pages

1. 不要提交客户端密钥、OAuth JSON、rclone 配置或令牌。仓库一旦公开，里面的每个文件都会公开。免费的 GitHub Pages 需要公开仓库。
2. 在仓库 Settings → Pages 里选择从分支发布，分支用包含这些文件的分支，目录选 `/ (root)`。
3. 等 `index.html`、`privacy.html`、`terms.html` 都能直接打开。三个页面都不需要登录。

项目页的地址形如 `https://<拥有者>.github.io/<仓库名>/`。仓库名本身是 `<拥有者>.github.io` 时，地址是 `https://<拥有者>.github.io/`。

页面之间用相对链接，所以放在项目页或域名根目录都可以。不要把首页做成像其他域名的跳转。Google 要求同意屏幕上的首页 URL 打开后仍停在同一个域名。

## `github.io` 可能无法当作授权域名

Google 要求首页、隐私权政策和服务条款所在的域名能证明是你的，并写明不要把首页放在无法验证所有权的第三方平台上。`github.io` 是公共后缀。品牌页对 `*.github.io` 常返回「域名无效」或 Domain not found，即使 Search Console 已经用 HTML 文件验证过 URL 前缀。

能对上授权域名检查的做法，是使用一个你自己注册的域名：

1. 用 CNAME 把该域名指到 GitHub Pages。
2. 用这个 Cloud 项目的 Owner 或 Editor 账号，在 [Google Search Console](https://search.google.com/search-console) 验证该域名。
3. 品牌页改填 `https://<你的域名>/`、`https://<你的域名>/privacy.html`、`https://<你的域名>/terms.html`，授权域名填 `<你的域名>`。

本仓库的 HTML 没有写死主机名，换域名时不用改页面。

## 数据访问和客户端类型

在数据访问里只添加这一个范围：

```text
https://www.googleapis.com/auth/drive
```

这是 rclone 完整访问云端硬盘（不含应用数据文件夹）所用的范围，也是隐私权政策里写明的唯一范围。不要另加 Gmail、通讯录、日历或 `docs`。如果以后确实要改范围，先改 `privacy.html`，再让用户重新同意。

OAuth 客户端类型选桌面应用。重定向使用 rclone 在本机打开的地址，不要把这个网站填成重定向 URI。rclone 配置里的 scope 选 `drive`，与本站声明保持一致。

## 品牌验证不会自动完成受限范围审核

这个网站用来满足品牌页对首页、隐私权政策和服务条款的内容要求。品牌验证通过后，同意屏幕可以显示应用名和这两份链接。

`https://www.googleapis.com/auth/drive` 是受限范围。Google 写明：应用只由本人或少数你认识的人使用时，可以不提交完整的范围审核；测试状态会限制刷新令牌的有效期。把外部应用发布为正式版并让任意 Google 账号授权，则还要单独做数据访问审核。

Google 还写明，应用若通过第三方服务器访问受限范围的数据，需要年度安全评估。按这些页面的说明，令牌和文件只在用户自己的电脑与 Google 之间传输。是否因此免于评估，以 Google 的审核结论为准。
