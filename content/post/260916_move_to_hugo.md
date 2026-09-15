---
date: 2026-09-16T02:29:15+08:00
draft: false
title: 從Bearblog搬到Github pages
description:
summary: " "
math: true
---
我終於受夠Bearblog了。

其實Bearblog也沒有惹到我什麼，只是在我某一天起床時，實在是受不了總是要先送成草稿再修改連結的工作方式了，恰好[這篇]({{< relref "260914_talkinshit" >}})讓我實在很想試一下用Obsidian當CMS的手感，再加上我一直想找個理由體驗一下Github Actions。

於是我，創了新的Github帳號，建立了新的repository，加入了public ssh key，拉下repository，用`hugo new project dissinmanga.github.io --force`建立新紀元，用`git submodule git@github.com:jpanther/congo.git theme/congo`拉了Hugo的主題。

在Bearblog這邊把舊文章以markdown格式打包匯出，開心的是檔名全是我當初給的連結名稱，我就不用再一個一個慢慢改，痛苦的是Bearblog有自己的Front Matter，還好我有sed︰

+ 用`sed -i "s/publish_date/date/g" *.md`轉成hugo的格式
+ 用`sed -i "//d"`把Bearblog的Front Matter清掉

刷刷搞定舊的文章格式後，再來是把Obsidian Vaults掰成我想要的形狀，其一是把新文章的預設路徑指到`content/post`，其二是把Obsidian的Templates接到Hugo的Archetype——嚴格來說，是放棄Hugo的Archetypes，Templates路徑指到`archetypes`，default改成這樣︰

```yml
---
date: '{{date:YYYY-MM-DDTHH:mm:ss+08:00}}'
draft: true
title: ""
description: " " 
summary: " "
---
```

應該還有更漂亮的改法，但我懶了，笑死。

考量到Github Pages要求repository公開的設定，最好先弄一個`.gitignore`把`.obsidian`設定成不要同步，如果你打算設定Obsidian Git，會需要申請GitHub的access token，我不確定Obsidian會不會把這個token明碼儲存，如果會的話，你一把`.obsidian`丟上去，隨便一個幼稚園小孩都可以把你的帳號當公廁。

所以回顧一下我對obsidian做了什麼手腳︰
+ 更改新檔案路徑$\rightarrow$config放在`.obsidian/app.json`
+ 更改templates路徑$\rightarrow$config放在`.obsidian/templates.json`

這些加完就可以放心把`.obsidian`丟進`.gitignore`了

好煩喔，為什麼我要在這裡寫這種文章，我不是應該要噴漫畫的嗎？

把所有檔案推到repo後，接下來在設定這邊的pages，可以選擇用github actions去接CI/CD，居然不用自己寫設定檔，爽到翻過去。

這邊要注意一下裡面hugo的版本，預設給`0.138.0`，改成一個你喜歡的版本，不然不會成功。

手機這邊記得要用https的格式pull——如果你想的話，用這種格式︰

```sh
https://[your account]:[your PAT]@github.com/dissinmanga/dissinmanga.github.io.git`
```

就不用手動在Obsidian的git plugin設定PAT。

好啦，blog搬完了，就讓我看看Obsidian操作起來有多不直覺吧，也許我會不直覺到不小心跳回去Bearblog也說不定。

◆

好啦我其實認同Obsidian在前置設定上真的有點門檻，而且基於手機Obsidian的git在不知名的狀況下會產生延遲一事，讓我遲遲不願意把Obsidian當成CMS用，不過試試沒損失，你說是吧？

