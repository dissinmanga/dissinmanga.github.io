---
date: 2026-11-08T23:24:00+08:00
draft: false
title: 義大利麵
description: 如果我不要堅持用C硬幹的話，今天我就可以多享受一下人生，然後這篇文章可能就會更多采多姿一些的。
summary:
---
這裡快變成解題心得雜談了，笑死；其實我最想翻白眼的地方在，我非常無懸念地選擇了很好逃避的方向在鑽——而照著順序下來，在我把interval和stack寫得差不多後，就是那個，我終於再也避無可避的，linked list。

說來linked list概念上不是什麼複雜的存在，應該吧，只是我在各種搜索中不斷被疊加各種印象，以致於我現在一看到rust跟pointer的組合就非常想閃——那根紮在腦裡的怕麻煩之針真的是永遠拔不掉。

總之今天的[份](https://leetcode.com/problems/simplify-path/)是把路徑字串裡的`/./`、`/../`處理掉，不就是把`/./`拔掉，把`/../`連著前面那一格的字串拔掉，最前面那個代表根目錄的`/`留著，不以`/`結尾，這樣而已嗎？

我整理一下：

+ 讀到`/`時
	+ 前面是`/`：`//`等於`/`，跳過這一回，繼續往下個字元前進
	+ 前面是`/.`：把`/.`吐掉，繼續往下一個字元前進
	+ 前面是`/..`：把`/..`連同前面的`/blah`一起吐掉——不要吐到blah前的`/`，繼續往下一個字元前進
+ 其餘狀況皆把字元塞進去

其實不用這麼克難，不過因為一時興起，想說來陷C於不義——你應該有發現，如果testcase是以`/.`或`/..`結尾時，會出事：

```C
char* simplifyPath(char* path) {
    char* v = malloc(strlen(path)*(sizeof(char)+1));
    uint16_t top_p = 0;
    uint16_t len = strlen(path);

    v[0]='/';
    
    for (uint16_t p = 0; p < len; p++){
        if (path[p] == '/'){
            if (v[top_p] == '/') { continue; }
            else if (v[top_p] == '.') {
                if (v[top_p-1] == '/') {
                    top_p -= 1;
                    continue;
                } else if (v[top_p-1] == '.' && (2<=top_p && v[top_p-2] == '/')) {
                    top_p -= 2;
                    if (top_p != 0) {
                        top_p -= 1;
                        while (v[top_p] != '/') {
                            top_p-=1;
                        }
                    }
                    continue;
                }
            }

            if (p == len-1) { continue; }
        }

        top_p+=1;
        v[top_p] = path[p];
    }

    if (v[top_p] == '.') {
        if (v[top_p-1] == '/') {
            if ( top_p == 1 ) { top_p = 0;} else { top_p -= 2; }
        } else if ( v[top_p-1] == '.' && v[top_p-2] == '/' ) {
            top_p-=2;
            if (top_p!=0) { 
                top_p-=1;
                while (v[top_p] != '/') { top_p-=1; }
            }
        }
    }
    if (0<top_p && v[top_p] == '/') { top_p-=1; }
    v[top_p+1] = '\0';

    return v;
}
```

寫到後面已經懶得再想新的邏輯去cover`/..`結尾的case，所以就掛了一串補救的在後面。

現在的C應該可以不用寫到那麼克難，就，純粹是我在發瘋。

我一邊寫一邊在想，起初我看到這題還滿排斥的，不知道為什麼，下意識覺得就是要用最hardcore的想法去做——如果是寫過python的人（我不確定能推及到哪個程度，搞不好連Java都是），看到這東西時，下意識都是以`/`當分隔符把字串切開，一坨一坨塞進stack，讀到`..`就pop一次，最後再用`/`把stack整個串回來。

中文字有點多，有點眼花繚亂，所以就寫成code吧：

```rust
pub fn simplify_path(path: String) -> String {
	let mut r = vec![];
	path.split('/')
		.filter(|&x| !matches!(x, "" | "."))
		.for_each(|i| if i == ".." { r.pop(); } else { r.push(i); } );
	format!("/{}", r.join("/"))
}
```

我覺得這兩份code做的事應該差不多——C那個大概在不少邊界條件上會出問題；而Rust這份理論上會因為`join()`的緣故多耗一份memory，不過就看一下跑出來的結果吧：

|      | Runtime | Memory  |
| ---- | ------- | ------- |
| C    | 1 ms    | 8.93 MB |
| Rust | 0 ms    | 2.26 MB |

btw，memory這邊，波峰分別出現在9.9MB跟2.3MB，不過考量到OJ平台底部各種服務造成的可能延遲，總之看成Rust的iterator寫法在底層的optimization基本與C持平就行。

其實C有`strtok`或`strcpy`一類的工具可以用，搞不好用上了也能跟Rust一樣舒服；但我不想再寫第三次了[^1]。

如果我不要堅持用C硬幹的話，今天我就可以多享受一下人生，然後這篇文章可能就會更多采多姿（？）一些的。

好啦我知道工具是無辜的，我要跟C下跪，我對它真的沒有愛，所以壓根不想去摸懂它的API，才會搞出第一part那坨義大利麵，請不要因為我而對C留下不好的印象…。

[^1]: C：幹。