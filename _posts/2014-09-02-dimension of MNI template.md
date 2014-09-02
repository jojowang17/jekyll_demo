---
layout: article
categories: [MIP]  
tags: [brain]
title: The dimension of MNI templete
comments: yes
---

Generally, there are two kinds of dimension in MNI template: <font color="#FF0000">(1) 181 x 217 x 181 (1mm resolution) (2) 182 x 218 x 182 (1mm resolution)</font>. For example, the ICBM-152 Atlas that comes from the UCLA web site has dimensions of 182 x 218 x 182 .  Within DTI Studio it is 181 x 217 x 181. There seems to be a single slice  difference between both templates?  Where would the single slice need to be added for both templates to match?

<font color="#FF0000">It is likely that the difference is just 0 padding at the 182th, 218th, and 182th line. You can convert our 181x217x181 atlas to 182x218x182 using the "Crop" function in Landmarker. Another possibility is that it was interpolated, which can also be done by Landmarker. You may want to make sure by subtracting the images (you can do it in RoiEditor). </font>

I don't know why there are two versions with different dimensions. 


Reference: <a href="http://lists.mristudio.org/pipermail/mristudio-users/2009/000709.html">http://lists.mristudio.org/pipermail/mristudio-users/2009/000709.html</a>
