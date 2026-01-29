---
tags:
TODO:
public: true
star: true
AIGC:
link:
---
# 无名杀前后端Serverless解决方案

## 1. 准备工作

- [ ] GitHub账号
- [ ] Cloudflare账号

## 2. 前端（游戏Web客户端）


1. fork [Quaternijkon/noname-client](https://github.com/Quaternijkon/noname-client)
2. 登陆 [Cloudflare](https://dash.cloudflare.com/) 创建**页面**
   ![image.png](https://file.quaternijkon.online/2026/01/053fb977303d983fadd1f154f19d5952.png)
3. 导入现有 Git 存储库（第一次配置需要设置权限，按照页面指引）
   ![image.png](https://file.quaternijkon.online/2026/01/3bdc94a2e2ee4b8238c766fd21a51934.png)
4. 全部留空，等待部署完成
   ![image.png](https://file.quaternijkon.online/2026/01/29e2ca56ab488dd45dc2948424dcce38.png)

5. 现在可以使用这个域名访问你部署的客户端了
   ![image.png](https://file.quaternijkon.online/2026/01/e2549ff0990a090286cf2d63f86de0fe.png)

## 3. 后端（用于联机）

1. fork [Quaternijkon/noname-server](https://github.com/Quaternijkon/noname-server)
2. 登陆 [Cloudflare](https://dash.cloudflare.com/) 创建Worker
   ![image.png](https://file.quaternijkon.online/2026/01/52a1d9ec0cec2a32d8bd16d9aed4bbba.png)
3. 选择"Continue with GitHub"
   ![image.png](https://file.quaternijkon.online/2026/01/2cc1c9c9aa7a0eabefb6ccd32a5eaa67.png)
4. 选中刚刚fork的仓库
   ![image.png](https://file.quaternijkon.online/2026/01/4e543bdaf07752ef08c6baebedc5bf01.png)
5. 保持默认，等待部署完成。
6. 现在你可以在联机大厅使用这个链接了
   ![image.png](https://file.quaternijkon.online/2026/01/563afd89d7ade952a70771514210830e.png)

# 进阶配置
## 1. 使用同域名组建前后端（避免跨域）
![image.png](https://file.quaternijkon.online/2026/01/710ea11a77b7c8e4966775dac21faf50.png)

在server项目中添加路由，如果你的前端链接为url，那么这里填url/ws*（ws可以换成任何字符串，加不加*无所谓，但千万不能是url/*，这样work会接管所有请求，就连不上网页了）
![image.png](https://file.quaternijkon.online/2026/01/3e6734c5a478838ea711563a8b2d54d7.png)

相当于告诉 Cloudflare：“如果有人访问 `url/ws`，请不要去 Pages 找文件，直接交给 Worker 处理。这样前后端都使用相同的域名。

最后你需要更新你的github仓库中的`./wrangler.toml`（zone_name 可以不需要配置，cloudflare能猜测到）
![image.png](https://file.quaternijkon.online/2026/01/97b1bbf1d3b4a06b325a1da0153f9beb.png)

## 2. 设置联机大厅的默认域名

进入你的client仓库，修改`./index.html`
![image.png](https://file.quaternijkon.online/2026/01/1c3f99ae97ce584bd1cb5a41638f4eca.png)
如果你按照前面过程设置了前后端同源，这一步可以不用更改。
不然注释图中的110，111行代码，将112行的URL改为你worker中的链接

保险起见，`./noname/library/index.js`的链接也最好一起改掉。
![image.png](https://file.quaternijkon.online/2026/01/57d48f3f0d3f652b5805824dfed45e54.png)





