---
date: 2026-11-02T23:39:30+08:00
draft: false
title: 無題 ⑥
description:
summary: " "
---
今天想給自己~~偷懶~~反芻的時間，所以寫點minigrep的雜記。

起初接觸螃蟹時，我最不習慣的是例外處理這塊，舉個例子，今天你想在自己寫的一首小詩裡找有沒有包含frog的關鍵字，於是你下了這個指令︰

```
grep frog poem.txt
```

我把`grep`換成自己寫的`fakegrep.py`︰

```python
#!/usr/bin/python3
import sys
print(sys.argv)
```

```
❯ ./fakegrep.py frog poem.txt
['./fakegrep.py', 'frog', 'poem.txt']
```

在螃蟹裡可以用iterator的形式讀進來︰

```rust
use std::env

fn main(){
	let args = env::args();
}
```

你可以把iterator想成一條隊伍，~~繞一大圈只為了鋪iterator~~，只要你對它喊下面一位︰

```rust
args.next();
```

隊伍的下一個成員就會跑出來，但你喊下面一位時看不到這個隊伍裡還有沒有人，rust包了一層有人（`Some()`）和沒有人（`None`）的殼在外面[^1]——而只要操作有一層不確定性，在rust裡都會包一層殼。

說起來在我剛接觸時，覺得這樣一直剝殼（`unwrap()`）實在有點惱人[^2]，不過我覺得這個設計很合理，而且rust的語法是有配合到的。

假設現在我要把搜索的字串讀進來，可以寫成這樣︰

```rust
args.next();
let query = match args.next {
	Some(arg) => arg,
	None      => return Err("Didn't get the query string.")
};
```

當args隊伍裡還有人時（`Some(arg)`），透過樣式比對的語法就可以在剝殼的同時把錯誤的狀況也一起交代掉。

或假設說，你只是想把字串讀進來印一下，沒特別想幹嘛︰

```rust
args.next();
match args.next {
	Some(arg) => println!("you passed the string {arg}"),
	_         => ()
};
```

起初我還覺得match的排版好整齊好好看，寫久就在想，靠北這語法怎麼這麼冗。後來就被[clippy](https://en.wikipedia.org/wiki/Rust_(programming_language)#Clippy)戳，表示可以把第二段的語法寫成這樣︰

```rust
args.next();
if let Some(query) = args.next {
	println!("you passed the string {query}");
};
```

昨天也被戳，其實第一段語法可以這樣寫︰

```rust
args.next();
let Some(query) = args.next else { return Err("Didn't get the query string."); }
```

後來[^4]看了一下才發現書有[提到](https://doc.rust-lang.org/stable/book/ch06-03-if-let.html)，看著下面那句話︰

> If you have a situation in which your program has logic that is too verbose to express using a `match`, remember that `if let` and `let...else` are in your Rust toolbox as well.

我不是個喜歡記語法糖的人，是故心情實在有點複雜，因為我是真的覺得`match`寫久還…滿…冗…的…。

題外話，原本我是因為讀了[這邊](https://doc.rust-lang.org/stable/book/ch09-02-recoverable-errors-with-result.html#propagating-errors)，所以我一開始是這樣寫︰

```rust
args.next();
let query = args.next().ok_or("Didn't get the query string.")?;
```

但看著乾淨的錯誤傳遞流[^3]，我果然還是喜歡這套設計。

另一個題外話，在我用`contents.split('\n')`把詩按行切成iterator時，看到書裡這樣寫：

```rust
contents.lines()
```

我不知道要怎麼把內心幹意如萬馬奔騰的悸動轉化成「原來還有這麼寫法，又學到一課了！」，但我會努力的，不過說真的，這個對一門語言越來越熟悉的過程，還是很令我高興。

我真的該多練習專注在內心的感謝的。

[^1]: 據說這就是大名鼎鼎的monad，但，內函子範疇上的一個么半群…我好像不用勉強自己一定要讀懂…。

[^2]: 剝殼但不處理例外是壞習慣。

[^3]: 看著go也有類似[設計](https://go.dev/doc/tutorial/handle-errors)，突然覺得自己真的好脫節。

[^4]: 又再一次發現自己對沒有興趣的東西，真的是完全不會注意到。
