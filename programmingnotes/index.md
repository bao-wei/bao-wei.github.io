# Psychopy online experiment programming notes




> **科技的进步从来不是靠自我封闭，这也是为什么我将笔记保存在Github Pages上公开，而不是保存在个人网站或社交平台上。你可以从仓库里面直接保存原版的md文件，也可以随时访问这个页面。**
>
> 在此记录Psychopy的编程笔记，一方面是给自己做一个备忘录，另一方面是希望通过这个方式抛砖引玉，让各位能够在解决类似问题时有一些思路，同时也可以给出更好的解决方案。如果恰好笔记有所帮助，那也给这篇笔记增加了不小的意义。
>
> 对于科研人而言，分享不图任何报酬、利益，但希望获得尊重。如果这个笔记有所帮助，你想告诉更多的人，那非常欢迎。**当你进行转载或搬运的时候，请注明出处(贴个网址就行)，最重要的：请不要进行有偿分享。**
>
> Psychopy本身是一个开源软件，它的出现是为了让我们更好、更方便的进行实验，其作者的初衷和Steven Luck一样，后者将ERPLab的功能进行界面化、简单化，让EEG的分析变得更简单，为更多科研者所用。所以，**请让科研变得更简单，不要让它变得更复杂。**

</br>

# Online and Offline Experiment

{{< admonition type=tip title="Tip 1: 达到正确率后通过练习" open=false >}}

{{< link href="https://www.jianshu.com/p/9d562d34d5ec" content=练习返回code组件代码设置 >}}

这一步只能单纯的实现，被试达到一定正确率后通过练习。按照上面链接中的设置，可以出现的效果是：达到设定的正确率后，被试可以通过练习，但无法进行再次练习，只能进行正式实验。对于练习较少的实验，被试可能会没有完全掌握实验操作方式，且无法再次进行练习。这就需要添加达到正确率后，既可以选择*通过练习*，也可以选择*返回重新练习*

{{< /admonition >}}

{{< admonition type=tip title="Tip 2: 达到正确率后选择通过/再次练习" open=false >}}

设置被试主动重复练习的按键，即达到正确率后仍然想再次练习。在[计算练习正确率code组件]^(Tip 1 中的最后一个code组件)中的`begin routine`和`end routine`部分中增加代码。

**Begin routine部分**：

```python
if number_correct/(practice_trials.nTotal + 1) >= 0.80:
    end_practice.setText("练习结束\n按[空格]键开始正式实验\n按[B]键重新练习")
```

**End routine部分**：

```python
if number_correct/(practice_trials.nTotal + 1) >= 0.80:
	if key_resp_3.keys == 'space':
    	practice_loop.finished = True
	if key_resp_3.keys == 'b':
    	practice_loop.finished = False
```

**注意**：因为在没有达到要求的正确率时，程序出现的指导语是：练习未通过，请按空格重新练习。通过练习时也是按空格进入正式实验，所以这里会存在一个问题：在**end routine**部分如果不加第一句限制正确率的条件代码，同时，如果被试没有达到要求的正确率（如这里设置的0.8，即80%），按空格键也会跳出练习的循环。`if number_correct/(practice_trials.nTotal + 1) >= 0.80`这句代码在**end routine**部分必须加上，不是多余的。

{{< /admonition >}}

{{< admonition type=tip title="Tip 3: 在练习中增加反馈" open=false >}}

**方法**：单个trial的组件一般放在同一个routine中(如图中`practice_stroop`)，反馈就单独新建一个routine放在trial routine后，即被试按键后判断后(如图中`feedback_stroop`)。在`feedback_stroop`中有两个组件，首先是`code_3`组件，用于判断正确错误，然后是text文字组件`fb_2`，用于呈现反馈的文字，文字组件中的内容部分输入`$fb_text`，这样`code_3`组件生成的反馈内容就在文字组件中呈现。如下图：

{{< image src="/imgs/tip6_routine.png" alt="Tip 3" width="600" height="300">}}

在这个`feedback_stroop`Routine中增加一个`code_3`组件(名称随意)，将组件的[语言方式设置为both]^(默认是Auto->js)，在其中的**begin routine**部分增加如下代码：

**左侧的python窗口输入**：

```python
if practice_key_resp_stroop.corr == 1:
    fb_text = '✔'
    fb_col = 'green'
if practice_key_resp_stroop.corr == 0:
    if practice_key_resp_stroop.keys == None:
        fb_text = '未反应'
        fb_col = 'white'
    else:
        fb_text = '❌'
        fb_col = 'red'
```

