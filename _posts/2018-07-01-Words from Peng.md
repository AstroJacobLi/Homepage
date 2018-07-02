---
layout: post
title:  "Discussions with Peng"
date:   2018-07-01
desc: "Discussions with Peng"
keywords: "Galaxy, Notes"
categories: [Galaxy]
tags: [Notes]
icon: icon-smile
---

我:
manga和galaxy zoo，BPT diagram这些的技术细节，有任何疑问，哪怕是最小的疑问，都要多问问可欣和程鹏，他们比我清楚，确保不要出错。



图1里看，可能一半的ring galaxy是LINER。但我比较好奇那些SFR很高的ring galaxy是什么样的，可能mass比较大？ 因为ring意味着整体的SFR可能比较低了。把color coding换成sSFR看看。



图2里看，大概只有25%的ring galaxy有bar，如果认为P>80%的有bar。但可能和mass，sSFR都有关。但图3里似乎不清楚。

可以在图3里，画ring galaxy的bar fraction（程鹏好像用的P>50%就算bar，你去问问他，你再计算时也可以试试取不同的P的threshold）。因为样本数不多，你可以用大一点的box，比如0.5dex in SFR and 0.5dex in mass。同样的这个图上，画一下ring galaxy的AGN fraction


我:
然后自己也要想想，我们这里是想搞清楚，ring的出现，和bar，AGN有没有关系。


主要问题是那2w个照片的选取是否合理。


######为GZ1建立mask，同时给GZ1的星系建立各个类别比如spiral的mask。non_elli_mask意思是非椭圆星系 ##############
GZ1mask=~np.isnan(all_fits[1].data['SPIRAL']) ## 重点是isnan，GZ1mask为true的位置是有GZ1

