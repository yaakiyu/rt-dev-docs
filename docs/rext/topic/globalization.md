# RT多言語化について
RTは多言語対応するために特殊な仕組み(`core.t`など)を使用しています。  
これらの仕組みについて簡単な解説をします。

## core.t
```python
from discord.ext import commands

from core import Cog, t

class CogName(Cog):
    "〇〇機能のコグです。"

    def __init__(self, bot) -> None:
        self.bot = bot

    @commands.command(description="The command")
    async def command_name(self, ctx: commands.Context):
        await ctx.send(t(
            {"ja": "完了。", "en": "Completed."}, ctx
        ))
```
まずは上のコードをみてみて下さい。

ctx.sendの中に`t(...)`という関数が置かれています。

この関数は、辞書とctxを引数に取り、適した言語に合わせたstrを返す仕組みになっています。  
ctxはcommands.Contextだけでなく、

* discord.Interaction
* discord.Message
* discord.Guild
* discord.TextChannel

など、`.guild`や`.author`属性を持つものは全てctx引数に入れることができます。
