# NO.2-Hugo常用代码整理


pre: 把常用代码整理出来，方便后面回看  :(far fa-keyboard):

<!--more-->

---



# Hugo常用代码整理



> 这些代码主要是在博客推送时需要用到。整理出来便于查找，在没有找到更好的推送方式前，基本每次都会用到。

***



## 基本代码

- 创建新的站点

  ~~~
  hugo new site
  ~~~

- 创建新的博客

  ~~~
  hugo new posts/文章的名字.md
  ~~~



***



## 更新博客时用的代码

> 以下代码在写好博客的md文件之后，基本都是全部复制粘贴执行一遍就可以了。在博客目录下右键执行`git bash here`



- 在博客目录下执行：

~~~
hugo --theme=hello-friend-ng --baseUrl="https://bao-wei.github.io/" --buildDrafts
~~~

- 接着转到创建的发布文件夹下：

~~~
cd public
~~~

- 接着复制粘贴：

~~~
git init

git add .

git commit -m "修改的备注"
~~~

- 下一步代码如果在同一天多次执行，可能会出现错误提示`fatal: remote origin already exists.`，表明远程连接已经连接，不管它，继续执行下一步代码即可：

~~~
git remote add origin https://github.com/bao-wei/bao-wei.github.io.git
~~~

- 继续下一步：

~~~
git push -u origin master
~~~

​	   如果在同一时间段内有执行过上一句代码，这一步就可以替换成下面的代码直接push：

~~~
git push
~~~



**发现在最后一句代码有时会要等一段时间才会有反应，因为一段时间没有push的话，登录信息会失效，需要在这一步重新登录，所以要等弹窗出来输入用户名和密码，以防这一步出什么麻烦，最好还是在电脑上安装 `Github` 客户端。**

***



# 最后

哎~ 这个毕业论文不知是喜还是忧~~~
