# Psychopy EEG Trigger Notes


</br>

> 本文介绍的是并行端口(Parallel Port)的打Mark方式，串行端口(Series Port)的方式大同小异，可参考：[Sending triggers via a Serial Port](https://psychopy.org/hardware/serialPortInstr.html)
>
> 如果你还不清楚什么是并口和串口，可学习：[你可知道：并口与串口的区别](https://mp.weixin.qq.com/s/g0blLnv6EiQOt1YTfK2dtg)
>
> 正式开始前，你还需要检查你的设备是否已经正确配置了打开端口的必要环境，可参考：[Make sure you have the correct drivers installed](https://psychopy.org/hardware/parallelPortInstr.html#step-two-make-sure-you-have-the-correct-drivers-installed)、[EEG实验中Matlab并口数据位发送和接收的实现方法](https://zhuanlan.zhihu.com/p/84134816)

# 刺激Mark加入方式

**实现目标：**

如图，需要打mark的刺激是**SCREEN_3**这个组件，这个组件的内容出现，mark就要出现。

{{< image src="/imgs/Blog_EEG_Trigger_pic1.png" alt="Stimuli Mark" width="600" height="300">}}

**操作方式：**

1. 在**SCREEN_3**组件下方添加一个 **Parallel Out** 并行端口组件（Builder窗口下，右边Component选项卡 --> EEG选项卡 --> Parallel Out），如上图，`p_port_2`是我添加的并行端口组件
2. 点开`p_port_2`组件的属性选择 ---> [基础]^(Basic)选项卡中--->**Start**，即**Mark**的开始方式。在下拉选项卡中选则[条件]^(condition)，在后方加入代码：`$SCREEN_3.status==STARTED`。表示**SCREEN_3组件**一开始，**p_port_2组件**就发送Mark
3. 在Stop中，将[持续时间]^(duration time)改为0.1，即100ms。这个持续时间建议不低于0.016秒，即60Hz刷新率下的一帧的时间。
4. 然后在[数据]^(data)选项卡中 ---> Start data的框内，输入你条件文件中指代Mark对应的变量名，如下图，我在excel文件中，有一列变量为Mark类型，这一列的名字为condition_type，把这个列名放在这里即可。

{{< image src="/imgs/Blog_EEG_Trigger_pic1.png" alt="Stimuli Mark" width="600" height="300">}}

5. 然后切换到[硬件]^(hardware)选项卡 ---> **Port address**位置选择设备对应的**端口号**

   如果没有你设备的端口号，就需要手动添加 ---> [添加方法](https://psychopy.org/hardware/parallelPortInstr.html)

---

至此，刺激mark的设置完毕。

</br>

# 反应Mark加入方式

> 反应Mark涉及到根据被试的按键正确和错误判断出现的Mark代码，所以就没法使用 **Parallel Out 组件** 来实现，只有使用 **Code 组件** 来实现，原理和E-prime的方式有些类似。

**实现目标：**

- 被试按键判断：正确 ---> 出现Mark  88

- 被试按键判断：错误 ---> 出现Mark  99

- 被试按键判断：无反应 ---> 出现Mark  90

**操作方式：**

1. 如下图，我设置按键组件是**key_resp**，所以在后面添加了一个code组件**code_7**

{{< image src="/imgs/Blog_EEG_Trigger_pic4.png" alt="Stimuli Mark" width="600" height="300">}}

2. 如下图，在**code_7组件**的 **Begin Experiment选项卡** 中输入代码：

   ```python
   p_port = parallel.ParallelPort(address='0x3FF8')
   ```

   - ⚠️该代码的p_port是我随意设置的一个端口名字，但是这个名字可能会和刺激Mark中的Parallel Out组件的名字有冲突，所以建议改为p_port_99，这样就避免和其他默认的端口组件名字产生冲突。
   - ⚠️`address='0x3FF8'`这里的端口号要记得改为和设备一致的端口号。

{{< image src="/imgs/Blog_EEG_Trigger_pic5.png" alt="Stimuli Mark" width="600" height="300">}}

3. 如下图，在**code_7组件**的 **End Routine 选项卡** 中输入如下代码：

   ```
   if key_resp.corr == 1:
       p_port.setData(88)
   if key_resp.corr == 0:
       if key_resp.keys == None:
           p_port.setData(90)
       else:
           p_port.setData(99)
   ```

   - ⚠️代码中，`key_resp`是键盘组件的名称，需要和你自己的名称改为一致的
   - ⚠️p_port是 **Begin Experiment选项卡** 中设置的名称，这两个地方要一致，名称随意改。

{{< image src="/imgs/Blog_EEG_Trigger_pic6.png" alt="Stimuli Mark" width="600" height="300">}}

---

## 消除信道中的反应Mark信息

> 在打入反应Mark后，如果不添加本部分代码，那只有第一个试次的Mark会全部出现，在后面的试次中就只会有反应Mark出现，因为反应Mark打入后，并没有停止信号的输入，它会持续占用通信通道，导致下一个试次的刺激Mark无法呈现。因此，需要在打入反应Mark后，再进行本部分代码的添加，让反应Mark出现一段时间后停止，避免一直占用通道。

**实现目标：**

在反应Mark出现后，消除反应Mark的通道占用。

**操作方式：**

1. 在刺激呈现和反应的Routine后面，新增一个Routine，然后在新增的这个Routine放入一个Code组件，如图中的**code_5组件**，图中的图片组件是我实验设计所需，不是必须的。

{{< image src="/imgs/Blog_EEG_Trigger_pic7.png" alt="Stimuli Mark" width="600" height="300">}}

2. 打开Code组件的属性设置  --->  切换到  **Begin Routine选项卡**  --->  在里面填入如下代码

   ```python
   t_port = tThisFlipGlobal
   ```

   这个表示获取当前的时间

   {{< image src="/imgs/Blog_EEG_Trigger_pic7.png" alt="Stimuli Mark" width="600" height="300">}}

3. 切换到  **Each Frame选项卡**  --->  在里面填入如下代码

   ```python
   if tThisFlipGlobal > t_port + 0.1 - frameTolerance:
       p_port.setData(0)
   ```

   这个代码表示，如果当前时间达到0.1秒，就执行`p_port.setData(0)`，即把p_port端口归零，设置停止数据。这里的p_port就是设置反应Mark的代码中设置的p_port，两个地方的名称要统一。代码中的`0.1`可以改为0.016及以上的时间，单位为秒。

---

到此，psychopy EEG实验打Mark全部完成。

---

</br>

# 最后

目前这种打Mark的方式，经过两次实验的测试，发现有较小几率会出现第一个Mark会掉的情况。直到现在还没有找到问题的根源。解决方法就是在实验开始的指导语的Routine中，给指导语的图片加一个parallel out组件，设置方式和刺激mark的设置方式一样，只是在[数据]^(Data)选项卡中，使用默认设置，即`1`，不放入我们的条件Mark进去，让这第一个Mark先出来。即使是这样，也不能完全保证第一个Mark丢失。很奇怪，经过反复测试，只丢第一个Mark，其他试次的Mark都不会丢。这个问题如果找到解决方法了，会在此更新。