**右侧的js窗口输入**：

```javascript
if ((practice_key_resp_stroop.corr === 1)) {
    fb_text = "\u2714";
    fb_col = "green";
}
if ((practice_key_resp_stroop.corr === 0)) {
    if ((practice_key_resp_stroop.keys === undefined)) {
        fb_text = "\u672a\u53cd\u5e94";
        fb_col = "white";
    } else {
        fb_text = "\u274c";
        fb_col = "red";
    }
}
```

两部分代码唯一的区别在于：无反应在python中的表示为`None`，在js中的表示为`undefined`，这里为了本地端和网页端都能正常运行，就将代码模式设置为`both`，这样python代码和js代码就可以不一样，这样两端都可以正常运行程序。

**更新**：`None`和`undefined`从 psychopy2022.2.3版本以后似乎不存在这个问题了，code组件的code type直接用`Auto->Js`模式即可。

{{< /admonition >}}

{{< admonition type=tip title="Tip 4: 屏幕刷新率和刺激呈现时间" open=false >}}

刺激呈现时间t=[N(帧数)]/[60Hz(刷新率)]，最短时间是28.67ms（60hz的显示器两帧之间的时间约等于16.67ms，屏幕自上而下刷新一次大概12ms（源自：《实验编程：psychopy从入门到精通》第十二章第一节）

{{< /admonition >}}

{{< admonition type=tip title="Tip 5: 从正式试次中随机选取10试次练习" open=false >}}

例如：从108试次里面随机抽取10试次进行练习，在trials循环层面的`select rows`位置填入代码：`$random(10)*108`，然后condition文件选择正式实验的excel文件即可。(不推荐这个方法，建议另外创建练习的condition表格文件，尽量把每个条件都选1到2个试次进行练习，这样更平衡。)

{{< /admonition >}}

{{< admonition type=tip title="Tip 6: 数字格式的刺激呈现问题" open=false >}}

所有数字不能是“**以数字形式储存文本**”的形式在excel表格中，不然psychopy会把这个识别为数字，在excel中设置为“常规”格式即可，然后psychopy中用`str()`转换为字符即可(括号内为刺激条件名称)

{{< /admonition >}}

{{< admonition type=tip title="Tip 7: 刺激大小/视角" open=false >}}

> Psychopy的默认单位是height，这个单位的好处是能够根据屏幕的大小自动适配刺激大小，不会因屏幕分辨率不同造成刺激大小不同。
>
> 具体参考文档：{{< link "https://www.psychopy.org/general/units.html" >}}

***以我的实验程序为例，需要刺激大小在3°以内，屏幕分辨率是$1920pix \times 1080pix$***

- 视角计算参数：屏幕长：`40cm`  宽：`30cm`，距离屏幕：`60cm`

- 用`height`为单位，默认字体大小为`0.05`，如果高度为1的话就是全屏高度，即像素的`1080pix`，那`0.05`就是`5%`。

- 所以默认的$0.05height \approx 1080pix \times 5$ % $= 54pix $，这根据[视角计算工具]^(文末有网址)得出大约为不到**1.1度**视角

- 根据视角计算器，`3°`视角大约为`151pix`.

- 因此，把`151pix`换算到`height`，$height=151pix/1080pix\approx0.14$，即`3°`视角大小的刺激。

- 乘数和被乘数上下移动**0.5度**错开位置，**答案保持中心不变**。
  移动位置为：$height=25pix/1080pix\approx0.023$。

⭐stroop正式实验刺激大小为2度(`100.5pix`，即`0.093height`)，反馈字体大小为1.5度，掩蔽刺激大小6.4度

⭐乘法口诀正式实验刺激大小为`0.093height`(即`2°`)

{{< /admonition >}}

{{< admonition type=tip title="Tip 8: 随机刺激屏的方式" open=false >}}

例如：随机300-500ms

```python
0.3+0.2*random() # 在线下可能需要使用 random.random(),具体原因见下方解释
```

{{< image src="/imgs/random_stimuli.png" alt="random_time" width="600" height="300">}}

⚠️  **<font color=#dd001b>注意</font>**：如果**线下程序**里面由于其他需求，在程序里面插入了代码`import random`，那随机时间这里的代码需要改为`0.3+0.2*random.random()`。改成这样可以运行程序。**线上程序**如果没有引入多余的[模块]^(module)那就还是维持`random()`不变，如果引入了多余的模块，可能同样需要修改为`random.random()`。是什么原因导致这个问题还没有去深究。了解原理后再更新。

