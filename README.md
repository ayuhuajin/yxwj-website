#部署地址 #https://ayuhuajin.github.io/myblog/

#部署 git 插件
hexo-deployer-git

hexo clean 清空 public 文件夹

hexo generate 生成 public 部署文件夹
hexo deploy 部署 public 部署文件夹 到 github
hexo server 本地部署

# Deployment

## Docs: https://hexo.io/docs/one-command-deployment

deploy:
type: "git"
repository: https://github.com/ayuhuajin/yxwj-website.git #你的仓库地址
branch: gh-pages #绑定的仓库分支

url: https://ayuhuajin.github.io/yxwj-website/

hexo-admin 简易后台
