## 部署地址

[官网文档](https://hexo.io/docs/one-command-deployment)
[部署地址](https://ayuhuajin.github.io/myblog/) https://ayuhuajin.github.io/myblog/

## hexo 命令

1.hexo clean 清空 public 文件夹
2.hexo generate 生成 public 部署文件夹  
3.hexo deploy 部署 public 部署文件夹 到 github 需要安装 hexo-deployer-git
4.hexo server 本地部署
5.hexo new "article01" 创建文章
6.hexo new page about 创建新页面

## 插件

1.部署 git 插件 hexo-deployer-git
2.hexo-admin 简易后台  
3.下载插件 hexo-asset-image 引用图片
4.hexo-generator-searchdb 搜索插件
5.hexo-generator-json-content

## Docs:

```
deploy:
type: "git"
repository: https://github.com/ayuhuajin/yxwj-website.git #你的仓库地址
branch: gh-pages #绑定的仓库分支

url: https://ayuhuajin.github.io/yxwj-website/
```

### 插件使用

1.本地引用图片

> (1).打开 hexo 目录下的\_config.yml
> post_asset_folder: true

> (2)修改代码  
> /node_modules/hexo-asset-image/index.js

```js
<script>
'use strict';
var cheerio = require('cheerio');

// http://stackoverflow.com/questions/14480345/how-to-get-the-nth-occurrence-in-a-string
function getPosition(str, m, i) {
  return str.split(m, i).join(m).length;
}

var version = String(hexo.version).split('.');
hexo.extend.filter.register('after_post_render', function(data){
  var config = hexo.config;
  if(config.post_asset_folder){
    	var link = data.permalink;
	if(version.length > 0 && Number(version[0]) == 3)
	   var beginPos = getPosition(link, '/', 1) + 1;
	else
	   var beginPos = getPosition(link, '/', 3) + 1;
	// In hexo 3.1.1, the permalink of "about" page is like ".../about/index.html".
	var endPos = link.lastIndexOf('/') + 1;
    link = link.substring(beginPos, endPos);

    var toprocess = ['excerpt', 'more', 'content'];
    for(var i = 0; i < toprocess.length; i++){
      var key = toprocess[i];

      var $ = cheerio.load(data[key], {
        ignoreWhitespace: false,
        xmlMode: false,
        lowerCaseTags: false,
        decodeEntities: false
      });

      $('img').each(function(){
		if ($(this).attr('src')){
			// For windows style path, we replace '\' to '/'.
			var src = $(this).attr('src').replace('\\', '/');
			if(!/http[s]*.*|\/\/.*/.test(src) &&
			   !/^\s*\//.test(src)) {
			  // For "about" page, the first part of "src" can't be removed.
			  // In addition, to support multi-level local directory.
			  var linkArray = link.split('/').filter(function(elem){
				return elem != '';
			  });
			  var srcArray = src.split('/').filter(function(elem){
				return elem != '' && elem != '.';
			  });
			  if(srcArray.length > 1)
				srcArray.shift();
			  src = srcArray.join('/');
			  $(this).attr('src', config.root + link + src);
			  console.info&&console.info("update link as:-->"+config.root + link + src);
			}
		}else{
			console.info&&console.info("no src attr, skipped...");
			console.info&&console.info($(this));
		}
      });
      data[key] = $.html();
    }
  }
});
</script>

```

2.搜索插件

> (1)安装 hexo-generator-searchdb，在站点的根目录下执行以下命令：
> $ npm install hexo-generator-searchdb --save

> (2)编辑 站点配置文件，新增以下内容到任意位置：

search:
path: search.xml
field: post
format: html
limit: 10000

> (3).编辑 主题配置文件，启用本地搜索功能：

```
# Local search

local_search:
enable: true
```

3.hexo-generator-json-content

> (1).npm install hexo-generator-json-content
> (2).站点 config.yml 添加配置

```
jsonContent:
  meta: false
  pages: false
  posts:
    title: true
    date: true
    path: true
    text: false
    raw: false
    content: false
    slug: false
    updated: false
    comments: false
    link: false
    permalink: false
    excerpt: false
    categories: false
    tags: true
```

## 菜单栏

```
#菜单
menu:
  主页: /
  Archives: /archives
  Tags: /tags
  Categories: /categories
  #随笔: /tags/随笔/
  产品: /product
  联系我们: /contact
  关于我们: /about
  测试: /test
  自定义: /custom
```

![这是图片](/assets/img/philly-magic-garden.jpg "Magic Gardens")