{{< /admonition >}}

{{< admonition type=tip title="Tip 9: ⭐增加中途休息" open=false >}}

在单个试次的`loop`下，新增一个`routine`，在这个`routine`中添加一个code组件，在里面的`begin routine`下增加如下代码：

```python
if trials.thisTrialN==191 or trials.thisTrialN==383:
    continueRoutine = True
else:
    continueRoutine = False
```

⚠️`trials.thisTrialN`中的trials要根据loop的名称对应修改，因为我设置的loop名称为`trials`，如果loop名称为`trials_stroop`，那就要修改为`trials_stroop.thisTrialN`。

代码解释：如果试次数达到191或383，则运行该Routine，其他试次则不运行。运行该Routine时，则弹出文字“请休息一下，按空格键继续”。如下图。

{{< image src="/imgs/psychopy_rest.png" alt="rest_code_1" width="600" height="300">}}

{{< image src="/imgs/psychopy_rest2.png" alt="rest_code_2" width="600" height="300">}}

{{< image src="/imgs/psychopy_rest3.png" alt="rest_code_3" width="600" height="300">}}

{{< /admonition >}}

{{< admonition type=tip title="Tip 10: ⭐Selected rows是左开右闭" open=false >}}

[Confused about indices used in selected rows](https://discourse.psychopy.org/t/confused-about-indices-used-in-selected-rows/2615)

假如你想在`loop_1`选择**第1行到第8行**进行循环，`loop_2`选择**第9行到第16行**进行循环，`loop_3`选择**第17行到第24行**进行循环。

那在添加loop后，需要分别在Selected rows部分填入：0:8、8:17、17:25

⚠️可以看到，**python**的选择方式是`[`**左开  右闭**`)`的方式

{{< image src="/imgs/selected_rows.png" alt="selected_rows" width="600" height="300">}}

{{< /admonition >}}

---

</br>

# Online Experiment 

{{< admonition type=tip title="Tip 1: 在线运行错误可能原因" open=false >}}

在网页运行的时候要关闭阻止广告插件、油猴脚本等插件，会导致程序运行不正确

{{< /admonition >}}

{{< admonition type=tip title="Tip 2: PC端无法同步到网页端解决方法" open=false >}}

同步本地和线上程序时，如果直接同步不可用，可以搜索在线程序来同步（**<font color=#55bde2>注意，同步要同步两次，同步一次可能不会生效</font>**）。方法如下：

通过build界面`在线搜索`功能解决无法同步程序到网页端的方法：{{< link "https://psychopy.org/online/sharingExperiments.html" >}}

{{< /admonition >}}

{{< admonition type=tip title="Tip 3: Python代码和Js代码都要检查" open=false >}}

如果本地端能运行，但网页端不能运行，说明生成的psychojs代码不对。修改的每一个地方都要对照python和js的代码来修改，有的python代码不一定适用于js代码，所以要根据python的意思来修改js的代码，才能让网页端程序正常运行，如 **<font color=#55bde2>Online and Offline Experiment Tip 3</font>** 中的例子。

{{< /admonition >}}

{{< admonition type=tip title="Tip 4: 同步前要先生成js代码" open=false >}}

每次修改完build界面的程序，在同步到网页端前，**<font color=#55bde2>都要生成一次js代码，然后保存js代码</font>**，这样build的修改才会在js代码里面生效，不然build界面的修改是不会自动同步到js代码里的！！！！！

{{< /admonition >}}

{{< admonition type=tip title="Tip 5: 同步前要关闭网络代理" open=false >}}

同步时要关闭电脑上的网络代理工具，不然会导致网络连接失败

{{< /admonition >}}

{{< admonition type=tip title="Tip 6: 同步时可能出现的问题" open=false >}}

同步时，要关闭所有需要上传的文件，不然可能会因为文件被程序占用导致上传报错

{{< /admonition >}}

{{< admonition type=tip title="Tip 7: 网页端的修改没有生效" open=false >}}

当仓库显示上传成功时（根据更新时间判断），如果在网页端的控制台piloting实验时发现修改没有生效，说明浏览器缓存没有更新，要么清除浏览器缓存的cookie以及历史记录（近一小时或最近的即可），要么换个浏览器piloting

{{< /admonition >}}

{{< admonition type=tip title="Tip 8: ⭐本地端和网页端加载资源方式" open=false >}}

如果在本地增加了新的文件夹存放图片或excel条件文件，要提前在**build界面**的**全局设置**中，**online选项卡**下添加这些resources，不然加载图片或条件会报错，提示无法加载相应的材料(如下图)

{{< image src="/imgs/online_files.png" alt="online_files" width="600" height="300">}}

{{< /admonition >}}

{{< admonition type=tip title="Tip 9: ⭐打开在线实验卡在：Initialising experiment白屏" open=false >}}

在这里先打开浏览器的开发者选项查看报错是什么，Chrome和Edge浏览器是按住`Ctrl+Shift`再按`I`。如果报错是connection相关的，那就可以参考下方的解决方法，如果报错是syntax相关的，那就参考这个[解决方法](https://psychopy.org/online/psychoJSCodingDebugging.html#launch-errors-stuck-on-initialising-the-experiment)

**原因:** 导致程序卡在白屏位置是因为Psychopy的online实验网站Pavlovia在线加载程序时需要依赖nmp开源代码(具体是什么我也不清楚，理解为网页端需要的资源即可)。其中涉及到js和CSS代码，都是在线程序打开需要用的基础代码，且这些代码是开源的。而Pavlovia使用的这个代码是jsdeliver网站下的，这个网站由于网站证书到期，没有续期，所以被国内屏蔽，就导致在线程序需要的网页资源无法加载，所以就卡在白屏位置。

**解决方法：**

- 在上传实验后，在你电脑上程序文件夹里应该有一个`index.html`网页文件，这个文件在你每次修改并上传程序后都会被覆盖，且里面的内容都会被修改为默认内容。这个是需要注意的。

- 在你的在线仓库里(不知道这是什么的自己查一下)，同样也会有`index.html`这个文件。打开这个文件，修改下图中的第8、14、15、16行网址，修改内容如下：

  将`https://cdn.jsdelivr.net/npm/`改为`https://unpkg.com/`，其他部分保持不变，或者直接用后面这个网址替换上述几行网址代码也行：`https://unpkg.com/jquery-ui-dist@1.12.1/jquery-ui.min.css`

  {{< image src="/imgs/index_jsdeliver.png" alt="index_html" width="600" height="300">}}

- 这个的原理就是换一个开源代码的源，其他源可以参考这个知乎回答：[jsdelivr cdn报错无法访问](https://zhuanlan.zhihu.com/p/447713250)

{{< /admonition >}}

{{< admonition type=tip title="Tip 10: ⭐在线实验记录IP地址" open=false >}}

在线实验存在一个被试重复多次参加的情况，为了避免这种问题造成数据质量下降，可以增加获取IP地址来对数据进行检查，辅助判别是不是有重复参加实验的被试。

[参考原帖](https://discourse.psychopy.org/t/how-to-get-the-ip-address-of-the-participant/11675/4)

在程序的开头，增加一个Code组件，然后在Begin Experiment加上如下代码，代码的模式设置为`both`，代码只放右边JS部分，左边python部分为空(**参考下图**)：

```javascript
  $.getJSON('https://api.ipify.org?format=json', function(data){

    console.log(data.ip);
    localStorage.setItem('ip',data.ip);

    psychoJS.experiment.addData('IP_Addresss', data.ip)

  });
```

{{< image src="/imgs/psychopy_get_IP.png" alt="get_IP_code" width="600" height="300">}}

{{< /admonition >}}

---

</br>

# Offline Experiment

{{< admonition type=tip title="Tip 1: ⭐按键平衡代码" open=false >}}

1. 在程序指导语处加一个code组件

2. 在code组件的  **Before Experiment选项卡**  中放入代码：

   ```python
   import re # 导入re模块
   ```

3. 在code组件的  **Begin Experiment选项卡**  中放入代码：

   ```python
   sub_name = expInfo['participant']
   sub = re.findall(r"\d+\.?\d*", sub_name)
   sub_num = int(sub[0])
   if (sub_num % 2) == 0:
       keybalance = "2"
   else:
       keybalance = "1"
   ```

**第3步代码解释：**

以字符串和数字组合为例，如被试号：`S20`

```python
# 将被试信息participant作为按键平衡的信息输入，输入方式为S1、S2等。
sub_name = expInfo['participant']
```

分离字符`S`和数字`20`

```python
# 将字符 S 和 数字 20 分离，得到数字
# python提取字符串中的数字，参考：https://blog.csdn.net/qq_38486203/article/details/80309478
sub = re.findall(r"\d+\.?\d*", sub_name)	
```

然后用 `sub_num % 2 == 0` 的表达方式判断奇偶，进行按键平衡

```python
# 参考：https://discourse.psychopy.org/t/using-an-if-statement-to-create-counter-balancing-online/20653
sub_num = int(sub[0])
if (sub_num % 2) == 0:
    keybalance = "2"
else:
    keybalance = "1"
```

---

参考：

[Counterbalancing across participants](https://discourse.psychopy.org/t/counterbalancing-across-participants/11989/7)

[python 从字符串中提取数字](https://blog.csdn.net/weixin_44178960/article/details/117266731)

[python从字符串中提取数字](https://blog.csdn.net/qq_38486203/article/details/80309478)

---

*这个方法没有在线上实验程序中尝试，因为本地运行时，代码`import re`在自动转换成js代码时会报错。因为编写的程序是线下版，就没有测试线上程序。后续这个功能会加入到线上程序中，然后更新本文内容*。

{{< /admonition >}}

{{< admonition type=tip title="Tip 2: ⭐单独提取某一列进行随机" open=false >}}

[参考文章](https://discourse.psychopy.org/t/simultaneously-randomize-two-columns-in-conditions-file/22635/4)

在帖子回答中用`ctrl+F`搜索 **Independent Randomisation** 这个 Demo，下载查看程序就知道怎么回事。

{{< /admonition >}}

{{< admonition type=tip title="Tip 3: ⭐EEG实验中Psychopy发送Mark" open=false >}}

[Psychopy EEG Trigger Notes]({{< relref "psychopyeegtriggernotes" >}} "Psychopy EEG Trigger Notes")

{{< /admonition >}}

{{< admonition type=tip title="Tip 4: ⭐Shuffle某一List后，需要 .copy() 才能赋值" open=false >}}

*<font color=#dbd8da>本Tip来源：wyh_task</font>*

*本Tip需要结合本部分的**Tip2**才能明白什么回事。*

例如：加载了一个List：`list_motivation_cue_4 = []`，然后在循环中对这个List写入数据：`list_motivation_cue_4.append(motivation)`，写入的数据就是**excel条件**中`motivation`这个变量，然后打乱这个List：`random.shuffle(list_motivation_cue_4)`，传给后面的程序使用。参考下面代码：

```python
# 首先新建一个Routine，在这个Routine里面放入code组件，然后在Begin Experiment中写入下一句代码
list_motivation_cue_4 = [] #创建空的List

# 然后Begin Routine中写入下一句代码
# 通过.append()循环写入motivation这个变量，是否打乱顺序写入取决于Loop属性中loopType是不是选择的random
list_motivation_cue_4.append(motivation) 

# 在End Routine中写入下面代码
random.shuffle(list_motivation_cue_4) # 先打乱这个List
list_motivation_4 = list_motivation_cue_4.copy() #然后通过 .copy() 将打乱的List赋值给其他变量，经测试直接用 ‘=’ 进行赋值无效

# 最后给这个Routine加上一个Loop，并选择相应的条件excel。
```

{{< /admonition >}}

---

</br>



**<font color=#55bde2>参考内容</font>**

- Psychopy论坛：{{< link "https://discourse.psychopy.org/" >}}

- 代码仓库地址：{{< link "https://gitlab.pavlovia.org/" >}}

- 在线程序地址：{{< link "https://pavlovia.org/#main" >}}

- 视角计算工具网站：{{< link "https://www.sr-research.com/visual-angle-calculator/" >}}

- 网页端已经支持组件列表：{{< link "https://psychopy.org/online/status.html" >}}

- python代码和js代码转换文档：{{< link "https://discourse.psychopy.org/t/psychopy-python-to-javascript-crib-sheet/14601" >}}

- 网页端运行时产生“Unknown Resource” issue报错的原因： 

  - {{< link "https://discourse.psychopy.org/t/unknown-resource-online-but-works-fine-offline/25793/10" >}}
  - {{< link "https://psychopy.org/online/configureOnline.html" >}}
  - {{< link "https://psychopy.org/online/index.html" >}}

- ⭐参考书籍推荐：

  《实验编程：PsychoPy从入门到精通》

  {{< image src="/imgs/psychopy_book.jpg" alt="psychopy book" width="600" height="300">}}










