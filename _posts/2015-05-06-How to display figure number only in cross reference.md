---
layout: article
categories: [office]  
tags: [word]
title: How to display figure number only in cross reference using word (word中交叉引用图片只显示数字)
comments: yes
---
<b>English:</b>

Q: Is there a way to have a cross reference to a figure only display the number, without displaying the label?
For example, I want to reference several figures at once, so it is most natural to say "see Figures 1,3,6, and 9".  It is less natural to say "see Figure 1, Figure 3, Figure 6, and Figure 9".

A: After you have inserted the cross-reference, add a so-called numeric picture switch to specify a number format. To do that, press Alt+F9 to display field codes; you'll find that the cross-references are REF fields. Add \# "0" to the end of the relevant REF fields. Press Alt+F9 again to hide the codes, and press F9 to update fields in the selection. 



<b>Chinese:</b>

问题：word中交叉引用图片时怎么只显示数字而不显示标签呢？比如说一幅图片的题注是“Figure 1   The pipeline of xxx”，交叉引用时，默认的选项，如果选择“只显示标签和编号”，则显示的是Figure 1。但有时候，我们不想要前面的Figure，或者我们想显示的是Fig. 1。该如何处理？

回答：还是使用交叉引用，选择“只显示标签和编号”，然后选中引用显示的文字，这里是Figure 1，然后右击选择“切换域代码”（window下也可以按快捷键Alt+F9）。这时Figure 1就会变成域代码{ REF _Refxxxxxx \h },然后在这个域代码的最后加上\# "0"，即变成{ REF _Refxxxxxx \h<font color="#FF0000"> \# "0"</font>}，然后把光标放入域代码中，右击选择“更新域”（window下也可以按快捷键F9）即可。这时候Figure 1就会变成只有1了。如果要显示Fig. 1，再在1的前面插入“Fig. ”就可以了。


<b>References:</b>

<a href="http://answers.microsoft.com/en-us/office/forum/office_2007-word/how-to-display-figure-number-only-in-cross/02ae3cd0-6c3c-401d-990c-5220d7f0be1e#userconsent#" target="_blank">http://answers.microsoft.com/en-us/office/forum/office_2007-word/how-to-display-figure-number-only-in-cross/02ae3cd0-6c3c-401d-990c-5220d7f0be1e#userconsent#</a>
