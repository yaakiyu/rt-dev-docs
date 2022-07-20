[ホームへ戻る](../../README.md) [RextRTトップへ戻る](../README.md)

# 新RTのDatabaseManagerについて
*注意 - この項目ではRext式新RTのDatabaseManagerしか扱いません。それ以前のRTのDatabaseManagerの仕組みは自分で調べてください。*

*注意2 - この項目では主に使い方について解説します。仕組みを詳しく知りたい場合は自分で調べてください。*

## データベースの種類は?
RTはデータベースに[aiomysql](https://github.com/aio-libs/aiomysql)の`Pool`を利用しています。  
生でデータベースを操作したい場合は`bot.pool`を利用してください。(非推奨ですが)  

名前から分かる通り、データベースの種類は**mysql**です。ここではmysqlの解説はしませんのでご了承ください。

## DatabaseManagerとは?
RTではデータベースの操作が簡単にできるようにDatabaseManagerという仕組みを提供しています。  
通常、データベースを扱いたいのであればこれを使うのがいいでしょう。ただし仕組みは結構特殊ですが。

## DatabaseManagerの基本的な使い方
マネージャーを継承したクラスを作成します。基本的にはそれだけです。  
(クラス継承がわからない人は、Cogのようなものだと思ってください。)

```python
from core import DatabaseManager, Cog


class DataManager(DatabaseManager):
    "〇〇機能のデータマネージャーです。"

    def __init__(self, cog) -> None:
        self.cog = cog
        # (下のself.poolは必須です。これがないとbotが動作しません。)
        self.pool = cog.bot.pool
```
そして次に、これをCogの方に紐付けさせます。
(関連項目: [Cogについて](cog.md))
```python
# 略

class CogName(Cog):
    "〇〇機能のコグです。"

    # このCogはcore.Cogですが、commands.Cogとほぼ同じように使えます。

    def __init__(self, bot) -> None:
        self.data = DataManager(self)
        # 名前はdataじゃなくてもいいのですが一般的にはdataを使います。
        # cogを引数に取るのでselfを渡してあげましょう。
```
また、何かテーブルを使う場合は(使わないことはほとんどないと思いますが)テーブル初期化用のコードを書いておきましょう。  
それを`cog_load`関数(Cogのロードが終わるとdiscord.pyに自動で呼ばれるasyncの関数です)の中でそれを実行します。
```python
from core import DatabaseManager, Cog, cursor


class DataManager(DatabaseManager):
    # 略

    async def prepare_table(self) -> None:
        "テーブルを準備します。"
        await cursor.execute("CREATE TABLE IF NOT EXISTS TableName(GuildId BIGINT, Data JSON)")
        # テーブル名やカラム名はそれぞれパスカルケース(単語ごとに大文字始まり)で書いてください。

# 略

class CogName(Cog):
    # 略

    async def cog_load(self) -> None:
        await self.data.prepare_table()
        # ↑関数の名前も習慣的にprepare_tableが望ましいです。
```
ここで一つ気づいたことがありませんか?  

そうです。`cursor`をimportして使っています。  
このDatabaseManagerは自動でcursor引数を関数に増設して使っているようです。  

しかし、実際のコードにはcursor引数なんてないから見かけ上はエラーが出てしまいます。  

そこで、cursorをimportしてそれを使っているように見せかけているのです。  
ちなみにDataManagerの外でcursorを使うことはできません。(実際の中身はNoneなので)  

cursor引数は自動で渡されるので、cogからDatabaseManagerにアクセスするときのコードには何も書く必要がありません。
```python
from rtlib.common.json import dumps
# jsonモジュールに関してもrtlibのものを使用する必要があります。

# 略

class DataManager(...):
    # 略
    async def add_data(self, guild_id: int, data: dict) -> None:
        "データを追加します。"
        # DatabaseManagerのそれぞれの関数に説明やアノテーションはつけておきましょう。
        await cursor.execute(
            "INSERT INTO TableName VALUES (%s, %s);",
            (guild_id, dumps(data))
        )

# 略

class CogName(Cog):
    # 略
    @commands.command(...)
    async def add_data_one(self, ctx, data: str):
        await self.data.add_data(ctx.guild.id, {"data_one": data})
        # ↑cursor引数を自分でつけたりする必要はありません。
        await ctx.reply("Ok")
```
基本的な使い方は以上です。

## 応用編
いくつかの覚えておきたい応用的な書き方が存在します。

### cursorの無駄遣いを減らす
cursorは関数が一回実行されるたびに生成されます。

ということは、DatabaseManagerにある関数の中で別の関数を呼び出すと、cursorが2個生成されることになります。  
同じcursorを使って処理をするために、キーワード引数を使うことができます。
```python
# 略

class DataManager(DatabaseManager):
    # 略

    async def check_data(self, user_id: int) -> bool:
        "データのモードがAAAかどうかを調べます。"
        if await self.data_exists(user_id):
            await cursor.execute("SELECT Mode FROM TableName WHERE UserId = %s;", (user_id,))
            return (await cursor.fetchone())[0][0] == "AAA"
        return False

    async def data_exists(self, user_id: int, **_) -> bool:
        "この機能がオンになっているかを調べます。"
        await cursor.execute("SELECT OnOff FRON TableName WHERE UserId = %s;", (user_id,))
        return bool((await cursor.fetchone())[0][0])

```
上のコード例の`data_exists`関数のキーワード引数、`**_`がそれにあたります。  
実際にキーワード引数を利用するわけではないので引数名は`_`になっています。
