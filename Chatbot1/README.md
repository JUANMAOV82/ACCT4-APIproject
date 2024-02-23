# Chatbot (UI part) PROJECT
I envisioned my discord bot as a very sleepy, passive bot, with most of its feedback based on using emojis to reply to users in the channel; and I wanted it to use specific emojis to express tiredness or disinterest. Or convey that style through the way it responds, such as using short, less-than-enthusiastic replies.

![image](https://github.com/JUANMAOV82/ACCT4-APIproject/blob/main/assets/%E6%88%AA%E5%B1%8F2024-02-22%20%E4%B8%8B%E5%8D%886.15.02.png)

# code
  ```py
  import os
import discord
from discord.ui import View

intents = discord.Intents.default()
intents.message_content = True

client = discord.Client(intents=intents)


class AnotherView(View):

  def __init__(self):
    super().__init__()

  @discord.ui.button(label="Hi")
  async def button_callback(self, interaction, button):
    await interaction.response.send_message("👋")

  @discord.ui.button(label="Bye")
  async def button_callback2(self, interaction, button):
    await interaction.response.send_message("🥱")

  @discord.ui.button(label="???")
  async def button_callback3(self, interaction, button):
    await interaction.response.send_message("😴")


@client.event
async def on_ready():
  print("Bot is ready!")


@client.event
async def on_message(message):
  if message.author == client.user:
    return

  if client.user in message.mentions:
    ourview = AnotherView()
    await message.channel.send("Thanks for mention.....Zzz", view=ourview)

  if message.content.startswith("$hello....llo.........Zzz"):
    ourview = AnotherView()
    await message.channel.send("what!", view=ourview)


my_secret = os.environ['DISCORD_BOT_SECRET']
client.run(my_secret)

  ```

