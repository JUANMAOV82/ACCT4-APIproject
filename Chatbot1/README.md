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

# Result
The robot uses the sleeping emoji (😴) to indicate that it is sleepy, as well as the facepalm emoji (🤭) and the waving emoji (👋) to respond to messages mentioning it. And I added "Zzz..." at the end of all the robot's dialogue to represent its sleepy personality.

![image](https://github.com/JUANMAOV82/ACCT4-APIproject/blob/main/assets/%E6%88%AA%E5%B1%8F2024-02-22%20%E4%B8%8B%E5%8D%886.25.41.png)


# Chatbot (interact) PROJECT
In the follow-up, I added a "wakeup service" to my chatbot, and only a certain option can wake it up.
#code
  ```py
import discord
from discord.ui import View, Button, Modal, TextInput
import os


class Node:

  def __init__(self, value, answer="", children=[]):
    self.value = value
    self.answer = answer
    self.children = children
    #setup the Node


node1 = Node('tired', answer='You good?')
node2 = Node('good night', answer='Nothing')
node3 = Node('Zzz....huh?', answer='Yes', children=[node1, node2])
node4 = Node('Zzzzz...', answer='No')
node5 = Node('just let me sleep.......zz', answer="Maybe")
root = Node('Zzzzzzz......me?', children=[node3, node4, node5])


class GuessOptionView(View):

  def __init__(self, node):
    super().__init__()
    for child in node.children:
      self.add_item(GuessButton(child))
    #setup the GuessOptionsView

  async def handleButtonPress(self, interaction, node):
    if (node.children == []):
      await interaction.response.send_message(
        content=f'Why did you wake me up?......{node.value}',
        view=WrongView(node))
    else:
      await interaction.response.send_message(content=node.value,
                                              view=GuessOptionView(node))
    #what should happen when a button is pressed?


class GuessButton(Button):

  def __init__(self, node):
    super().__init__(label=node.answer)
    self.node = node
    #setup the GuessButton

  async def callback(self, interaction):
    await self.view.handleButtonPress(interaction, self.node)
    #what happens when this button is pressed


class WrongView(View):

  def __init__(self, node):
    super().__init__()
    self.node = node

  @discord.ui.button(label="HEY!!!")
  async def buttonCallback(self, interaction, button):
    await interaction.response.send_modal(FeedbackModal(self.node))


class FeedbackModal(Modal):

  def __init__(self, node):
    super().__init__(title="Feedback")
    self.newAnimal = TextInput(label="What were you thinking of?")
    self.newQuestion = TextInput(label="Sleeping.... next time plz")
    self.oldAnswer = TextInput(label="What answer get to the guess?")
    self.newAnswer = TextInput(label="Zzz?")
    self.add_item(self.newAnimal)
    self.add_item(self.newQuestion)
    self.add_item(self.oldAnswer)
    self.add_item(self.newAnswer)
    self.node = node


async def on_submit(self, interaction):
  newNode1 = Node(self.node.value, answer=self.oldAnswer.value)
  newNode2 = Node(self.newAnimal.value, answer=self.newAnswer.value)
  self.node.value = self.newQuestion.value
  self.node.children = [newNode1, newNode2]
  await interaction.response.send_message(
    content='Thanks for updating......zzzZZZZZZZ {self.node.value}')


intents = discord.Intents.default()
intents.message_content = True

client = discord.Client(intents=intents)


@client.event
async def on_ready():
  print(f'We have logged in as {client.user}')


@client.event
async def on_message(message):
  if message.author == client.user:
    return

  if message.content.startswith('$ARE YOU THERE?'):
    await message.channel.send(content=root.value, view=GuessOptionView(root))


token = os.getenv("DISCORD_BOT_SECRET")
client.run(token)

```
<img width="323" alt="截屏2024-04-18 下午8 07 14" src="https://github.com/JUANMAOV82/ACCT4-APIproject/assets/113642935/38469ea2-aff4-40e7-a656-8918c9db7701">
In the first part, you can wake up the bot by asking, and answer the bot's questions.
<img width="442" alt="截屏2024-04-18 下午8 20 07" src="https://github.com/JUANMAOV82/ACCT4-APIproject/assets/113642935/e4136607-c87a-4528-bf92-984cecf99c9d">
If the wake-up fails, the bot will fall asleep again, but some notes can be left for it.
<img width="434" alt="截屏2024-04-18 下午8 19 59" src="https://github.com/JUANMAOV82/ACCT4-APIproject/assets/113642935/e7ae1c73-91fd-420f-915e-02427e79d6f1">


