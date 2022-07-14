[ホームへ戻る](../../README.md) [RextRTトップへ戻る](../README.md)

# RextRT式Cogの使い方
ここではRextRT式のCogの使い方について説明します。

## Cogとは
[`discord.py`](https://discordpy.readthedocs.io/ja/latest/index.html)の機能です。  
この機能を活用すると、機能ごとにコマンドを分割したり、ファイルを分けたりできます。  
ここではCogの基本的な使い方を知っているものとして解説します。

## importする
まず初めに、Rext式のCogは特定の場所からimportする必要があります。  
一部の機能が使えなくなるだけでなく、基本的な動作にも支障が出る可能性があります。  
```python
from core import Cog

class CogName(Cog):
    "〇〇機能のコグです。"

    def __init__(self, bot) -> None:
        self.bot = bot
```
こんな感じで、Rext式でも基本的には通常のコグと変わりはありません。

## いくつかの追加機能
Rext式Cogを使うことでいくつかの便利な追加機能を使うことができます。
```python
# ① Cog.group_index
### これはグループコマンドの親コマンドに置き、サブコマンドなしの時に「使用法が違います」と表示してくれるものです。
### 使用例↓
    @commands.group(...)
    async def command_name(self, ctx):
        await self.group_index(ctx)

# ②Cog.Embed
### titleが位置引数になったEmbedです。また、カラーがデフォルトで適用されるようになります。
### 使用例↓
    @commands.command()
    async def command_name(self, ctx, *, arg):
        await nanraka_no_syori(ctx, arg)
        await ctx.reply(embed=Cog.Embed("Ok", description="Ok."))
```
基本的な使い方は以上です。
