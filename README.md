# earnexa-bot
import telebot
from telebot import types

BOT_TOKEN = "8455360316:AAHcOpzqbLf137vSZQFeXLKXmXHUm2BqrBs"
ADMIN_ID = 123456789  # 6676785749
BOT_USERNAME = "earnexa_bot"

bot = telebot.TeleBot(BOT_TOKEN)
users = {}

def menu():
    m = types.ReplyKeyboardMarkup(resize_keyboard=True)
    m.add("💼 Earn", "👥 Invite")
    m.add("💰 Balance", "🏧 Withdraw")
    return m

@bot.message_handler(commands=['start'])
def start(message):
    uid = message.chat.id
    users.setdefault(uid, 0)
    ref = f"https://t.me/{BOT_USERNAME}?start={uid}"
    bot.send_message(
        uid,
        f"👋 Welcome to Earnexa!\n\n💰 Earn by tasks\n👥 Invite friends\n\n🔗 Referral Link:\n{ref}",
        reply_markup=menu()
    )

@bot.message_handler(func=lambda m: m.text == "💼 Earn")
def earn(m):
    bot.send_message(m.chat.id, "📝 Task:\n👉 https://example.com\n\n✅ Complete & return")

@bot.message_handler(func=lambda m: m.text == "👥 Invite")
def invite(m):
    uid = message.chat.id
    bot.send_message(uid, f"👥 Invite link:\nhttps://t.me/{BOT_USERNAME}?start={uid}")

@bot.message_handler(func=lambda m: m.text == "💰 Balance")
def balance(m):
    bot.send_message(m.chat.id, f"💳 Balance: {users.get(m.chat.id,0)} Points")

@bot.message_handler(func=lambda m: m.text == "🏧 Withdraw")
def withdraw(m):
    bot.send_message(m.chat.id, "📨 Withdraw request sent!")
    bot.send_message(ADMIN_ID, f"Withdraw request from {m.chat.id}")

bot.polling()
