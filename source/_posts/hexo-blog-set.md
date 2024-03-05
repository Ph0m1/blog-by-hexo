---
title: 如何使用Hexo部署个人博客
---

   本文将详细介绍如何使用Hexo进行博客部署
   [Hexo官网](https://hexo.io/)

## 详细步骤

1. 工具:

    - npm(Node.js)
    - netlify (也可以用vercel)
    - github
    - git

2. npm的安装
   - Archlinux用户：

   ```sh
   sudo yay -S npm
   ```

   - ubuntu用户

   ```sh
   sudo apt-get install npm
   ```

   - Windows用户
     
     请访问[Node.js](https://nodejs.org/)自行下载
     
     [Git官网](https://git-scm.com/)


1. hexo的安装及其初始化

   选择一个空的文件夹（存储博客文件）这里用 `~/blog` 来作为博客存储路径

   ```sh
   sudo npm install -g hexo-cli #安装hexo组件
   cd ~/blog
   hexo init #初始化hexo程序
   npm install #自动补全组件
   ```

    ！！*注意* ！！ `hexo init` 必须在一个空的文件夹内

2. [Github](https://github.com/) 仓库的建立

   登陆Github
   新建一个仓库
   将该仓库链接到本地 `~/blog` 目录下

   ```sh
   cd ~/blog
   git init #初始化git仓库
   ```

   查看 ./.gitignore 文件是否忽略
   .DS_Store
   Thumbs.db
   db.json
   *.log
   node_modules/
   public/
   .deploy*/
   _multiconfig.yml
   **（该文件一般自行生成，无需更改）**

   将本地文件上传Github仓库

   ```sh
   git add .

   git commit -m "setup"

   git branch -M main

   git remote add origin <你的Github仓库链接>

   git push -u origin main
   ```

3. 使用 [netlify](https://app.netlify.com/) 托管博客

   <font color=Red size=3.5>***netlify必须上传身份证照片进行实名认证，否则账户会被封禁***</font>

   选择 sign in 并使用 Github 登陆 netlify

   在该页面的侧边栏中选择 **Sites** 随后选择 `Add new site`

   在下拉栏中选择 `Import an existing project`

   进入新的页面后选择 `Deploy with Github` 并授权

   随后勾选 `Only select repositories` 并选择第四步中创建的仓库 然后点击 `Save`

   回到 netlify 主页，点击准备部署的仓库 `Team` 栏可以选择任意名字，这里我选择用我的用户名

   `Site name` 填写你想要的域名如填写 phom 将会生成 phom.netlify.app 这个默认域名

   剩下的选项会自行填充好，如有需要，请自行修改

   设置完成后点击 `Deploy XXX` 等待部署好站点啊，点击访问；

   默认使用 `landscpace` 主题，并会生成名为 hello world 的博客

   后续要上传博客，只需将 markdown 文件放入`./source/_posts/` 目录下，并推送至github即可自行部署

   ***[vercel](https://vercel.com/)*** 操作基本一致，只需选择对应的github仓库就行
   

4. 进一步优化博客页面

     这里需要自行学习一些前端知识

   1. 本地部署

      ```sh
      hexo clean
      
      hexo g #hexo generate 生成静态页面
      
      hexo s #hexo server 在本地服务器部署博客

      hexo new page <目录> #新增页面

      ```

      **关于草稿** 在source目录下创建名为 `_drafts` 的目录，建议将草稿文件放在该目录下进行编辑；

                    使用 hexo s --drafts 部署即可预览   

   2. 配置

      在/blog下找到 `_config.yml`

      参考[Hexo官方文档](https://hexo.io/zh-cn/docs/configuration)修改对应参数

      <table>
       <thead>
         <tr>
          <th>参数</th>
          <th>描述</th>
         </tr>
       </thead>
       <tbody>
        <tr>
         <td><code>title</code></td>
         <td>网站标题</td>
        </tr>
        <tr>
         <td><code>subtitle</code></td>
         <td>网站副标题</td>
        </tr>
        <tr>
         <td><code>description</code></td>
         <td>网站描述</td>
        </tr>
        <tr>
         <td><code>keywords</code></td>
         <td>网站的关键词。支持多个关键词。</td>
        </tr>
        <tr>
         <td><code>author</code></td>
         <td>您的名字</td>
        </tr>
        <tr>
         <td><code>language</code></td>
         <td>网站使用的语言。对于简体中文用户来说，使用不同的主题可能需要设置成不同的值，请参考你的主题的文档自行设置，常见的有 <code>zh-Hans</code>和 <code>zh-CN</code>。</td>
        </tr>
        <tr>
         <td><code>timezone</code></td>
         <td>网站时区。Hexo 默认使用您电脑的时区。请参考 <a target="_blank" rel="noopener external nofollow noreferrer" href="https://en.wikipedia.org/wiki/List_of_tz_database_time_zones">时区列表</a> 进行设置，如 <code>America/New_York</code>, <code>Japan</code>, 和 <code>UTC</code> 。一般的，对于中国大陆地区可以使用 <code>Asia/Shanghai</code>。</td>
        </tr>
       </tbody>
      </table>
   3. 目录修改
      
      使用hexo new page <路径>来创建新的页面

      并修改 `./source/<路径>/index.md` 来完善新的页面

   4. 主题修改
      
      请在[主题文档](https://hexo.io/themes/)中选择心仪的主题

      如 [flex-block](https://github.com/miiiku/hexo-theme-flexblock) [butterfly](https://butterfly.js.org/) [next](https://theme-next.js.org/)

      这里以 next 举例

      ```sh 
      npm install hexo-theme-next #安装主题
      
      cat ./node_modules/hexo-theme-next/_congif.yml > _config.srud.yml #将配置文件拷贝至主目录

      ```
      然后打开 _config.yml 找到 theme 并将后面的值修改为 next
      
      重新部署博客即可

      其余主题步骤类似，详情请参考各主题文档进行修改配置

5. 自定义域名
   
   请在腾讯云等网站注册自己的域名，并解析到netlify生成的链接上

   并在netlify中打开刚刚的博客管理页面 选择侧边栏中的 `Domain management`

   填写您自己的域名并验证，验证成功后点击 `Add domain` 高亮按钮，即可使用自定义域名访问博客
   