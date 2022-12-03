# Rextlib.py
rextlibの中に入ってるものを解析した結果。ちなみにrtlibから変わってるところはあまりない。

## Cacher
キャッシュ管理。`bot.cachers`を使って以下のように操る。
```py
if TYPE_CHECKING:
    from rextlib.common.cacher import Cacher
    from core import Core

# 略
class CLAAAAASSSS:
    def __init__(self, bot: Core):
        self.bot = bot
        self.cache: Cacher[str, int] = bot.cachers.acquire(300.0)
```
これで5分キャッシュの出来上がり。  
`self.cache["my_age"] = 15`みたいにやると、5分間はアクセスできても5分後に自動で消える。  
`bot.cachers.acquire(float)`で指定秒数経つと消えるキャッシュができる。

使い方を単純にいうと、スパム対策。(というかそれ以外の使い道思いつかない)  
一回DBにアクセスする機能でアクセスされたら結果をキャッシュに入れといて、短い期間で何回アクセスされてもキャッシュから返せばDBに負荷がかからない。  
なおCacherは基本的にただの`dict`の拡張なので(正確にはdictにあるいくつかの機能が使えないが)dictっぽく使っていって大丈夫。


