---
date: 2026-09-27T23:33:22+08:00
draft: false
title: 無題 ③
description: " "
summary: " "
---
最近出於興趣在摸Rust，到了Hash的章節時，不禁在想一些事。

我到現在才發現語言的實作會刻意跟理論名稱區分開來。或該說，實作這邊會著重在底層特性的區別，所以會另外取名。

好比說，在理論中，會看到linked list、stack或queue一類的名詞，但語言提供的實作有幾種，一是連續的記憶體空間array、追加若干特性的vector、或是更高階語言裡的list——寫到這才驚覺自己的不學無術[^3]，我現在也不是很確定vector底層的記憶體空間是否連續，印象中當長度超出宣告的記憶體空間則把整段搬到更長的記憶體等細節被包起來了。Rust保留了Vector。

要弄出這些理論上的儲存空間，我印象中通常是靠自己實作，或選一個語言的實作。

就像是Hash這個，在Python的實作叫Dictionary，而在Rust或Haskell的實作是HashMap。

因為不同的語言在設計當下有自己的考量點，所以這類巴別塔詛咒一類的議題，我倒還能理解——說起來我也很喜歡從語言留下的限制去品味它們被設計出來的動機。

就好比C/C++跟Rust之所以把硬體細節曝露出這麼多，是因為它們被用來設計追求效能的程式；perl當初被設計出來，是為了提供一個能把bash、sed、awk給整合起來的工具，所以它保留了很多方便隨手寫的彈性。

之前我聽過一個說法︰任何警告訊息被張貼出來都是有故事的，請勿垂釣的告示表示這邊真的釣得到魚，請勿戲水表示這邊曾經有人因為戲水發生危險，請勿隨地大小便表示這邊曾經真的有人…。

語言的新特性通常也是這樣被做出來的，沒有什麼特性是「因為我們覺得這樣很cool」而被推出來的，吧？

好吧，有些語言確實是做爽的。也有些語言的出發點比較有時代原因。

像Ruby，這個語言當初的定位，有點把正當紅的object oriented特性帶進Perl這類動態語言中，從而作為Perl的後繼者的味道。只是當潮流不再留在OO，且Python[^1]成為新寵兒後，Ruby就相對比較受冷落了[^2]。

不過我其實沒有寫過太多Ruby，說起來，不管是在Python還是Ruby，我一直沒有把`=`搞懂，打個比方，`a=b`，那`a`在什麼情況下吃的是指向`b`的reference，什麼情況又是把`b`整個clone過來。我印象中Python傳的全是reference的樣子。

目前我Rust摸著摸著，有種設計者想通過引入以Haskell為首的函數式語言的特徵，做出一個C++ Alternative的感覺。看到enum跟pattern matching聯手，以struct、impl與Traits把OO那些比較不受歡迎的特徵拆掉，用Generics去縮限黑魔法化的C++ templates等，看著有點舒服。

我尤喜歡那個重新設計過的enum，終於知道C當初設計出enum[^4]可能是想解決什麼問題。

在摸著Rust時，不禁覺得，這種重新認識一件事物，與當年的自己和解的感覺，真的很美好。

◆

我不知道在哪個時機，才會促使我從站在巨人的肩膀上，往研究巨人的方向轉變。

[^1]: 查了一下才知道Python其實有[萬物皆物件](https://medium.com/@jayantnehra18/python-fundamentals-everything-is-an-object-in-python-156b94c43f8d)的設計哲學，我是知道當年的背景在Python靠Pandas、NumPy等套件，靠DL跟AI殺出來的，但不是很清楚資料科學家沒採用Ruby的原因。

[^2]: 當年其實還有[Twitter搬家](https://medium.com/@mittalyashu/why-did-twitter-switch-from-ruby-on-rails-dac66150044d)的事件，不過前後端發展史我暫時不太清楚。

[^3]: 其實有[實作](https://doc.rust-lang.org/std/collections/index.html)。

[^4]: 翻書時意識到Rust在enum的重新設計是基於C enum跟union。
