---
date: 2026-10-27T23:44:33+08:00
draft: false
title: 靈異現象
description: 為什麼我愛大蟒蛇？
summary: " "
---
因為今日份的[感謝正拳](https://leetcode.com/problems/create-grid-with-exactly-one-path/description/?envType=daily-question&envId=2026-10-27)有點問號，玩心大起，就寫了一堆解。

```python
def createGrid(self, m: int, n: int) -> list[str]:
	return ["."*n]+[("#"*(n-1)+".")]*(m-1)
```

~~其實我本來打算以「為什麼我愛大蟒蛇」為題，然後只貼上面那一行，但想想實在太下流，就沒這麼幹。~~[^1]

在整理自己寫過的那些東西時，因為實在有點在意list comprehension和generator expression的效能問題，就又寫了這個[^2]︰

```python
def createGrid(self, m: int, n: int) -> list[str]:
    return [ ''.join(('.' if j==0 or i==m-1 else '#' for j in range(n))) for i in range(m)]
```

理論上來說，list comprehension應該會建立一個list，再經過`join()`產生`str`，雙倍的記憶體損耗，不過兩個實際跑過一次後——我直接列表好了︰

|           | Runtime | Memory  |
| --------- | ------- | ------- |
| ~~四則運算版~~ | 0 ms    | 19 MB   |
| list      | 6 ms    | 19.1 MB |
| generator | 7 ms    | 19.1 MB |

…我感覺內心好像有一塊地方碎掉了。

說起來，雖然最好寫的方式是走直角（第一row直接走到底，或第一個column走到底），但題目只要求畫出一條唯一路徑，所以就~~一邊看著康熙一邊~~把走斜線的邏輯給順了出來︰

```rust
pub fn create_grid(m: i32, n: i32) -> Vec<String> {
    (0..m)
        .map(|j| (0..n)
            .map(|i| if i == j || i == j+1 || j == m-1 && j < i || n <= j && i == n-1 {'.'} else {'#'})
            .collect::<String>() )
        .collect::<Vec<_>>()
}
```

以前在寫python就有某種感覺，那個list comprehension的語法幾乎是把`map()`跟`filter()`交融得異常美麗，就是可惜在這種設計還滿不適合巢狀的。總之，一樣都是跑雙層迴圈的情況下，我更新一下前面的表格︰

|                | Runtime | Memory  |
| -------------- | ------- | ------- |
| Python( list ) | 6 ms    | 19.1 MB |
| Rust           | 0 ms    | 2.2 MB  |

其實我只是在練手，我也知道這個比較是不公平的，Python的語法還是比較適合只想管邏輯的狀態的…應該啦…。

不過在賽後檢討的時候，有個點讓我有點在意，於是試了一下︰

```
> a="Happy";a[2]='b'
Traceback (most recent call last):
  File "<python-input-0>", line 1, in <module>
    a="Happy";a[2]='b'
              ~^^^
TypeError: 'str' object does not support item assignment
```

說來我一直有Python有很多地方是immutable的印象，除了前面提的`str`外，以`a=5; a=6`為例，在Python底下是`a`這個reference從指向5這個物件改成指向6這個物件︰

```
> a=5; print(f'{id(5), id(a)}'); a=6;print(f'{id(6), id(a)}')
(139983249484176, 139983249484176)
(139983249484208, 139983249484208)
```

到頭來，5跟6是不是immutable好像也不是那麼重要，Python的實作方式讓你根本無法意識到這之間的差別，反倒是這些reference指向mutable的物件時才會有各種靈異現象。

我早該把這些東西寫下來的。原本我打算寫點巴別塔詛咒之類的東西去嘴嘴list對應到`append()`、vector對應到`push()`或`push_back()`之類的事，不過比起那些還要發明shallow copy、deep copy一類的名詞來解決的靈異現象——我放在下面，有興趣可以看看——巴別塔詛咒什麼的似乎也就微不足道了。

人好像真的總被自己發明的東西所困。似乎只要直視著鬼，不試圖發明更多言不及義的名詞，它就不再那麼可怕，也許。
### 列表
#### Python

```
> a=[1,2];b=a;b+=[3]; print(f'{id(a)}: {a}, {id(b)}: {b}')
139873420031744: [1, 2, 3], 139873420031744: [1, 2, 3]
```

#### Ruby

```ruby
b=a; c=a; b+=[2]; c.append(5)
p a, b, c
p a.object_id, b.object_id, c.object_id
```

Result:
```
[1, 5]
[1, 2]
[1, 5]
20
24
20
```

### 字串
#### Python

```
> a="Happy";b=a;print(f'{id(a)}: {a}, {id(b)}: {b}')
139873417073344: Happy, 139873417073344: Happy
> b+=" Holiday"; print(f'{id(a)}: {a}, {id(b)}: {b}')
139873417073344: Happy, 139873417506480: Happy Holiday
```

#### Ruby

```ruby
a='Happy'; b=a; c=a
b+=' Holiday'; c[2]='d'
c.concat(' Birthday')
p a, b, c
p a.object_id, b.object_id, c.object_id
```

Result:

```
"Hadpy Birthday"
"Happy Holiday"
"Hadpy Birthday"
12
16
12
```

[^1]: ~~所以我放在另一個有緣就看得到的地方了。~~

[^2]: 這是generator的版本，把`join()`裡面最外層的小括改中括就是list comprehension的版本了。
