---
date: 2026-09-16T02:29:15+08:00
draft: false
title: 從Bearblog監獄穿過充滿屎尿的下水道後終於掉進GitHub監獄
description:
summary: " "
math: true
---
我終於受夠Bearblog了。

其實Bearblog也沒有惹到我什麼，只是在我某一天起床時，實在是受不了總是要先送成草稿再修改連結的工作方式了，恰好[這篇]({{< relref "260914_talkinshit" >}})讓我實在很想試一下用Obsidian當CMS的手感，再加上我一直想找個理由體驗一下Github Actions。

於是我，創了新的Github帳號，建立了新的repository，加入了public ssh key，拉下repository，用`hugo new project dissinmanga.github.io --force`建立新紀元，用`git submodule https://github.com/jpanther/congo.git theme/congo`拉了Hugo的主題。

啊，是的，我選用了Hugo+Congo的組合，聽說Congo也是那種麻雀衰小五臓俱全型的極簡主題，所以就試著用看看，這個主題把我拉的屎裝潢得如此素雅，實在讓我有些害臊。

在Bearblog這邊把舊文章以markdown格式打包匯出，開心的是檔名全是我當初給的連結名稱，我就不用再一個一個慢慢改，痛苦的是Bearblog有自己的Front Matter，還好我有`sed`︰

+ 用`sed -i "s/publish_date/date/g" *.md`轉成hugo的格式
+ 用`sed -i "//d"`把Bearblog的Front Matter清掉

其實我本來就有摸`awk`/`sed`的動機，寫Blog真的會一直讓我遇到類似的需求，~~寫Blog大法好，sed大法好~~。

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
+ 更改新檔案路徑→config放在`.obsidian/app.json`
+ 更改templates路徑→config放在`.obsidian/templates.json`

這些加完就可以放心把`.obsidian`丟進`.gitignore`了

好煩喔，為什麼我要在這裡寫這種文章，我不是應該要噴漫畫的嗎？

把所有檔案推到repo後，接下來在設定這邊的pages，可以選擇用github actions去接CI/CD，說起來，github action居然不用自己寫設定檔，太爽了吧。

這邊要注意一下裡面hugo的版本，預設給`0.138.0`，改成一個你喜歡的版本——請你喜歡`0.160.0`以後的版本，不然高機率不會成功。

接著到Github申請Personal Access Token，通常是放在帳號設定的Credentials，選Fine-grained PAT，縮限repo的範圍，content權限開read write。

手機這邊記得要用http的協定pull——如果你想的話，用這種格式pull︰

```sh
https://[your account]:[your PAT]@github.com/dissinmanga/dissinmanga.github.io.git`
```

就不用手動在Obsidian的git plugin設定PAT。

好啦，blog搬完了，就讓我看看Obsidian操作起來有多不直覺吧，也許我會不直覺到不小心跳回去Bearblog也說不定。

◆

好啦我其實認同Obsidian在前置設定上真的有點門檻，而且基於手機Obsidian的git在不知名的狀況下會產生延遲一事，讓我遲遲不願意把Obsidian當成CMS用。

不過試試沒損失，你說是吧？

如果你知道obsidian不是開源軟體——即時我一直覺得obsidian是手機上最好用的Markdown editor，我怕會有失去obsidian的那一天，我一直避免自己對他上癮。而且像前面提的，誰知道哪天Obsidian的git突然又開始卡，那種由奢入儉的痛苦（一般都說從天堂掉回地獄吧）我不想體驗。

這篇文的脈絡亂得簡直就像是我躺在床上，騎著下巴睡眼惺鬆，用錯的要死的嘴巴對著手機講話在寫這篇文章一樣——說起來，我現在就是這麼幹的，把這種亂七八糟的文章發在部落格上真的沒有問題嗎？快笑死。

我到底有多不尊重blog到什麼地步？

所以我不會考慮把Obsidian加入我的寫作Workflow，手機是寫不了認真長文的。