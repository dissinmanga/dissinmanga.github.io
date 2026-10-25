---
date: 2026-10-25T21:29:38+08:00
draft: false
title: 內疚
description: 在梳開那些愛恨情仇後，也許最讓我感到痛苦的，一直是那個無法理解各種設計是被生出來解決什麼問題的自己。
summary:
---
{{< katex >}}
如果說挫折總要隨便找個理由發洩出去，那這個理由通常不會多有道理。

我對C++有一個很奇妙的反感點[^3]，array與vector共存，我知道這件事很荒謬。一個靜態語言在使用一段連續的memory時，必須事先講清楚我要用多少（`int a[16]`一類的），vector則是把宣告在heap上，使用到某個長度時則double等細節包成一句`std::vector<int> a(16)`。

類似的心病在`std::string`上也有出現，也許我早該去面對這個糾結背後的點。

我大概會用，當時我遭遇到的各種無助與無望，全數以「一件事應該有一個完美做法」的想法合理化後輸出，這種角度重新理解當時的挫折。在做了更多頭腦體操後，我終於逐漸發現，不用額外偵測`array_size`去`malloc()`跟`free()`是怎麼樣的一個感覺。

為此我一直在問自己，如果我喜歡實話，那我到底有什麼理由厭惡C++？在梳開那些愛恨情仇後，也許最讓我感到痛苦的，一直是那個無法理解各種設計是被生出來解決什麼問題的自己。

字串在很多地方應該都是地獄級別的題目，純粹的英數字串、英數與CJK交錯的字串，大寫小寫，blah blah的，昨天在做一個判斷字串是否為回文的練習題，被其他語言慣壞的我寫到快發瘋[^4]。

總之，我第一個寫出來的版本長這樣︰

```rust
pub fn is_palindrome(s: String) -> bool {
	let mut t = s.chars()
		.filter(|c| c.is_ascii_alphanumeric())
		.map(|c| c.to_ascii_lowercase());
	loop {
		match (t.next(), t.next_back()) {
			(Some(a), Some(b)) => if a!=b { return false },
			_ => break
		}
	}
	true
}
```

當時在寫時，除了覺得感覺像在作弊外，又再一次理解了特意在array外包一層`String`、`Vec`的原因——在rust裡是不能用index直接去翻`String`的[^5]，就是考量到CJK、emoji的寬度跟英數字元不同，所以rust希望你在這麼幹之前先寫出`as_bytes()`[^1]，有點像是你開車壓到道路邊線時設計成讓汽車尖叫，你要主動跟汽車說︰「對，我現在就是要壓邊線，thanks for your opinion and shut the fuck up.」[^2]。

有夠安全，被這層門擋在外面的我不禁這麼想，而在我還跟rust不太熟悉的狀況下，幾乎是被迫去用雙端迭代器來處理這題。

用上`as_bytes()`後就能解鎖two pointer這個老手段來做︰

```rust
pub fn is_palindrome(s: String) -> bool {
	let (mut f, mut r, s) = (0, s.len()-1, s.as_bytes());
	while f < r {
		while f < r && !s[f].is_ascii_alphanumeric() { f+=1 }
		while f < r && !s[r].is_ascii_alphanumeric() { r-=1 }
		if r == f { continue }
		match s[f].eq_ignore_ascii_case(&s[r]) {
			true => {f+=1; r-=1},
			false => return false
		}
	}
	true
}
```

想了很多事，我現在其實是在用有點沉痛的心情在寫這篇文，那種自己的熱忱並沒有好好轉化成能幫到這個世界的空虛感。一想到我好像是真的非常喜歡寫的當下，內心感覺空了好大一塊。

同場加映我以前用Python寫出來的一種解法︰

```python
def isPalindrome(self, s: str) -> bool:
	s=''.join([i.lower() for i in s if i.isalnum()])
	return s==s[::-1]
```

效能？都用Python寫了，跑得出東西就好了吧？

[^1]: 我有點懶得去求證C++是怎麼設計它的，但大概是門戶大開，你不小心從窗戶摔出去是你的問題。

[^2]: 雖然我嘴上這麼說，我對Rust真的只有感謝，它讓我找到了某種平靜。

[^3]: 現在已經沒有了，之前我還有在玩social media時，發現這類心境有反轉的文章，如果沒有讓讀者跟上反轉會被誤會，而我對自己的寫作能力已經沒有信心了，所以在這邊補充。

[^4]: 我是滿粗心的人來著，而我對於`fold()`, `map()`, `filter()`一類可以減少各種index與迴圈的函數情有獨鍾，像這題，其實我第一版寫得很快，基本只花了一點時間在怎麼完成alphanumeric的判斷跟大小寫——如你所見，現代語言把這些操作都包好了。雖然時間複雜度都是\(\mathcal{O}(n)\)，我不確定rust對iterator的惰性求值特性是否能把看起來像要對同個`nums`跑兩次迴圈壓成一次，不知道怎麼trace，我就爛。

[^5]: 這讓我回想起一件事，以前跟朋友討論時，一致認為OO的封裝為的就是這類保護，我原本想說OO已死，不過，與其說它死了，不如說是OO這個包裝已經不再新鮮，但被打包起來的各種概念已經成為了膝反射。
