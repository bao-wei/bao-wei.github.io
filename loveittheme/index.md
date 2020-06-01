# NO.3-强大的Hugo主题LoveIt


Pre：这个主题的官网也有介绍，说其原型是基于LeaveIt和KeepIt两个主题的，但这个主题集成了很多便于博客定制化的功能，十分方便！

<!--more-->

<br/>

## 主题网址

**Github:** https://github.com/dillonzq/LoveIt

**主题文档:** https://hugoloveit.com/zh-cn/categories/documentation/

---

<br/>

## Hugo版本确认

之前我在本地运行预览的时候一直报错，然后在`github` 提交issue有热心网友解答才发现是`hugo`版本问题

官网下载地址：https://github.com/gohugoio/hugo/releases

### 常规版本

在对应操作系统下直接下载64或32位即可，如：

{{< image src="https://i.loli.net/2020/05/03/wX7mfYrDPuGoCae.png" alt="图片1" caption="windows对应常规版本" title="下载Hugo" >}}

{{< image src="https://i.loli.net/2020/05/03/k3maK91fiEFelbn.png" alt="图片2" caption="macOS对应常规版本" title="下载Hugo" >}}

### 扩展版-本文主题使用的Hugo版本

多数主题的很多功能都需要下载扩展版的Hugo，所以最好安装扩展版，扩展版安装包有`extended`字样如：

{{< image src="https://i.loli.net/2020/05/03/cufsnqjVNZhJRtS.png" alt="图片3" caption="不同操作系统下的扩展版Hugo" title="下载Hugo" >}}

按照[第一篇博客](https://baowei.life/第一篇博客/#第二步-安装hugo)中的方法安装即可。

---

<br/>

## 主题下载

> 由于我自己就是code小白，所以我的一切博文都是以一个初学者的视角来介绍，这样即使不懂，也能按照这个步骤把博客搭建起来。

---

例如：我在`D盘目录`下空白处鼠标**右键**启动`git bash here`，创建了一个`blog`站点文件夹。如果不知道如何启动git，可以看之前的[第一篇博客](https://baowei.life/第一篇博客/#第三步-开始搭建博客)。

然后在**git命令窗口**中接着输入：`cd themes`

把LoveIt主题clone到**themes文件夹**下，接着输入命令：

~~~
git clone https://github.com/dillonzq/LoveIt.git
~~~

这时即可在`blog/themes/LoveIt`路径下看到该主题的文件。

---

<br/>

## 主题配置

这个地方一定要参考官网给的[教程](https://hugoloveit.com/zh-cn/theme-documentation-basics/#3-%E9%85%8D%E7%BD%AE)，不然很多东西[自己]^(主要是我这个小白)根本弄不懂，但我主要梳理几个步骤思路，便于快速让博客成形：

<br/>

- 在blog目录下有一个[config.toml文件]^(就是你的博客页面配置文件)，用VSCode打开之后其实什么东西都没有，只有几行代码，我们需要在**LoveIt主题文件夹**里面找到**examplesite文件夹**，如路径：

  ~~~
  D:\Hugo\loveitblog\themes\LoveIt\exampleSite
  ~~~

  里面有一个相同的`config.toml`文件，将这个文件复制到blog文件夹下替换掉原来的`config.toml`文件；

  <br/>

- 用VSCode打开上一步替换后的`config.toml`文件。注释掉**主题路径**两行代码，并将**是否使用git信息**设置为`false`，因为还没有配置git仓库，如图：

  {{< image src="https://i.loli.net/2020/05/03/MWaNGpXuAn2lSrE.png" alt="图片4" caption="注释路径代码" title="主题配置" >}}

  {{< image src="https://i.loli.net/2020/05/03/P91stpjB8KHGNrQ.png" alt="图片5" caption="不使用git信息" title="主题配置" >}}

- 然后在**blog文件夹**中空白处鼠标右键选择`git bash here`，输入：

  ~~~
  hugo serve -e production
  ~~~

  **在生产环境下本地运行网站**，如果用`hugo server`运行的话会因为博客主题有评论系统等功能，产生报错。

  ---

  <br/>

至此，就可以看到一个最原始的LoveIt主题博客:(far fa-hand-peace):  :tada: :tada::tada: 

---

<br/>

> 由于目前只想到这么多关键的地方，等后面想起来再整理……:sleeping::sleeping::sleeping:






