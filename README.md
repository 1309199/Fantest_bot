from aiogram import Bot, Dispatcher, types
from aiogram.types import InlineKeyboardMarkup, InlineKeyboardButton
from aiogram.utils import executor
import os

# Telegram tokenini environment variable orqali olamiz
TOKEN = os.getenv("BOT_TOKEN")
bot = Bot(token=TOKEN)
dp = Dispatcher(bot)

# Start menyusi
def start_menu():
    keyboard = InlineKeyboardMarkup(row_width=1)
    keyboard.add(
        InlineKeyboardButton("📚 Maktab testlari (5–11)", callback_data="school_tests"),
        InlineKeyboardButton("🎓 Abituriyent testi", callback_data="dtm_tests"),
        InlineKeyboardButton("📊 Natijalarim", callback_data="results"),
        InlineKeyboardButton("💎 Premium", callback_data="premium"),
        InlineKeyboardButton("🍩 Donat rivojimiz uchun", callback_data="donate")
    )
    return keyboard

# /start komandasi
@dp.message_handler(commands=['start'])
async def send_welcome(message: types.Message):
    await message.answer(
        "👋 Xush kelibsiz, FanTest_bot’ga xush kelibsiz!\nQuyidagilardan birini tanlang:",
        reply_markup=start_menu()
    )

# Callback tugmalar handler
@dp.callback_query_handler(lambda c: True)
async def process_callback(callback_query: types.CallbackQuery):
    await bot.answer_callback_query(callback_query.id)
    if callback_query.data == "school_tests":
        await bot.send_message(callback_query.from_user.id, "📚 Maktab testlari boshlanadi… (misol)")
    elif callback_query.data == "dtm_tests":
        await bot.send_message(callback_query.from_user.id, "🎓 Abituriyent testi boshlanadi… (misol)")
    elif callback_query.data == "results":
        await bot.send_message(callback_query.from_user.id, "📊 Natijalar… (misol)")
    elif callback_query.data == "premium":
        await bot.send_message(callback_query.from_user.id, "💎 Premium sahifa… (misol)")
    elif callback_query.data == "donate":
        await bot.send_message(callback_query.from_user.id, "🍩 Donat sahifasi… (misol)")

# Botni ishga tushurish
if __name__ == '__main__':
    executor.start_polling(dp, skip_updates=True)
