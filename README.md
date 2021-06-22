# Пишем Telegram-ботов на Python (v3.9) 

В NotePad создаем и сразу же сохраняем фаил в пустую папку с именем на анг языке без пробелов и с расширением .py
примнр cicada_3301.py
это нужно для того чтобы Notepad понимал с каким языком мы рвботаем и помогал тыделять текст 

# Теперь теория ОБЯЗАТЕЛЬНО !!!!!!

1 Переменая - это обьект который хранит в себе какое либо значение и оно может быть изменено во время исполнения кода
    переменая не может начинаться с цифр !!!! только с текста  тоесть 111Вася - так нельзя будет ошибка а Вася111 - так можно 

# Пример

name = 'Гусь' 

тоесть в коде если написать команду вывести текст 

print(name)
Будет ответ Гусь

# Библиотеки 

Почти для каждого кода нужны библиотеки это можно сказать набор команд для работы с тем для чего пишим код
код всегда начинаеться с импорта библиотек мы пишем бота для Telegram и будем использовать библиотеку aiogram
команды найдешь тут [aiogram](https://pypi.org/project/aiogram/)

создадим фаил bot.py и напишем следующий код:

    #!venv/bin/python
    import logging
    import asyncio
    from aiogram import Bot, Dispatcher, executor, types
    from aiogram.utils.exceptions import BotBlocked
    from os import getenv
    from sys import exit
    ###################################
    ##  Все что выше это библиотеки  ##
    ###################################
    
    
  

    bot = Bot(token=bot_token)
    dp = Dispatcher(bot)
    logging.basicConfig(level=logging.INFO)


    # Хэндлер на команду /test1
    @dp.message_handler(commands="test1")
    async def cmd_test1(message: types.Message):
        await message.reply("Test 1")


    # Хэндлер на команду /test2
    async def cmd_test2(message: types.Message):
        await message.reply("Test 2")

    # Где-то в другом месте...
    dp.register_message_handler(cmd_test2, commands="test2")


    @dp.message_handler(commands="answer")
    async def cmd_answer(message: types.Message):
        await message.answer("Это простой ответ")


    @dp.message_handler(commands="reply")
    async def cmd_reply(message: types.Message):
        await message.reply('Это ответ с "ответом"')


    @dp.message_handler(commands="dice")
    async def cmd_dice(message: types.Message):
        await message.answer_dice(emoji="🎲")


    @dp.message_handler(commands="block")
    async def cmd_block(message: types.Message):
        await asyncio.sleep(10.0)  # Здоровый сон на 10 секунд
        await message.reply("Вы заблокированы")


    @dp.errors_handler(exception=BotBlocked)
    async def error_bot_blocked(update: types.Update, exception: BotBlocked):
        # Update: объект события от Telegram. Exception: объект исключения
        # Здесь можно как-то обработать блокировку, например, удалить пользователя из БД
        print(f"Меня заблокировал пользователь!\nСообщение: {update}\nОшибка: {exception}")

        # Такой хэндлер должен всегда возвращать True,
        # если дальнейшая обработка не требуется.
        return True


    if __name__ == "__main__":
        # Запускаем бота и пропускаем все накопленые входящие
        executor.start_polling(dp, skip_updates=True)

# Важно !!!

Так как этот код зациклен на выполнения покругу фаил конфигурации я да и все опытные програмисты создаеться отдельным файлом 

создадим фаил config.py 

в верхнем коде есть такие строки 

    bot = Bot(token=bot_token)
    dp = Dispatcher(bot)
    logging.basicConfig(level=logging.INFO)

bot - это код программы передающий команды через библиотеку aiogram где мы импортировали команду Bot у которой в нутри Bot(token) а токен мы задаем в конфиге 

открываем config.py и пишем:

     bot_token = '334278:jhksfjhjdf' # токен
     
далее нам нужно указать коду от куда брать и что для этого в библиотеках допишим 

    from config import bot_token
    
выйдет такой код уже полность готовый и его можно запускать:


      #!venv/bin/python
      import logging
      import asyncio
      from aiogram import Bot, Dispatcher, executor, types
      from aiogram.utils.exceptions import BotBlocked
      from os import getenv
      from sys import exit
      from config import bot_token




      bot = Bot(token=bot_token)
      dp = Dispatcher(bot)
      logging.basicConfig(level=logging.INFO)


      # Хэндлер на команду /test1
      @dp.message_handler(commands="test1")
      async def cmd_test1(message: types.Message):
          await message.reply("Test 1")


      # Хэндлер на команду /test2
      async def cmd_test2(message: types.Message):
          await message.reply("Test 2")

      # Где-то в другом месте...
      dp.register_message_handler(cmd_test2, commands="test2")


      @dp.message_handler(commands="answer")
      async def cmd_answer(message: types.Message):
          await message.answer("Это простой ответ")


      @dp.message_handler(commands="reply")
      async def cmd_reply(message: types.Message):
          await message.reply('Это ответ с "ответом"')


      @dp.message_handler(commands="dice")
      async def cmd_dice(message: types.Message):
          await message.answer_dice(emoji="🎲")


      @dp.message_handler(commands="block")
      async def cmd_block(message: types.Message):
          await asyncio.sleep(10.0)  # Здоровый сон на 10 секунд
          await message.reply("Вы заблокированы")


      @dp.errors_handler(exception=BotBlocked)
      async def error_bot_blocked(update: types.Update, exception: BotBlocked):
          # Update: объект события от Telegram. Exception: объект исключения
          # Здесь можно как-то обработать блокировку, например, удалить пользователя из БД
          print(f"Меня заблокировал пользователь!\nСообщение: {update}\nОшибка: {exception}")

          # Такой хэндлер должен всегда возвращать True,
          # если дальнейшая обработка не требуется.
          return True


      if __name__ == "__main__":
          # Запускаем бота и пропускаем все накопленые входящие
          executor.start_polling(dp, skip_updates=True)
