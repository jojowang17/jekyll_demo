---
layout: article
categories: [image_processing]  
tags: [brain, fMRI]
title: Strang result of covariance regression in DPARSFA (DPARSFA剔除协变量之后图像很奇怪)
comments: yes
---
<b>English:</b>

We always need to regress out nuisance covariates in the MRI preprocessing before FC calculation. In the DASRSF advance version, the MRI image becomes strange after the regression. As shown in the figure, we can find that (1) there are singal out of the brain in the regressed image; (2) the whole regressed image looks gray.

<p><img src="https://cloud.githubusercontent.com/assets/8384205/5212725/c2102630-7639-11e4-9fc5-04b77bd7a51f.png" align="middle"></p>
<p>before regression</p>

<p><img src="https://cloud.githubusercontent.com/assets/8384205/5212726/c39a6556-7639-11e4-881b-ebd509fe9b32.png" align="middle"></p>
<p>after regression</p>

This regressed result is correct. Before regression, (1)the signal intensity in the brain is much higher than that out in the background. Hence, we can clearly identify the brain from the background; (2) the BOLD signal intensities of GM/WM/CSF are different, so we can see the structures in the brain clearly. When the regresion is performed, both the signal covariates of head movement, GM/WM/CSF .. and the mean value of the time domain are regressed. The signal difference between the brain and the background as well as the signal difference among GW/WM/CSF become small. Therefore, the regressed brain image looks blurrier than the imge before regression.


<b>中文：</b>

我用DPASRFA2.1版做功能连接前的预处理，做了slice timing、reliagnment、normalise（T1像）、detrend and filter，继续做了剔除头动、全脑平均、白质、脑脊液协变量后，发现图像变得怪怪的，主要有两个问题，一是所有volume在脑外也出现了信号，二是有部分volume图像整个都是灰灰的，如图。过程中文件夹都正常生成了，后面的功能连接图也跑的顺利。
我单跑了剔除头动或剔除全脑平均等，并且用rest软件试了下，换成spm5等等，还是这种情况，为什么输入软件推荐的步骤，出来的结果却很奇怪呢。后来发现spm qq群里有几个人都是类似的情况，所以很希望老师们帮忙分析下，是那里的问题？如果这一步可以纠正，我还是希望用DPARSFA做，毕竟简单快捷。另外建议老师们能上传下你们处理的中间数据的示意图，我们可以作为质量控制的标准。


Re
解释一下为什么回归之后你会看见这样的图像。在做回归之前，脑内的信号是远高于脑外的，所以你在图像上可以看见清晰的脑内脑外差别。同理，灰质白质脑脊液三者的BOLD信号强度也是不一样的，所以在回归之前，你能够很清楚地看到大脑的图像。然而，在做回归的时候，不仅会去掉头动、白质…等等协变量信号，“时域均值”也同时去除掉了。所以这个时候，脑内信号和脑外信号的“时域均值”都被去掉了，你不会再看到脑内信号非常明显地高于脑外信号。同样，灰质白质之间的差别也相应被取消掉了，因为这个时候你看见的只是变异，已经没有时域均值了。当然，由于脑外信号大都是一些随机噪声，所以你会看见脑外的信号弱且随机性强。时域均值的去除与否，都不影响后面的频域分析（如ALFF）或相关分析（如功能连接或类似看相似性的ReHo）






Reference: <a href="http://restfmri.net/forum/node/1099">http://restfmri.net/forum/node/1099</a>





