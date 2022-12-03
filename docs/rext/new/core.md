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
上のは`update_prefixes`の関数の中の処理。bot_idにマッチして分岐してる。  
なお、`case _`はワイルドカード(つまり「その他」)。

### CONFIG
`core.config.CONFIG`はデフォルトでは`config.toml`から読み込まれる。(envでpathを変更できるっぽいけど)  
ここにいろんな情報が入る。`config.template.toml`ファイルから作らないといけないらしい。

どんな情報が入るかは`core.config.Config`を参照って書いてあるが2ファイルを行き来するのめんどいんでここに全部まとめたもの置いとく。  
`Config`だが、これまた命名規則がめんどくさくて、全部小文字だとファイル名、先頭大文字だと型を定義したクラス、全部大文字だと読み込んだ実際のデータになってる。これからは「config(ファイル)」みたいに書こうかな
(2022/12/03現在)
```toml
backend_origin = "http://127.0.0.1:8000" ## バックエンドに接続する際のオリジン
is_production = false  ## (bool)本番はtrue
token = "..." ## botのトークン
is_audio_only = false ## RT Audioとして起動する際はtrue
shard_count = 0 ## 動かすシャードの数
## なお、本来はここにshard_unit(接続するシャード数)が存在するはずだが,なぜかない。
topgg_token = "" ## topggのトークン
opus = "" ## opusライブラリのPath
prefixes = "" ## プレフィックス(一覧?)。型の定義はSequence[str]だがデフォルトがstrなんでよくわからん。
## prefixesは未設定だと自動で割り振られるらしい。

[databases.write] ## ここら辺はDB (rextlib.common.configを参照しなきゃいけないらしいがめんどいのであとで)
host = "127.0.0.1"
user = "rt"
password = "..."
db = ""
maxsize = 10

[feature.tts] ## ttsの設定(ここら辺ソースコードだとクラスが絡み合っているのであまり見るのはおすすめしない)
open_jtalk = "open_jtalk" ## OpenJTarkを使用する際のコマンド。Pathが通っていなければOpenJTalk実行部のフルPathを記述すれば良き、かな

[feature.tts.voicevoxes]
main = { url = "http://127.0.0:50021" } ## VoiceVoxサーバーのurl
## サブVoiceVoxを入れたいときは、
## sub = { url = "" }
## を追加すればヨシ。
```
