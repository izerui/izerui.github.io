hexo 目录如下：

    .
    ├── _config.yml 
    |
    ├── package.json 
    |
    ├── scaffolds 
    |
    ├── scripts 
    |
    ├── source 
    |
    |   ├── _drafts 
    |   |
    |   └── _posts 
    |
    └── themes 

参考：
https://hexo.io/zh-cn/docs/

新建文章：（会在source下生成相关md文件）

    hexo new [title]
    
本地查看：

    hexo server
    
发布部署：（两个命令的作用是相同的）

    hexo generate --deploy
    hexo deploy --generate
    