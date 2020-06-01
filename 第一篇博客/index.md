# NO.1-建站博客


pre: 终于把博客建起来了！LoveIt主题是真的好看！！:(far fa-grin-tears):

<!--more-->

## 一点想法

***

> 很久之前就想创建一个自己的网站，之前用的Wix这个网站模板，拖拽式的设计方式，虽然傻瓜，但是限制多，而且由于是国外服务器，加在速度很慢。其次，无论是桌面端还是移动端都需要自己适配，很麻烦。偶然看到Github宣布，其针对个人的仓库全部终身免费，加之在查资料时看到一位博主用Hugo搭建的网站很是美观，了解之后才知道用Hugo + Github papers 就可以搭建。程序小白花了一天时间才弄出来一个很初级的网站，算是在网络上有了自己的一席之地。 

****

## 接下来就是把一些搭建此博客的方法进行记录

### 1. 准备

- **[hugo](https://gohugo.io/)** 和 **[git](https://git-scm.com/)** 必备，需要在官网下载。同时需要能打开配置文件的**[VSCode](https://code.visualstudio.com/download)**编写代码的软件。

- 注册一个Github账户，存放博客文件。

- 注册之后，在头像位置选择`New repository`，创建一个新的仓库。

  {{< image src="https://i.loli.net/2020/04/20/ZLTmOutbV1vqdFo.png" alt="图片1" caption="选择`repositories`" title="创建新仓库" >}}

  {{< image src="https://i.loli.net/2020/04/20/Jx3oiqaTIuNdAbU.png" alt="图片2" caption="点`New`创建" title="创建新仓库" >}}

- 创建新仓库的名字时，**必须用`用户名.github.io`的组合形式**。

  {{< image src="https://i.loli.net/2020/04/20/CeA9v8YhEgjuSmq.png" alt="图片3" caption="输入仓库名" title="创建新仓库" >}}
  
  

***



### 2.开始搭建

#### 第一步 安装Git

在上面官网下载安装包下载，正常一直“下一步”就可以。

***



#### 第二步 安装Hugo

* 从官网下载最新对应操作系统的release压缩包，解压到自己想创建博客的文件夹位置（不一定必须放C盘）。创建一个文件夹**Hugo** ，然后在这个文件夹中创建一个文件夹命名为**bin**，把下载的release压缩包里面`hugo.exe`文件复制粘贴到bin文件夹中（**其他文件**也放进来，以防出错），**千万不要在bin文件夹里面创建文件夹**。

* 把`hugo.exe`的路径添加到系统环境变量中。

  复制`hugo.exe`所在文件夹的路径，如：`D:\Hugo\bin`

  右键点击`此电脑`→`属性`→在左边栏找到`高级系统设置`→点击`环境变量`→有两栏变量，选择**第二栏系统变量框**下的`Path`，选择之后点下方的**编辑**→点击新建→把刚才复制的`Hugo.exe`的路径粘贴进去即可。
  
  {{< image src="https://i.loli.net/2020/04/20/luADePGtMro6mwp.png" alt="图片4" caption="选择`高级系统设置`" title="添加环境变量" heigth="60%" width="60%" >}}
  
  {{< image src="https://i.loli.net/2020/04/20/iKgM1bpR8ucWP9A.png" alt="图片5" caption="选择`环境变量`" title="添加环境变量" heigth="60%" width="60%" >}}
  
  {{< image src="https://i.loli.net/2020/04/20/V4m73AthvSFHfcb.png" alt="图片6" caption="系统变量下`添加路径`" title="添加环境变量" heigth="60%" width="60%" >}}
  
  {{< image src="https://i.loli.net/2020/04/20/nsDL34JhKSr1coj.png" alt="图片7" caption="新建并`添加路径`" title="添加环境变量" heigth="60%" width="60%" >}}

- 最后，验证Hugo是否安装成功：`管理员方式打开命令提示符窗口`，输入`hugo help`按下Enter键。看到如下内容，代表安装成功。

~~~ 
A Fast and Flexible Static Site Generator built with love by spf13 and friends in Go. Complete documentation is available at http://gohugo.io
~~~



___



#### 第三步 开始搭建博客

- 创建一个存放博客文件的文件夹，我的文件夹和Hugo的bin文件夹放在一起，都在`D:\Hugo`路径下。

- 在第一步安装好`Git`后，在右键菜单里面可以看到`Git bash here`这个选项

- **在想要创建博客文件夹的文件窗口内空白处右键**，在右键菜单中选择`Git bash here`，如图：

  {{< image src="https://i.loli.net/2020/04/20/JMX7LAheBwvQbTo.png" alt="图片8" caption="新建博客文件" title="搭建博客" heigth="60%" width="60%" >}}

- **此时在Hugo这层目录下**，在命令窗口输入：`hugo new site myblog`，**myblog**是我想创建站点的文件夹名字，可以更改为自己的。

- 此时会生成一个名为**myblog**的文件夹。里面有一些基础文件，此时注意**config.toml**这个文件，后面需要更改。

- 然后需要下载**站点主题**，这个在Hugo官网的Themes分类下有很多。这个地方是一个坑，因为有的主题需要费很大的劲修改配置文件才能使用，因为自己是小白，实在折腾不起，所以换了很多主题后，选择了一个比较简单的主题：`hello-friend-ng`，[官网连接](https://themes.gohugo.io/hugo-theme-hello-friend-ng/)。

- 找到这个主题后，在起主页有安装代码，同样在刚刚那个命令窗口，输入安装代码：

  ~~~ 
  git clone https://github.com/rhazdon/hugo-theme-hello-friend-ng.git themes/hello-friend-ng
  ~~~

- 之后在`D:\Hugo\myblog\themes`路径下会看到主题的文件夹。里面有一个`exampleSite`文件夹，这个文件夹里面有一个`config.toml`文件和`recouse` `content`两个文件夹，是这个模板主题的配置文件，把这三个文件替换`myblog`文件夹里面的同名文件。

- 接着，需要创建一篇博客，在后面预览网站的时候方便看效果，在之前的命令窗口中输入命令：

  ~~~
  hugo new posts/文章名字.md
  ~~~

  创建的空博客会在`D:\Hugo\myblog\content\posts`路径下找到对应的markdown文件。

  可以用VSCode打开，编写博客内容。**注意里面的`draft:true`要删除**，不然在预览网站的时候会因为草稿状态无法看见。

  {{< image src="https://i.loli.net/2020/04/20/kwpNAXV7qxQTUj5.png" alt="图片9" caption="创建第一篇博客" title="搭建博客" heigth="60%" width="60%" >}}

  {{< image src="https://i.loli.net/2020/04/20/sZifPwTRC2MkBSp.png" alt="图片10" caption="博客内容结构" title="搭建博客" heigth="60%" width="60%" >}}

- 然后，你可以用VSCode对`myblog文件夹`下的`config.toml`文件进行一些网站修改，可以修改哪些地方，主题作者会在主页写出来的。也可以预览后进行修改。

***



#### 第四步 预览博客

依旧在`myblog`这一层下，右键调用`git bash here`，输入：

~~~ 
hugo server
~~~

然后就可以在浏览器中输入：`http://localhost:1313`进行预览。如果不满意布局之类的，可以修改`myblog`文件夹下的`config.toml`文件。然后就准备进行发布。

***



#### 第五步 发布博客

同样在`myblog`文件夹下右键调用`git bash here`命令窗口。输入：

~~~
hugo --theme=hello-friend-ng --baseUrl="https://bao-wei.github.io/" --buildDrafts
~~~

然后在命令窗口输入：`cd public` **进入生成的public文件夹目录下**。

接着分别输入以下代码：启动`git 连接`：

~~~
git init
git add .                   # add 后面有一个空格和英文输入法下的“.”
git commit -m "备注"         # 备注信息随便输入
~~~

下一步代码进行`github`账号登录：

~~~
git remote add origin https://github.com/bao-wei/bao-wei.github.io.git 
~~~

此时可能会要先要求**输入用户名**，然后要求**输入密码**。

然后把博客推送到`github`仓库：

~~~
git push -u origin master
~~~

至此，博客就可以用 `用户名.github.io` 网址在浏览器进行访问。



***



#### 博客更新



> 这部分也是我目前头疼的地方，因为似乎每次推送博客到仓库，好像都需要重复第五步的所有步骤，但我觉得应该有更好的办法，如果有哪位大神愿意赐教，由于还没有弄评论功能，所以非常希望你通过首页邮箱联系我！

目前我博客的更新步骤如下：

在`myblog文件夹`！！！这很重要，每次重新推送都要在`myblog文件夹`下右键调用`git bash here` ，然后 `hugo new posts/博客名字.md`，或者我就直接按照`hugo`博客的格式弄好开头，然后写`markdown文件`，再复制进`myblog/content/posts`文件夹里面。之后再用步骤四的方法进行推送。虽然觉得不太方便，但目前也只能慢慢摸索。



***



#### 最后



很多内容都是在网上向别人学习的，我只是把自己搭建过程记录一下，最麻烦的还是不容弄模板文件那个地方，还有修改配置文件等等，不容易使用到自己喜欢的模板，最好还是要有一定的javascript基础。总之，本文如果能有所帮助最好。第一篇实实在在博客，算是纪念一下。

在YouTube上有一位youtuber上传了一个讲解视频也不错，比较简洁，值得参考：[手把手教你从0开始搭建自己的个人博客 |第二种姿势 | hugo](https://www.youtube.com/watch?v=J7XQ2FeTHLg&t=1177s)










