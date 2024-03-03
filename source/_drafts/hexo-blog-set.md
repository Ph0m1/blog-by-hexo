---
title: 如何使用Hexo部署个人博客
---

    本文将详细介绍如何使用Hexo进行博客部署

## 准备工作

1. 工具:

    - npm(Node.js)
    - netlify
    - github

2. npm的安装
   - Archlinux用户：

   ```sh
   sudo yay -S npm
   ```
   - ubuntu用户
   
   ```sh
   sudo apt-install 
   ```

3. 

   选择一个空的文件夹（存储博客文件）这里用 `~/blog` 来作为博客存储路径

   ```sh
   sudo npm install hexo-cli #安装hexo组件
   cd ~/blog
   hexo init #初始化hexo程序
   ```
    ！！*注意* ！！ `hexo init` 必须在一个空的文件夹内

4. Github 仓库的建立

    
  