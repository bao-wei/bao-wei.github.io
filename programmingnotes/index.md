# Psychopy online experiment programming notes


练习返回code组间中代码



内容1：练习返回code组件中的代码

tip1：在网页运行的时候要关闭阻止广告插件、油猴脚本等插件，会导致程序运行不正确

tip2：同步本地和线上程序时，如果直接同步不可用，可以搜索在线程序来同步（注意，同步要同步两次，同步一次可能不会生效）

tip3：修改的每一个地方都要对照python和js的代码来修改，有的python代码不一定适用于js代码，所以要根据python的意思来修改js的代码，才能让网页端程序正常运行

tip4：例如在反馈代码(程序中的`fb_code`组件)中，无反应在python中的表示为`None`，在js中的表示为`undefined`，这里为了本地端和网页端都能正常运行，就将代码模式设置为`both`，这样python代码和js代码就可以不一样，这样两端都可以正常运行程序

tip5：每次修改完build界面的程序，都要生成一次js代码，然后保存js代码，这样build的修改才会在js代码里面生效，不然build界面的修改是不会自动同步到js代码里的！！！！！

tip6：同步时要关闭电脑上的网络代理工具，不然会导致网络连接失败

tip7：刺激呈现时间t=N(帧数)/60(刷新率)，最短时间是28.67ms（60hz的显示器两帧之间的时间约等于16.67ms，屏幕自上而下刷新一次大概12ms。源自：《实验编程：psychopy从入门到精通》第十二章第一节）

tip8：从108试次里面随机抽取10试次进行练习，在trials循环层面的select rows位置填入代码：`$random(10)*108`

tip9：同步时，要关闭所有需要上传的文件，不然可能会因为文件被程序占用导致上传报错

tip10：当仓库显示上传成功时（根据更新时间判断），如果piloting实验时发现修改没有生效，说明浏览器缓存没有更新，要么清除浏览器缓存的cookie以及历史记录（近一小时或最近的即可），要么换个浏览器piloting

tip11：设置被试主动重复练习的按键，即达到正确率后仍然想再次练习。在计算练习正确率的routine部分的code中增加代码。

begin routine部分：

```python
if number_correct/(practice_trials.nTotal + 1) >= 0.80:
    end_practice.setText("练习结束\n按[空格]键开始正式实验\n按[B]键重新练习")
```

end routine部分：

```python
if number_correct/(practice_trials.nTotal + 1) >= 0.80:
	if key_resp_3.keys == 'space':
    	practice_loop.finished = True
	if key_resp_3.keys == 'b':
    	practice_loop.finished = False
```

注意：因为在没有达到要求的正确率时，程序出现的指导语是：练习未通过，请按空格重新练习。通过练习时也是按空格进入正式实验，所以这里会存在一个问题：在end routine部分如果不加第一句限制正确率的条件代码，同时，如果被试没有达到要求的正确率（如这里设置的0.8，即80%），按空格键也会跳出练习的循环。

tip12：所有数字不能以“数字形式储存文本”的形式在excel中，不然psychopy会把这个识别为数字，在excel中设置为“常规”格式即可，然后psychopy中用str()转换为字符即可(括号内为刺激名称)

---

代码仓库地址：https://gitlab.pavlovia.org/

在线程序地址：https://pavlovia.org/#main

网页端已经支持组件列表：https://psychopy.org/online/status.html

python代码和js代码转换文档：https://discourse.psychopy.org/t/psychopy-python-to-javascript-crib-sheet/14601

网页端运行时产生“Unknown Resource” issue报错的原因： 

https://discourse.psychopy.org/t/unknown-resource-online-but-works-fine-offline/25793/10

https://psychopy.org/online/configureOnline.html

https://psychopy.org/online/index.html

如果在本地增加了新的文件夹存放图片或excel条件文件，要提前在build界面的设置中，online选项卡下添加这些resources，不然加载图片或条件会报错，提示无法加载相应的材料

通过build界面`在线搜索`功能解决无法同步程序到网页端的方法（对应tip2）：https://psychopy.org/online/sharingExperiments.html


