---
layout: article
categories: [image_processing]  
tags: [brain, fMRI]
title: The preprocessing of resting-state fMRI (静息态fMRI数据的预处理)
comments: yes
---
<b>English:</b>

Before the introduction of the preprocess, I will first introduce the format of the resting-state fMRI data. The brain is scanned by a scanner and we can get the .dcm files.  .dcm is 2-D data and stands for a slice of brain. We need several slices to construct a 3-D brain data (.img/hdr or .nii). There are several toolboxes to perform construction, such as SPM, dcm2nii. .nii file is the most commonest format of the fMRI data, and there are 3-D data and 4-D data for the .nii file. The 3-D data is a brain data. The 4-D data is a series of brain data at different time points. 

Before the preprocessing, the .dcm files need to be converted to .nii files or .img/hdr files. Then we need two matlab toolboxes to preprocess the resting-state fMRI data, i.e. SPM and REST. The whole preprocessing includes discarding the first 10 time points, slice timing, realign, normalize, smooth (For ReHo, do smoothing after ReHo calculation), detrend, filtering (For ALFF, do smoothing after ALFF calculation). After the preprocessing, we calculate ALFF, ReHo or FC. Slice timing, realign, normalize, smooth are all conducted in SPM. The other procedures are conducted in REST.

 (There is a button called "filt" to choose image files in SPM. Delete ".*", then input other characters to choose image files. "^x" means choosing the files whose names begin with x. "￥x"  means choosing the files whose names end with x. "x"  means choosing the files whose names include x) 
<img src="https://cloud.githubusercontent.com/assets/8384205/5237711/b6e8719a-78d2-11e4-9777-4ee4ccf45b84.jpg">

(1) Discarding the first 10 time points. Considering the instability of the subject at the initially scanning time, the image data at the first 10 time points must be discarded. 

(2) Slice timing.

(3) Realign
(4) Normalize
(5) Smooth
(6) Detrend,
(7) Filtering.
(8) Calculate ALFF/ReHo/FC.
<b>Chinese:</b>

 静息态fMRI数据预处理主要用到的两个工具包是SPM和REST（注意路径不能有中文）。 在介绍数据预处理以前，先介绍一下rest data的数据格式。rest data数据格式可以认为分为三种：.nii,.img/hdr,.dcm。.nii是最常用的一种，也是最通用的一种，它分为三维数据和四维数据两种。三维数据较好理解，因为一个大脑就是一个三维数据。四维数据是指将多个大脑封存在一起。.nii数据SPM可以直接处理，不管是三维还是四维。但是.nii数据不可以用REST处理，若要用REST处理，必须将其转换成.img/hdr的格式，这个可以用REST实现。.img/hdr数据SPM和REST都可以直接处理。.dcm数据是一层一层扫描的数据，是二维的，首先要将其转化成三维数据（.img/hdr），这个可以用SPM实现。一般来说大概30个左右的.dcm可以转换成一个三维数据。这里说明一下.nii数据的前期准备工作。SPM虽然既可以处理三维也可以处理四维数据，我觉得最好还是将四维数据转化成三维数据，这样方便以后处理。转换的软件是和MRIcroN一起的dcm2niigui.exe (它也可以将dcm转换成三维数据)。首先在Output Format中选择SPM8 （3D NlfTI nii），然后在File中选择Modify NIfTI，然后输入数据即可。
 
 <p>
<img src="https://cloud.githubusercontent.com/assets/8384205/5237709/b1585d8a-78d2-11e4-8424-52ffff2570cc.jpg"></p>
<p><img src="https://cloud.githubusercontent.com/assets/8384205/5237710/b2d704f4-78d2-11e4-9a73-8c3796ff6997.jpg"></p>

这里对MRI数据常用的一个处理软件MRIcroN简单说明一下。这个软件基本功能较齐全，可以观察数据的大小（eg：61*73*61等），定位坐标等。

下面具体介绍预处理的过程。整个过程包括剔除前10幅图像，Slice timing， Realign， Normalize， Smooth（ReHo不需要）， detrend，filter（0.01-0.08hz），计算特征（如ALFF、ReHo等）。2、3、4、5用SPM实现（在SPM中有一个filt用来筛选图像文件，如下图，将.*删去，若输入^再输入一个字母x，就表示筛选出文件名以x为首字母的文件，若输入￥再输入一个字母x，就表示筛选出文件名以x为结束字母的文件，如果只输入一个字母x，就表示筛选出文件名包括x的所有文件），之后用REST实现。

<p>
<img src="https://cloud.githubusercontent.com/assets/8384205/5237711/b6e8719a-78d2-11e4-9777-4ee4ccf45b84.jpg"></p>

（1）去前十个时间点。考虑到仪器和被试刚开始的不稳定性，每个被试前10幅图要剔除。（*.nii）

