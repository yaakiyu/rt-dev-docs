# RT core

## __init__.py
### class Core(commands.Bot)
新RTのコア。今まで`core/bot.py`にあった`Bot`だったもの。  
これからは型付けするときはこれを使うことになるのかな?  
なお詳細はまだ解析中

## config.py
最初と最後にある`auto_all.start_all()`、`auto_all.end_all()`は`auto-all`っていう外部ライブラリのものっぽい。  
startとendで囲まれた部分のソースコードにあるグローバルスコープのオブジェクトを全て自動で`__all__`に登録するやつらしい。

### PREFIXES
コンフィグファイルの中に珍しいmatch構文を発見。  
...いまだにgithubでさえハイライト対応してないのか。
```python
match bot_id:
    case 716496407212589087:
        # RT
        PREFIXES = (
            "rt!", "Rt!", "rT!", "RT!", "rt.", "Rt.", "rT.",
            "RT.", "rt,", "Rt,", "rT,", "RT,",
            "りつ！", "りつ!", "りつ。", "りつ.", "りつ、", "りつ,"
        )
    case 1007179908524539936:
        # RT Audio 1
        PREFIXES = (
            "r1!", "R1!", "r1,", "R1,", "r1.",
            "R1.", "り1!", "り１!", "り１！", "り1！"
        )
    case 1021034698165714994:
        # RT Audio 2
        PREFIXES = (
            "r2!", "R2!", "r2,", "R2,", "r2.",
            "R2.", "り2!", "り２!", "り２！", "り2！"
        )
    case 719434348931186709:
        # RT Canary
        PREFIXES = ("rc!", "rc.", "rc,")
    case _:
        # RT Dev / Other
        PREFIXES = ("rd!", "rd.", "rd,")
```

