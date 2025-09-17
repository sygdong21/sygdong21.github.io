+++
title = "搭建HugoDemo"
date = "2025-01-01"
categories = [
    "Blog"
]

image = "hugodemo-02.jpg"
+++



# Hugo框架搭建blgo页面

### 前置准备

- 参考资料
  

[博客](https://letere-gzj.github.io/hugo-stack/p/hugo/custom-blog/) [视频](https://www.bilibili.com/video/BV1KSZBYTE3S/?spm_id_from=333.337.search-card.all.click&vd_source=cf5d55b6a3ced74e1cea6e6c662f92ee)

- hugo环境
  

[hugo下载地址](https://github.com/gohugoio/hugo)

[hugo主题下载地址](https://themes.gohugo.io/)

---

### 本地部署

- 下载解压hugo，cmd命令创建dev文件夹
  

将hugo.exe复制到dev文件夹

> hugo new site dev

- 添加hugo主题
  
  - 下载主题 hugo-theme-stack-master
    
  - 重命名为hugo-theme-stack放进dev-themes文件夹
    
  - 复制主题中的示例网站exampleSite文件夹中的content文件夹和hugo.yaml文件放到dev最外层
    
  
  > 路径 dev\themes\hugo-theme-stack\exampleSite
  
  - 删除与hugo.yaml冲突的hugo.toml
    
  - 修改hugo.yaml配置
    
  
  > theme: hugo-theme-stack
  

- 本地部署运行
  

> cmd启动服务 hugo server -D
> 
> 访问地址 http://localhost:1313/

---

### GithubPage 部署

- 创建github.io项目，并创建blgocode分支
  
  - blgocode分支用于放源码
    
  - master分支放自动部署生成的文件
    
- 克隆项目到本地，切换到blgocode分支
  
- 为blgocode分支添加源码
  
  - 将dev文件夹的源码添加到blgocode分支
    
  - 配置hugo.yaml的baserurl改为你的github.io地址
    
  
  > baseurl: https://sygdong21.github.io/
  
  - 本地的blgocode提交到远程
    
- GithubAction自动部署
  
  - 在github网站开发者设置中创建token用于自动化部署
    
  - 在github.io库中设置Repository secrets变量，将token赋值给它
    
  - 在blogcode分支创建.github\workflows\hugo_deploy.yaml 配置文件实现自动化部署
    
  
  ```
  
  # 代码提交到远程blogcode分支时触发github action
  # gitbub action自动部署发布到master分支   
  # 所以源码在blogcode分支 public代码在master分支
  
  name: deploy
  
  on:
    push:
      branches:
        - blogcode
      paths:
        - "content/**"
        - "hugo.yaml"
        - "themes/**"
        - ".github/workflows/hugo_deploy.yaml"   
  
  
  jobs:
    deploy:
      runs-on: ubuntu-latest
      steps:
          - name: Checkout
            uses: actions/checkout@v4
            with:
                fetch-depth: 0
  
          - name: Setup Hugo
            uses: peaceiris/actions-hugo@v3
            with:
                hugo-version: "latest"
                extended: true
  
          - name: Build Web
            run: hugo -D
  
          - name: Deploy Web
            uses: peaceiris/actions-gh-pages@v4
            with:
                PERSONAL_TOKEN: ${{ secrets.TOKEN }}
                EXTERNAL_REPOSITORY: sygdong21/sygdong21.github.io
                PUBLISH_BRANCH: master
                PUBLISH_DIR: ./public
                commit_message: auto deploy
  
  ```
  
  - 提交到远程分支触发GithubAction自动部署
    
  - 访问地址
    
  
  > https://yourGithubUsername.github.io/