（2）Slice timing。因为大脑扫描时，每层数据获得不在同一个时间点，需要将其校正在同一个时间点,最终使一个大脑的所有层数是在一个时间点获得。这一步骤在SPM8的使用说明中不建议做，实现这一步骤需要知道数据获得时是逐层扫描还是隔层扫描，扫描层数，以及TR和TA，参考层（一般选择扫描中间的那一层）。一般处理的时候这一步还是做了的。(st_*.nii,st是自己设定的前缀)。

（3）Realign: Estimate & Reslice（头动校正），校正掉头动，而且最后需要去掉头动较大的点（利用rp_st_*.txt，里面的数据绝对值最大值超过2，就去除，如果很多被试都被去除，可以适当将阈值上调，比如3）；（rst_*.nii和meanst_*.nii，rp_st_*.txt）。这里说明一下rp_st_*.txt文件，每个大脑的头动是用大脑在三个主方向的平移值（mm）和围绕这三个方向的旋转角度来衡量的，故是6个参数，有n个脑袋就有n个6个参数，故文件时n行6列数字。注意两点：一、为什么有一个值大于阈值，就要去除该被试信息，而不是只去除这个值对应的脑袋。因为，去掉一个脑袋，这个时间点的数据就丢失了，信息不完整。（其实，我个人认为如果被试比较少，去除了被试实验数据太少了，就可以用插值方法或是其他方法估算出这个时间点的数据，不用去除这个被试的数据）。二、这里的rp.txt文件在FC中还要用到，用去组成协变量来去除协变量影响。这里就有一个疑问：为什么我已经做了头动校正，在算FC的时候还要去除头动影响呢？因为这一步头动校正，并没有完全去除头动的影响。
关于（2）（3）有一种观点是如果图像是顺序扫描，则在头动校正后进行时间校正，如果是交叉扫描，则在头动校正前进行时间校正。

（4）Normalize:Est & Wri(配准)。要将所有的脑袋配准到同一个空间。方法有两种。A.只有功能像，没有结构像，就直接配准到EPI模板上。这里souceimage是meanstrst_*.nii，images to write是rst_*.nii，template image是EPI.nii，bounding box一般设为-90 -126 -72；90 90 108, Voxel size设为3 3 3。（wrst_*.nii）B. by using T1 image unified segmentation(利用T1像对结构像进行分割配准，效果比前者好)。包含三步：一，将结构像和功能像配齐，即将结构像配准到功能像空间里。二，将结构像分割成白质、灰质、脑脊液，此时有一些其他矩阵产生。三，把估计出的参数写到功能像中，利用（*_seg_sn.mat）。具体操作如下：Coregister(Estimate),其中reference image是指和哪个图像配齐，是和功能像，这里选择meanst_*.nii，source image就是结构像，产生的结果保存在原来的结构像里。Segment，Data选择刚刚处理的结构像结果，Clean up any partitions选择Light Clean，在Affine regularisation中选择European Brains或是East Asian Brains,这个选择标准不定，只要保证所有的数据选择的是一样的就好。最后选择Normlize（write），Parameter file是name_seg_sn.mat，Image to write选择rst_*.nii,bounding box一般设为-90 -126 -72；90 90 108, Voxel size设为3 3 3。

（5）Smooth。这里需要设置Gauss核的FWMH（半高全宽）的值，如4mm等。因为平滑之后当前体素的灰度值已受到周围体素灰度值的影响，局部区域有着较好的一致性，而ReHo是用Kendall协和系数来度量的，计算过程也要考虑当前体素周围6,18或是27个体素直接的关系，如果先做平滑，ReHo的计算就没有意义了，所以点这一步在预处理的时候不做，计算ReHo完毕后再做Smooth。这一步骤的目的是减小空间噪声和减小被试解剖结构的差异。

（6）去线性漂移。可发现获得的数据随着时间的增加呈现上升趋势或是下降趋势，这可能是因为机器噪音随时间越来越大，或是人在机器里随时间增长心理发生变化，影响数据等等，它对结果有一定的影响，一般要去除。

（7）低通滤波（0.01-0.08），目的是减少呼吸和心跳等生理噪声的影响，同时也是因为ALFF的计算是聚焦与0.01-0.08HZ 。

（8）计算指标。这三个步骤是由REST一起执行完成的。这里Mask一般选择User-defined mask，比如BrainMask_05_61x73x61.img（这里61x73x61代表尺度，处理的图像是什么尺度，这里就要选什么尺度），Output parameters（ALFF map）中的Directory是存放一个记录参数设置的文件。下面的Divide ~~~ by the mean within the mask一般是要选的，然后点击 Do all即可。至于ALFF、fALFF、ReHo、FC等具体理论在其他文章中记录。
