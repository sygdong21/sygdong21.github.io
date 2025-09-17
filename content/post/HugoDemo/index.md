+++
title = "搭建HugoDemo"
date = "2025-01-01"
categories = [
    "Blog"
]

image = "hugodemo-01.jpg"
+++


## Hugo框架搭建blog

- 参考资料
  
  [博客](https://letere-gzj.github.io/hugo-stack/p/hugo/custom-blog/) [视频](https://www.bilibili.com/video/BV1KSZBYTE3S/?spm_id_from=333.337.search-card.all.click&vd_source=cf5d55b6a3ced74e1cea6e6c662f92ee)
  
- Hugo环境
  
  [Hugo下载地址](https://github.com/gohugoio/hugo) [Hugo主题下载](https://themes.gohugo.io/)
  

---

#### 本地部署

1. 下载解压Hugo cmd命令创建dev文件夹

```
hugo new site dev
```

将hugo.exe文件复制到dev文件夹

2. 添加Hugo主题
  
  下载主题 hugo-theme-stack-master
  
  重命名 hugo-theme-stack 放进dev-themes文件夹
  
  复制主题中示例网站exampleSite文件夹中的content文件夹和hugo.yaml文件 放到dev最外层
  
  ``` ```
  路径 dev\themes\hugo-theme-stack\exampleSite
  
  ```
  删除与hugo.yaml冲突的hugo.toml配置文件
  
  编辑hugo.yaml修改主题
  ```
  
  ```
  theme: hugo-theme-stack
  ```
  
3. 本地部署运行
  
  ```
  cmd启动服务    hugo server -D
  ```
  
  ```
  访问地址       http://localhost:1313/
  ```
  

---

#### GithubPage部署

1. 创建github.io项目
  
  创建项目blgocode分支
  
  其中blgocode分支放hugo框架源码
  
  master分支放自动部署后生成的代码
  
2. 克隆项目到本地，切换到blgocode分支
  
  将dev里的源码添加到本地
  
  修改hugo.yaml 的baseurl改为你的github.io地址
  
  ```
  baseurl: https://sygdong21.github.io/
  ```
  
  提交到远程blgocode分支
  
3. Github Action实现自动化部署
  

- 在github网站开发者设置中创建token用于自动化部署
  
- 在github.io库中设置Repository secrets变量 将token的值写进去
  
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
  
- 提交到分支触发Github Action自动部署
  
- 访问地址
  
  ```
  https://yourGithubUsername.github.io/
  ```