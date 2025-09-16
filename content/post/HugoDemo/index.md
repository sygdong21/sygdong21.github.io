---
title: HugoDemo
date: 2025-01-01
image: helena-hertz-wWZzXlDpMog-unsplash.jpg
categories:
    - Blog
---

## Hugo框架搭建博客demo


- 参考资料
  
  [博客](https://letere-gzj.github.io/hugo-stack/p/hugo/custom-blog/) [视频](https://www.bilibili.com/video/BV1bovfeaEtQ?spm_id_from=333.788.videopod.episodes&vd_source=cf5d55b6a3ced74e1cea6e6c662f92ee)
  
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
  
  ``` 
  路径 dev\themes\hugo-theme-stack\exampleSite
  
  ```
  删除与hugo.yaml冲突的hugo.toml配置文件
  
  编辑hugo.yaml修改主题

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
  
2. 克隆项目，将dev文件夹的文件复制到项目里，修改hugo.yaml配置，添加gitignore文件，提交到github
  
  ```
  #baseurl改为你的github.io地址
  baseurl: https://sygdong21.github.io/
  ```
  

```
#  .gitignore文件
public
static
hugo.exe
.hugo_build.lock
```

3. Github Action自动化部署
  
  - 在github网站开发者设置中创建token用于自动化部署
    
  - 在github.io库中设置Repository secrets变量 将token的值写进去
    
  - 创建.github\workflows\hugo_deploy.yaml 配置文件实现自动化部署
    
    ```
    name: deploy
    
    # 代码提交到master分支时触发github action
    # secrets.TOKEN为配置的token值
    # EXTERNAL_REPOSITORY为 用户名/项目名
    on:
      push:
        branches:
          - master
    
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
    
  - 提交到GitHub.io Github Action自动部署
    
  - 访问地址
    
    ```
    https://yourGithubUsername.github.io/
    ```