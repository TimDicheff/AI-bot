import discord
from discord.ext import commands
from keras.models import load_model  # TensorFlow is required for Keras to work
from PIL import Image, ImageOps  # Install pillow instead of PIL
import numpy as np

intents = discord.Intents.default()
intents.message_content = True

bot = commands.Bot(command_prefix='$', intents=intents)

@bot.command()
async def start(ctx):
  await ctx.send(f'Привет! Я специальный бот-помощник, который поможет тебе с правильной сортировкой мусора. У меня есть несколько функций. Напиши $help_, чтобы узнать о них.')

@bot.command()
async def help_(ctx):
  await ctx.send(f'Напиши $info для того, чтобы я рассказал тебе какие виды перерабатываемого пластика бывают. Напиши $photo, чтобы я показал тебе, где значок переработки можно найти на упаковке чипсов')

@bot.command()
async def info(ctx):
  with open('images/info.jpeg', 'rb') as f:
        picture = discord.File(f)
  await ctx.send(file=picture)

@bot.command()
async def photo(ctx):
  with open('photos/photo.jpg', 'rb') as f:
        picture = discord.File(f)
  await ctx.send(file=picture)

@bot.command()
async def main(ctx, n:int):
  if message.content <= 10:
    await message.channel.send(f'Это пластик')
  if message.content > 10 and message.content <= 20:
    await message.channel.send(f'Это cтекло')
  if message.content > 20 and message.content <= 30:
    await message.channel.send(f'Это металл')  

def get_class(model_path, labels_path, image_path):
    np.set_printoptions(suppress=True)
    model = load_model(model_path, compile=False)
    class_names = open(labels_path, "r", encoding="utf-8").readlines()
    data = np.ndarray(shape=(1, 224, 224, 3), dtype=np.float32)
    image = Image.open(image_path).convert("RGB")

    size = (224, 224)
    image = ImageOps.fit(image, size, Image.Resampling.LANCZOS)

    image_array = np.asarray(image)

    normalized_image_array = (image_array.astype(np.float32) / 127.5) - 1

    data[0] = normalized_image_array

    prediction = model.predict(data)
    index = np.argmax(prediction)
    class_name = class_names[index]
    confidence_score = prediction[0][index]

    return(class_name[2:], confidence_score)    

@bot.command()
async def check(ctx):
  if ctx.message.attachments:
    for attachment in ctx.message.attachments:
        file_name = attachment.filename
        file_url = attachment.url
        await attachment.save(f'images/{attachment.filename}')
        await ctx.send(get_class(model_path='keras_model.h5', labels_path='labels.txt', image_path=f'images/{attachment.filename}'))
         
  else:
    await ctx.send("Вы забыли картинку")

bot.run('TOKEN')
