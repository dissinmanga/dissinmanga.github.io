---
date: 2026-11-08T00:50:03+08:00
draft: false
title: 所以自由
description: 好比因為限制所以自由，因為猶豫，那些本該有機會進去的大門，總也會關得果決。
summary: " "
---
近期在整理各種螃蟹食記（？），頓覺某種，見山又是山。

總之，基於某種情懷，我又回去看那些，從前沉迷其中的各種宗教戰爭文，我沒想過以前居然能津津有味地讀著它們，連型別後置這種小事也可以挑出來嘴，好懷念。

by the way，這是型別前置：

```C
int dissinmanga = 2;
```

這是型別後置：

```rust
let dissinmanga: i32 = 2;
```

而Rust的compiler會嘗試推導型別，所以其實也可以這麼寫：

```rust
let dissinmanga = 2;
```

在這一波搜索中，意外得知早在[C++11](https://en.cppreference.com/cpp/language/auto)也支援類似的自動推導寫法：

```cpp
auto dissinmanga = 2;
```

而在我早習慣python那個迴圈的寫法時：

```python
for i in range(5):
	print(i)
```

又意外發現，早在C++11（又是C++11）就支援類似寫法：

```cpp
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> v = {1, 2, 3, 4, 5};
    for (int n : v)
        std::cout << n << ' ';
    std::cout << '\n';
}
```

~~天下文章一大抄。~~

◆

昨天寫[這題](https://leetcode.com/problems/insert-interval/description/)時，花了一點心力。

一般來說，這題的切法就是從頭一個個檢查，跟要安插的區間沒有重疊的就直接塞到要回傳的空array，遇到重疊到的，就把在區間裡的那些區間吐掉，直到出區間為止，把這些併起來後塞回去，再把剩下的那些全塞回去。

我在念經？

概念上很簡單，但邊界條件一大堆——我一看就不想寫，笑死。於是，我無聊拉到hint看了一眼：

> Can you use Binary Search to find the correct position to insert the new Interval?

> Can you try merging the overlapping intervals while inserting the new interval?

簡單說，就是找到重疊的範圍，把這些重疊的區間吸掉後，再把合體完的區間塞回去。說到尋找重疊的點，基本上就是直接從`find`下手，只有在要求效能時才會特意去找結果差不多但速度可能快一些的`binary search`——寫到後來，我已經不知道狂用`std`事先實作完的function算不算是邪門歪道。

當下看到這兩行提示，看著手上的那些還很生疏的工具。於是，接受挑戰：

```rust
pub fn insert(mut intervals: Vec<Vec<i32>>, new_interval: Vec<i32>) -> Vec<Vec<i32>> {
    let k = |a,b| intervals
        .binary_search_by_key(&new_interval[a], |x| x[b])
        .and_then(|x| Ok(x+a))
        .unwrap_or_else(|x| x);
    let (f_l, f_r) = (k(0,0), k(0,1));
    let (r_l, r_r) = (k(1,0), k(1,1));

    let f = if f_l == f_r { new_interval[0] } else { intervals[f_r][0] };
    let r = if r_l == r_r { new_interval[1] } else { intervals[r_r][1] };
    intervals.splice(f_r..r_l, [vec![f,r]]);
    intervals
}
```

btw，`splice()`的功能是把`intervals`裡`f_r..r_l`這個range的範圍替換成`[vec![f,r]]`，簡直跟那個merging the overlapping intervals whild inserting the new interval是天作之合。

我幾乎可以看到，當下敲出這鬼東西的我大概就像這樣：

![](/blogpics/261108_brainlet.webp)

那些邊界條件不知道殺了我多少腦細胞，然而在死了這麼多腦細胞後，我還是不知道這玩意怎麼拿到AC的。

總感覺我的思考可以再軟一些，不知道在硬什麼。

◆

其實我對`splice()`這個字有很稀薄的印象，在做了一點調查後，這玩意似乎在前端圈子是[常識](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)，在我試用它時，也有感受到它是個瑞士刀一般的存在，很實用。

像我先前在[這篇]({{< relref "261025_i_aint_the_shit" >}})寫過的，在Rust裡，不管是類似Python的做事方法，或是偏C的做事方法，似乎都有一種熟悉的方式可以把它們表達出來，於是又讓我再次感受到，人透過語言與其他人事物溝通，語法只是語法，重點一直是背後的人。

每每在這種微小的地方意識到自己不擅長與人相交時就覺得有點難受。不過在這些小地方感受到可能各種背景跑來試Rust的人可能都可以找到屬於自己的一見如故點，總是會讓我感受到某種站在巨人肩膀上的感動。

而Rust那個因為各種ownership被compiler扁成豬頭的經驗，也不是語法上能感受到的。就像golang為人稱道的似乎是它那個超快的編譯速度，重要的東西總是在看不到的地方，不禁這麼想。

我懷疑rust之所以能成功創造某種效能怪物的印象，是因為基於安全的各種限制被做進compiler，所以工程師反而更能放開手去測紅線。

因為限制，所以自由，對。

◆

至此，再回望那些各種針對語法的宗教戰爭文章，真的好懷念。

對大多數的人而言，黑夜過去，黎明終將到來，似乎是象徵著希望；但於我，似乎帶著某種哀愁，我不知道這種情緒是怎麼來的。

以前看千と千尋の神隠し時——我曾聽過某個人會因為白龍找到自己的名字而動容，甚至哭得不能自己，不過這部最讓我不能自持的地方，是千尋在跟著失而復得的爸媽走出隧道時，那個向著隧道口凝視的長鏡頭，而後頭也不回地離開這個地方，電影結束。

![](/blogpics/261108_chihiro.webp)

不得不說[選圖](https://www.ghibli.jp/works/chihiro/#frame)的人真的很有sense。

那種不管是黑夜或黎明、熾熱或餘溫，終究只是過客，這類的敘事總是很勾我，我也許就少了千尋那點果決——說來，人生倒也矛盾，好比因為限制所以自由，因為猶豫，那些本該有機會進去的大門，總也會關得果決。

外邊晝夜交替，我總是缺了那點再向前一步的勇氣。