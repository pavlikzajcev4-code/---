# ---import telebot
import time
import threading


API_TOKEN = "Ваш Токен от @BotFather"
bot = telebot.TeleBot(API_TOKEN)

reminders = {}

def reminder_thread(chat_id, message, delay):
    time.sleep(delay)
    bot.send_message(chat_id, message)

@bot.message_handler(commands=["start"])
def send_welcome(message):
    bot.reply_to(message, "Привет, я-бот напоминалка! Пиши /remind для установки напоминания")

@bot.message_handler(commands=['remind'])
def set_reminder(message):
    try:
        parts = message.text.split(maxsplit=2)
        if len(parts) < 3:
            bot.reply_to(message, "Пожалуйста укажите время в секундах и сообщение")
            return
        
        delay = int(parts[1])
        reminder_message = parts[2]
        chat_id = message.chat.id
        
        bot.reply_to(message, f"Напоминание установлено на {delay} секунд")
       
        threading.Thread(target=reminder_thread, args=(chat_id, reminder_message, delay)).start()
        
    except ValueError:
        bot.reply_to(message, "Пожалуйста, укажите корректное время в секундах")

if name == 'main':
    bot.polling(non_stop=True)
