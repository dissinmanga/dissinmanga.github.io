---
date: 2026-11-09T20:54:46+08:00
draft: false
title: 說走就走
description: 天冷了，人也懶了。彷彿吃螃蟹的季節已然遠去。
summary: " "
---
天冷了，人也懶了。彷彿吃螃蟹的季節已然遠去。

人的狀態總是波動的，我一度希望我能維持著對Rust的穩定熱忱——說來我其實有點糾結，我覺得我暖身還沒暖開，但有個耳語再也令我無法忽視，刷OJ網遊是沒辦法把traits、struct、mod這類的東西化成手感的。

好懶得用逛街的心態去逛github…。

於是又繞回去打拳——靠北我剛練起來的流派被[禁賽](https://leetcode.com/problems/linked-list-cycle/)。

![](https://www.blindfiveyearold.com/wp-content/uploads/2015/06/unacceptable-lemongrab-e1434739410519.png)

說來這似乎是Rust的硬性限制造成的，人稱王老先生有塊地，在Rust裡，這塊地就只能是王老先生的——除非王老先生把產權過渡出去。而王老先生死前若沒有把這塊地交出去，等王老先生死了，Rust就會自己跑過來把這塊地充公。

{{< mermaid >}}
flowchart LR
	subgraph A[王老先生]
		A_next[next]
	end
	
	subgraph B[王小先生]
		B_next[next]
	end
	
	subgraph C[王小小先生]
		C_next[next]
	end
	
	subgraph D[王尛先生]
		D_next[next]
	end
	
	A_next[next]-->B
	B_next[next]-->C
	C_next[next] --> D
{{< /mermaid >}}


而那個OJ平台在用rust實作linked list的邏輯是，如上圖，王小先生的擁有者被做在王老先生的next上（我知道比喻在這裡歪掉了），往後若要在王小先生後面加一個王小小先生，它的所有權會被登記在王小先生的next上，再加一個王尛先生，它的所有權會被登記在王小小先生的next，依此類推。

這時，若我想要把王尛先生的next指到王小先生上，那王小先生該歸誰管？是王老先生的next，還是王尛先生的next？

這件事在其他地方會被放行，但我猜，大概是Rust的Ownership之故，沒辦法用常理做出來，所以就直接被ban掉。

也只能笑了。

~~其實我當下有感受到列表裡的C++在閃閃發光，然而我終究無法拒絕大蟒蛇，舒服。~~

後來我又寫了一題，你知道的，這種不知道下面一位有沒有人在的情境會被Rust包一層`Option`，於是，剝殼剝到手抽筋不說，又再度狠狠地體驗一輪Rust的reference有多安全。

翻了一些以前寫的C，我想應該得改成說：狠狠地回想起linked list的實作細節到底有多細碎。

◆

所以我就臨時起意，來場[說走就走的旅行](https://go.dev/tour/list)。我一邊看一邊在想，好像不該貪多，我現在讀過一輪，體驗了一下迴圈怎麼寫，未來肯定是會忘記的。

~~然後因為語法不性感，所以我Go到一半就下車了，有夠荒謬。~~ 沒啦我還是對自己有點期許，想挑幾個example走一遭，以前有耳聞golang在web領域上的超強生產力，感覺會是個有趣的旅程。
