[users.txt.txt](https://github.com/SalamandraLTD/ubiquitous-rotary-phone/files/15438814/users.txt.txt)
from telegram import Bot
from telethon.sync import TelegramClient
from telethon.tl.functions.channels import InviteToChannelRequest

API_ID = "26857372"
API_HASH = "4b7bb8278ade1daaab424563b5b2b789"
PHONE_NUMBER = "+79185744215"
CHANNEL_USERNAME = "@secretumo"
BOT_TOKEN = "6797002864:AAH5F4qxU-v7ICugrjylBGpQqXkWTXJakmI"

# Функция для инвайта пользователей
def invite_users(users, channel_username):
    with TelegramClient('session_name', API_ID, API_HASH) as client:
        for user in users:
            client(InviteToChannelRequest(channel_username, [user]))

# Функция для получения пользователей из файла
def get_users_from_file(file_path):
    with open(file_path, "r") as file:
        users = [line.strip() for line in file]
    return users

# Функция для обработки команды start
def start(update, context):
    chat_id = update.effective_chat.id
    users = get_users_from_file("users.txt")
    invite_users(users, CHANNEL_USERNAME)
    context.bot.send_message(chat_id, "Пользователи были успешно приглашены в канал")

def main():
    bot = Bot(token=BOT_TOKEN)
    updater = Updater(bot=bot, use_context=True)
    dp = updater.dispatcher

    dp.add_handler(CommandHandler("start", start))

    updater.start_polling()
    updater.idle()

if __name__ == "__main__":
    main()
