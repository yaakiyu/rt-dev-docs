[ホームへ戻る](../../README.md) [RextRTトップへ戻る](README.md)

# 初めに
RTへ貢献してくれようとしてありがとうございます。  
まず前提として、RTの開発に参加する方法がいくつかあること、それぞれの方法について必要な知識が違うことを知っておく必要があります。  

* [rt-bot](https://github.com/RextTeam/rt-bot) : `Python`, `discord.py`(その中で特にCogの使い方), `MySQL`(任意)
* [rt-frontend](https://github.com/RextTeam/rt-frontend) : `HTML`, `CSS`
* [rt-backend](https://github.com/RextTeam/rt-backend) : `Python`, `sanic`(pythonのライブラリ)

全ての方法において、githubの基礎知識を持っていることが前提です。  
(issueとPull Requestを出せれば十分です。)  
わからない場合は「github issue 立て方」や「github fork 作り方」、「github PullRequest 出し方」でggってみてください。  
rt-botとrt-backendに関してはPythonの基礎知識が前提になります。  
(エラーの解決能力があれば十分です。)  
frontendやbackendに貢献したいという方は こちら(執筆中) をご覧ください。  
ここでは主にrt-botに貢献しようとしてくれている方向けの解説をします。

# ①issueを立てる
自分の作りたい機能や、バグなどの発見をした場合、issueを立てることができます。  
rt-botリポジトリにおいて「New issue」を押し、テンプレートを選択してissueを立ててください。  
Rextの人たちに自分が何をやるのか知らせるというわけです。  
もし何か問題があれば指摘されると思うので少し待ってみてください。  
何も言われなければ提案したものの作成を始めましょう。

# ②実際の作成
RTはpython(discord.py)で開発されています。  
そのことを頭に入れた上で、自分の目的の項目を参照してください。

## 新機能を作る場合
[RT式Cogの使いかた](topic/cog.md)や、[Databaseの扱い方](topic/databasemanager.md)などを参考にしながら自分の作りたい機能を作りましょう。  
作り終わったらPRを出してtasurenやその他Rextのみなさんにレビューをしてもらいましょう。  
...正直、彼らは**厳しいです**。めげずに頑張ってください。

## バグ修正の場合
執筆中...
