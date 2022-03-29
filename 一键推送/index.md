# NO.4-Hugo一键推送


Pre：其实就是把之前一句一句运行的命令写成在一个记事本文件中，重命名这个记事本就可以了:rofl:

<!--more-->

<br/>

{{< style "text-align:center; color:#36a5d1" >}}

[文章不长，来点音乐，看得更轻松]^(测试一下新功能哈哈)

{{< /style >}}

{{< music auto="https://music.163.com/#/playlist?id=2138099637" autoplay=true >}}

<br/>

## 原来的博客推送方式

原来我是一句一句的复制粘贴到命令窗口

~~~
hugo --theme=LoveIt --baseUrl="https://bao-wei.github.io/" --buildDrafts
cd public
git init
git add .
git commit -m "备注"
git remote add origin https://github.com/bao-wei/bao-wei.github.io.git
git push
~~~

<br/>

## 现在的推送方式

把以上命令手动粘贴，运行。感觉快阻止我写博客的动力了，所以在网上找了一下一键推送的方法，最后感觉下面这个[实用]^(不高大上)的方法最简单方便。

- 在博客文件夹下创建一个记事本文件
- 把上面的命令全部粘贴到记事本中
- 保存关闭
- 把这个记事本文件重命名为`push.sh`。<strong>注意：</strong>txt的后缀也要删除，后缀即为`.sh`，前面的`push`随便改为自己想命名的文件名字。这个文件必须放在博客目录下，<u>与content为同一层级</u>。

这样就得到一个git的执行文件，每次写完博客后，在博客目录下双击运行一下即可。

<br/>

## 最后

嗯，越来越有QQ空间的内味儿了​h​ah​ah​a:sunglasses:
