
import telebot
import json
import random

token = "7478488153:AAHVxXH3Xn6mpQ1ID3l-JaKyrkuOp_A2ZyA"

bot = telebot.TeleBot(token)

try:
    with open("user_data.json", "r", encoding="utf-8") as file:
        user_data = json.load(file)

except FileNotFoundError:
    user_data = {}


@bot.message_handler(commands=["start"])
def handler_start(message):
    bot.send_message(message.chat.id, "Привет")


@bot.message_handler(commands=["addword"])
def handler_addword(message):
    global user_data
    global word
    global chat_id
    chat_id = message.chat.id
    user_dict = user_data.get(chat_id, {})

    words = message.text.split()[1:]
    if len(words) == 2:
        word = words[0].lower()
        translation = words[1].lower()
        user_dict[word] = translation
        user_data[chat_id] = user_dict
        with open("user_data.json", "w", encoding="utf-8", ) as file:
            json.dump(user_data, file, ensure_ascii=False, indent=4)
        bot.send_message(message.chat.id, "Слово добавлено")


    else:
        bot.send_message(message.chat.id, "Произошла ошибка. попробуйте ещё раз")


@bot.message_handler(commands=["learn"])
def handler_learn(message):
    user_words = user_data.get(str(message.chat.id), {})
    try:
        words_number = int(message.text.split()[1])

    except:
        bot.send_message(message.chat.id, "Вы ввели неправльное количество слов или не ввели вовсе. образец команды: /learn 1 ")
        words_number = 1

    ask_translation(message.chat.id, user_words, words_number)


def ask_translation(chat_id, user_words, words_left):
    if words_left > 0:
        word = random.choice(list(user_words.keys()))
        translation = user_words[word]
        bot.send_message(chat_id, f"Напиши перевод слова {word}")

        words_left -=1
        bot.register_next_step_handler_by_chat_id(chat_id, check_translation, translation, words_left)





    else:
        bot.send_message(chat_id, "слова кончились")


def check_translation(message, translation, words_left):
    user_translation = message.text.strip().lower()
    if user_translation == translation.lower():
        bot.send_message(message.chat.id, "Правильно")

    else:
        bot.send_message(message.chat.id, f"неправильно, правильный перевод - {translation}")

    ask_translation(message.chat.id, user_data[str(message.chat.id)], words_left)


@bot.message_handler(func=lambda message: True)
def handler_all(message):
    if message.text.lower() == "как тебя зовут" or message.text.lower() == "как тебя зовут?":
        bot.send_message(message.chat.id, "пока нет имени.")

    elif message.text.lower() == "как дела":
        bot.send_message(message.chat.id, "хорошо.")

    elif message.text.lower() == "расскажи о себе":
        bot.send_message(message.chat.id, "Я бот для помощи в изучении английского языка")

    else:
        bot.send_message(message.chat.id, "я пока не понимаю что ты мне говоришь")


if __name__ == "__main__":
    bot.polling(none_stop=True)
