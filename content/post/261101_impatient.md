---
date: 2026-11-01T20:38:42+08:00
draft: false
title: 忍
description: " "
summary: " "
---
今天做了很多奇怪的事，想來有點羞恥，不過，試著寫下來。

我把那個昨天跟著the book敲出來的minigrep砍了，再試著重寫一遍，我覺得我在惡搞自己，那個進度條倒退的感覺真的是很刺激，再次翻攪出很多東西。

為什麼我好像還是沒辦法把主流程、偵測錯誤的code、建立struct、為其編寫實作、把核心邏輯拆成另一個檔案的整個流程弄得很輕鬆呢？我不是一直都想試試這種test-driven的做法嗎？

先前在加了[新功能]({{< relref "261012_add_fortune_function" >}})後，我又沒好好確定rss那邊的狀況，結果似乎有半個月都讓rss那邊掛了一串亂碼，昨天順著直覺修掉了。

不禁在想這個粗心，是不是，不太適合呢？

◆

我在想，這種把各種方法論落地的過程，是不是還滿像在練拳的，就像是以前的某種慣性下，一感覺到流程要切分就敲出`if`、一感覺到這邊有重覆的樣態就敲個`for`或`while`。

我不知道自己在焦慮什麼，但一想到以前的同伴能反射敲出`assert`做debug時——雖然我只是興趣使然，這種技不如人的焦慮總是壓不下來，明明吸收進來也只是分分秒秒的事而已。

寫到這裡時，我就感覺，自己好像在耍智障，只是在這種焦慮的時候，發現一點字都看不下去的感覺真的好可怕。

一反問自己是什麼時候開始沒有耐心的，就想到以前——我還真的不知道自己以前是怎麼走過來的，如果文字承載的內容是我有興趣的，我通常是耐得住性子讀下去；但文字本身的阻礙高過我對它內容的興趣的話，好像就很難有什麼辦法。

也許我該多試試深呼吸。

不禁在想，我的文字是不是挺浮躁的，我有被鍛練過做些枯燥的事嗎？雖然我很聒噪（only with my cute little keyboard），但我沒有感覺自己有受過什麼正統的作文訓練，不提論說文，連記敘文這種，我頂多是拉個兩三件事就上了，然後國文老師都沒意見。

要我按表操課式地寫文，或好好把一件事情講清楚，搞不好還真的會要了我的命也說不定。

◆

剛剛在滑著social media時看到某一篇在比較C++的`accumulate`與`reduce`的差別，總之我就跑去看了一下[`std::accumulate`](https://en.cppreference.com/cpp/algorithm/accumulate)——節錄其中的sample：

```cpp
#include <iostream>
#include <numeric>
#include <string>
#include <vector>

int main()
{
    std::vector<int> v{1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    
    int sum = std::accumulate(v.begin(), v.end(), 0);
    
    std::cout << "sum: " << sum << '\n'
}
```

Output:

```
sum: 55
```

其實根據document這段：

> `std::accumulate` performs a left [fold](https://en.wikipedia.org/wiki/Fold_\(higher-order_function\) "enwiki:Fold (higher-order function)").

以及[`std::reduce`](https://en.cppreference.com/cpp/algorithm/reduce)這段：

> `std::reduce` behaves like [std::accumulate](https://en.cppreference.com/cpp/algorithm/accumulate "cpp/algorithm/accumulate") except the elements of the range may be grouped and rearranged in arbitrary order.

事情差不多就結束了——等等，亂序是怎樣？[^1]

我先把原本想講的講完，看到這個當下讓我馬上開了python跑差不多的東西︰

```python
>>> import itertools; list(itertools.accumulate([1,2,3,4,5,6,7,8,9,10]))
[1, 3, 6, 10, 15, 21, 28, 36, 45, 55]
```

翻了一下才發現是我們Python最[特別](https://en.wikipedia.org/wiki/Prefix_sum#Scan_higher_order_function)，不過在看了[另一邊](https://en.wikipedia.org/wiki/Fold_(higher-order_function))後，大概是，`accumulate`這個詞本身就很特別。

◆

寫著寫著，我感覺有好多該反省[^3]的地方，像是只看那些酷酷的東西，卻對那些基本功[^2]興趣缺缺什麼的。耐著性子好好幹吧…。

[^1]: 後來看到exception，看起來`reduce()`是通過平行運算加速——反正最後終會摺成一個值，只要運算本身符合交換律與結合律就沒問題。

[^2]: 走完了minigrep的project，我不禁在想，內部的運作邏輯似乎是小事，主軸好像一直是錯誤訊息的處理與傳遞——從一開始，這個project是以哪個階段會翻車、該回傳什麼錯誤開始建構的，要到寫完testcase後才會開始動手實作邏輯。光是第一步Rust仰賴`Result<T,E>`把錯誤訊息處理完這個…有辦法複製到其他地方嗎…？也許我該累積更多經驗才比較能判斷。

[^3]: 整篇文的焦慮味濃到一個我回頭看也很受不了的程度，總感覺，當我想做好一件事的症頭牙起來時，反而變得有點患得患失的…在幹嘛？說起來，我現在還真的有點像是拿著榔頭找釘子敲的人，我好害怕這個有點用力過頭的自己，如果目標夠明確，實在沒必要把可以平靜無波的心湖搞成瘋狗浪四起的。
