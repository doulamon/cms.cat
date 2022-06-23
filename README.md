<p align = "center">
<img alt="CatCms" src="https://user-images.githubusercontent.com/95879856/147455181-fc28af03-6ae2-440e-b28f-d59717e64677.png">
</p>

## 💡 简介

[CatCms](https://github.com/doulamon/cms.cat) 是一款轻量级支持多用户，数据读写分离的内容管理系统。支持媒体文件管理，后台功能丰富，使用灵活，主题开发简单。

## 🗃 案例

* [cms.cat](https://cms.cat)

## ✨ 功能

* 多用户博客平台
* Markdown支持多种编辑模式
* 聚合分类 / 标签
* 自定义导航
* 多主题 / 多语言
* Atom / RSS / Sitemap
* 文章搜索
* 可配置动静分离

## 🎨 界面

### 管理后台

![dashboard](https://user-images.githubusercontent.com/95879856/175338449-11f30d95-c96b-457f-b644-4705eba103f2.png)

### 编辑主题

![code](https://user-images.githubusercontent.com/95879856/175338504-52eebbcf-59f9-4b92-988f-926e52989e2c.png)

### 主题选择

![theme](https://user-images.githubusercontent.com/95879856/175338557-9610cb85-96f9-451e-809b-9f3c60307119.png)

### 媒体管理

![media](https://user-images.githubusercontent.com/95879856/175338645-5485d453-d853-4dd8-9221-12401a4f62a0.png)

## 🛠️ 安装

CatCms 支持Linux/Windows 界面化安装

### NGINX 代理

```nginx
map $http_upgrade $connection_upgrade {
        default upgrade;
        '' close;
}

server {
     listen 80;
     server_name domain.com *.domain.com;
     return 301 https://$host$request_uri;
}

server {
     listen 80;
     listen 443 ssl;
     ssl_certificate conf.d/ssl/1_domain.com_bundle.crt;
     ssl_certificate_key conf.d/ssl/2_domain.com.key;
     ssl_session_cache shared:SSL:1m;
     ssl_session_timeout  10m;
     ssl_ciphers HIGH:!aNULL:!MD5;
     ssl_prefer_server_ciphers on;
 
     location / {
     
	if ($http_user_agent !~* "baiduspider|360spider|Sogou web spider|
		Sosospider|YisouSpider|Bingbot|Googlebot") {
		#return 403;
	}
	
	if ($http_user_agent ~* (Scrapy|Curl|HttpClient)) {
		return 403;
	}

	if ($http_user_agent ~* "WinHttp|WebZIP|FetchURL|node-superagent|java/|FeedDemon|Jullo|
		JikeSpider|Indy Library|Alexa Toolbar|AskTbFXTV|AhrefsBot|CrawlDaddy|Java|Feedly|
		Apache-HttpAsyncClient|UniversalFeedParser|ApacheBench|Microsoft URL Control|
		Swiftbot|ZmEu|oBot|jaunty|Python-urllib|lightDeckReports Bot|YYSpider|DigExt|
		HttpClient|MJ12bot|heritrix|EasouSpider|Ezooms|BOT/0.1|YandexBot|FlightDeckReports|
		Linguee Bot|^$" ) {
		return 403;
	}
	
	# 配置服务默认端口或定义的端口
	proxy_pass http://localhost:2021;
	client_max_body_size 200m;
	proxy_set_header Host $host;
	proxy_set_header X-Real-IP $remote_addr;
	proxy_set_header X-Appengine-Remote-Addr $remote_addr;
	add_header X-Frame-Options DENY always;
        add_header X-Content-Type-Options nosniff always;

	proxy_http_version 1.1;
	proxy_set_header Upgrade $http_upgrade;
	proxy_set_header Connection  $connection_upgrade;
	
     }
     
     location /console {# 配置自定义的后台访问路径
	   alias  /app; # 后台静态文件路径
	   try_files $uri $uri/ index.html;
    }
}
```

## 🏘️ 社区

[QQ群](https://qm.qq.com/cgi-bin/qm/qr?k=JxlOD5x4fr2nwXNLQ973FSmMemzmR35F&jump_from=webapi)

![QQgroup](https://user-images.githubusercontent.com/95879856/147330280-d57627e7-fb3d-4b42-bf44-b20ae771c936.JPG)

## 📄 授权

CatCms 免费使用

## 🙏 鸣谢
* [Nightingale](https://github.com/didi/nightingale)： 夜莺是新一代国产智能监控系统
* [Pipe](https://github.com/88250/pipe)： Pipe是一款小而美打开源博客平台
* [Vue.js](https://github.com/vuejs/vue)： 渐进式 JavaScript 框架
* [V-md-editor](https://github.com/code-farmer-i/vue-markdown-editor)： v-md-editor 是基于 Vue 开发的 markdown 编辑器组件
* [Gin](https://github.com/gin-gonic/gin)： 又快又好用的 golang HTTP Web 框架
* [GORM](https://github.com/jinzhu/gorm)： 极好的 golang ORM 库

---

## 特性说明

### 发布文章

CatCms 的文章编辑器支持 Markdown，并支持复制/粘贴图片、粘贴 HTML 自动转换 Markdown、流程图、数学公式等。

另外，可以为文章启用自动配图，会自动在文章最前面插入所选择的配图。

### 聚合分类

CatCms 使用“自底向上”的分类方式：

1. 定义分类，并配置该分类包含的标签
2. 查询某个分类文章列表时通过分类-> 标签集-> 标签关联的文章进行聚合

也就是说一篇文章在编辑时只需要打标签，访问分类时会根据该分类包含的标签将文章关联出来。这是一个自底向上的信息架构，在使用时更灵活一些，可以随时调整分类而不必重新更新文章。

### 域名绑定

在 CatCms 平台上的每个博客都可以配置独立域名值：

1. 网站主解析泛域名至服务器
2. 通过配置 NGINX 实现域名反向代理
3. 注册用户名即二级域名前缀访问

## 运维

### 数据库

CatCms 使用 Mysql 数据库引擎，同时支持数据读写分离方式运行，配置主从数据库成功后可在后台->其他栏目查看详细信息。

建议定期备份数据文件，避免意外情况导致数据丢失。

### 版本升级

在管理后台的关于中可以检查版本更新，如果提示有更新请尽快升级，一般来说升级只需要下载新的发布包然后部署重启，实际升级方式以每次版本发布公告为准。

## FAQ

### 如何做友链页面？

CatCms 拥有独立的友链管理功能。可通过后台->系统设置->友情链接进行管理。

## 结语

* 如果你在使用 CatCms
  的过程中碰到问题或者有功能需求，欢迎参与 [QQ群](https://qm.qq.com/cgi-bin/qm/qr?k=JxlOD5x4fr2nwXNLQ973FSmMemzmR35F&jump_from=webapi)
  讨论，我们会在第一时间回复 😄
* 如果你想自己开发 CatCms 主题，请参考后台->高级功能栏目

