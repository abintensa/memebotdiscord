# memebotdiscord
import discord
import requests
import json
import os
from dotenv import load_dotenv

load_dotenv()  # Carga el token desde .env
TOKEN = os.getenv("DISCORD_TOKEN")

def get_meme():
    response = requests.get('https://meme-api.com/gimme')
    json_data = json.loads(response.text)
    return json_data["url"]

class MyClient(discord.Client):
    async def on_ready(self):
        print(f'✅ Logged on as {self.user}!')

    async def on_message(self, message):
        if message.author == self.user:
            return

        if message.content.startswith('$hello'):
            await message.channel.send('Hello World!')

        if message.content.startswith('$meme'):
            meme = get_meme()
            await message.channel.send(meme)

intents = discord.Intents.default()
intents.message_content = True

client = MyClient(intents=intents)
client.run(TOKEN) #copy my token in another file for privacity

