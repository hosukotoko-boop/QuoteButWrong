# Проект: [QuoteButWrong]

## 1. Исследование предметной области и последовательность действий
- изучили курсы по созданию телеграм-ботов на языке Phyton
- создание скелета бота в BotFather
- придумали идею (что будет выполнять бот)
- придумали оригинальное название для бота (QuoteButWrong)
- установили библиотеку в PyCharm
- написали основной код для функционала бота

## 2. Техническое руководство по созданию [QuoteButWrong] с нуля (для начинающих)

# Как создать Telegram-бота на Python: пошаговое руководство для новичков

Это руководство поможет вам пройти весь путь от полного нуля до работающего бота. В качестве примера мы создадим бота, который выдаёт "неправильные" цитаты.


## Шаг 1. Изучаем теорию

Прежде чем писать код, важно освоить основы. Мы изучили курсы по созданию Telegram-ботов на языке **Python**. Это дало понимание:

- Как бот общается с серверами Telegram.
- Как обрабатывать команды и сообщения.
- Как использовать библиотеки для работы с Telegram API.

> **Совет:** Начните с бесплатных курсов на YouTube или Stepik — они идеально подходят для старта.


## Шаг 2. Создаём «скелет» бота в BotFather

BotFather — это официальный бот в Telegram для регистрации новых ботов.

1. Найдите в Telegram: **@BotFather**.
2. Отправьте команду: `/newbot`.
3. Придумайте **имя** бота (например, *Мой первый бот*).
4. Придумайте **уникальное имя пользователя**, которое обязательно заканчивается на `bot` (например, *my_first_test_bot*).

После успешного создания BotFather выдаст вам **токен** — это ключ, с помощью которого ваш код будет управлять ботом. Сохраните его!


## Шаг 3. Придумываем идею

Любой хороший бот начинается с идеи. Мы решили, что наш бот будет делать:

> **Идея:** Бот выдаёт цитаты, но с пропуском, и вам надо угадать что будет на месте этого пропуска.

Например:
- "Быть или не ...... вот в чем вопрос"
надо выбрать правельный ответ из предложенных вариантов


## Шаг 4. Даём оригинальное название

Хорошее название запоминается и отражает суть. Мы назвали бота:

**QuoteButWrong**  
*(«Цитата, но неправильная»)*

При желании вы можете придумать своё название, главное — чтобы оно было уникальным в Telegram.


## Шаг 5. Устанавливаем библиотеку в PyCharm

Чтобы Python мог общаться с Telegram, нужна специальная библиотека. Мы работаем в **PyCharm**.

### через терминал
Откройте терминал в PyCharm и выполните команду:

bash
pip install python-telegram-bot

## Шаг 6. Примеры кода нашего бота

import telebot
import random
import string
from telebot import types

# 🔑 Токен
BOT_TOKEN = "токен вашего бота"
bot = telebot.TeleBot(BOT_TOKEN)

# 📚 База цитат
QUOTES = [
    ("Я мыслю, следовательно я существую", "Рене Декарт"),
    ("Быть или не быть, вот в чем вопрос", "Уильям Шекспир"),
    ("Знание — сила", "Фрэнсис Бэкон"),
    ("Время — деньги", "Бенджамин Франклин"),
    ("Я знаю, что ничего не знаю", "Сократ"),
    ("Победа любит подготовку", "А.В. Суворов"),
    ("Учение — свет, а неучение — тьма", "Народная мудрость"),
    ("Делу время, потехе час", "Русская пословица"),
    ("Счастье любит тишину", "Народная мудрость"),
    ("Семь раз отмерь, один раз отрежь", "Русская пословица"),
    ("Всё проходит, и это пройдёт", "Царь Соломон"),
    ("Красота спасёт мир", "Фёдор Достоевский"),
    ("Делай сегодня то, что другие не хотят, завтра будешь жить так, как другие не могут", "Джейсон Стэтхем"),
    ("Самый лучший день для начала новой жизни — сегодня", "Джейсон Стэтхем"),
    ("Если ты упал, вставай, отряхнись и иди дальше", "Джейсон Стэтхем"),
    ("Не бойся медленного движения, бойся только стоять на месте", "Джейсон Стэтхем"),
    ("Успех — это не случайность, а результат ежедневной работы", "Джейсон Стэтхем"),
    ("Настоящая сила — в умении контролировать себя и свои эмоции", "Джейсон Стэтхем"),
    ("Лучше сделать и ошибиться, чем не сделать и пожалеть", "Джейсон Стэтхем"),
    ("Терпение и труд дают плоды, просто не сразу", "Джейсон Стэтхем"),
    ("Слабые ищут причину, сильные ищут возможность", "Джейсон Стэтхем"),
    ("Пока ты думаешь, другие уже делают", "Джейсон Стэтхем"),
]

# 🧠 Хранилище состояний
user_scores = {}  # Chat_ID -> Score (int)
used_quotes = {}  # Chat_ID -> Set of used indices (set)
user_game = {}  # Текущая сессия игры

# 😄 Шуточные сообщения об ошибках
ERROR_MESSAGES = [
    "😅 Почти! Но Шекспир бы поплакал...",
    "🤔 Хм, интересное мнение, но нет!",
    "❌ Мимо! Попробуй ещё раз!",
    "😬 Ой, кажется, кто-то не читал классику!",
    "🙈 Не то слово! Думай лучше!",
    "😵 Цитата в шоке от твоего ответа!",
]

# 🔙 Кнопка назад
back_btn = types.InlineKeyboardButton("🔙 В главное меню", callback_data="menu")


# 🔀 Inline-навигация внутри игры
def get_game_nav():
    markup = types.InlineKeyboardMarkup(row_width=2)
    markup.add(
        types.InlineKeyboardButton("🔀 Сменить цитату", callback_data="skip"),
        types.InlineKeyboardButton("🔙 В главное меню", callback_data="menu")
    )
    return markup


# ⌨️ Reply-клавиатура БЕЗ прогресса (только Новая игра)
def get_reply_keyboard_no_progress():
    markup = types.ReplyKeyboardMarkup(row_width=2, resize_keyboard=True, one_time_keyboard=False)
    markup.add(types.KeyboardButton("🎮 Новая игра"))
    markup.add(types.KeyboardButton("📊 Мой счёт"), types.KeyboardButton("ℹ️ Помощь"))
    return markup


# ⌨️ Reply-клавиатура С прогрессом (Новая игра + Продолжить)
def get_reply_keyboard_with_progress():
    markup = types.ReplyKeyboardMarkup(row_width=2, resize_keyboard=True, one_time_keyboard=False)
    markup.add(types.KeyboardButton("🎮 Новая игра"), types.KeyboardButton("▶️ Продолжить"))
    markup.add(types.KeyboardButton("📊 Мой счёт"), types.KeyboardButton("ℹ️ Помощь"))
    return markup


# 🔍 Проверка наличия прогресса
def has_user_progress(chat_id):
    return (chat_id in user_scores and user_scores[chat_id] > 0) or \
        (chat_id in used_quotes and len(used_quotes[chat_id]) > 0)


# ⌨️ Универсальная функция: возвращает нужную клавиатуру
def get_reply_keyboard(chat_id):
    if has_user_progress(chat_id):
        return get_reply_keyboard_with_progress()
    else:
        return get_reply_keyboard_no_progress()


def get_inline_main_menu(chat_id):
    """Inline-меню для помощи и навигации"""
    markup = types.InlineKeyboardMarkup(row_width=2)

    if has_user_progress(chat_id):
        markup.add(
            types.InlineKeyboardButton("🎮 Новая игра", callback_data="new_game"),
            types.InlineKeyboardButton("▶️ Продолжить", callback_data="continue")
        )
        markup.add(
            types.InlineKeyboardButton("📊 Мой счёт", callback_data="score"),
            types.InlineKeyboardButton("ℹ️ Помощь", callback_data="help")
        )
    else:
        markup.add(
            types.InlineKeyboardButton("🎮 Новая игра", callback_data="new_game")
        )
        markup.add(
            types.InlineKeyboardButton("📊 Мой счёт", callback_data="score"),
            types.InlineKeyboardButton("ℹ️ Помощь", callback_data="help")
        )

    return markup


def get_help_inline_menu(chat_id):
    """Меню для раздела Помощь"""
    markup = types.InlineKeyboardMarkup(row_width=2)

    if has_user_progress(chat_id):
        markup.add(
            types.InlineKeyboardButton("🎮 Новая игра", callback_data="new_game"),
            types.InlineKeyboardButton("▶️ Продолжить", callback_data="continue")
        )
    else:
        markup.add(
            types.InlineKeyboardButton("🎮 Новая игра", callback_data="new_game")
        )

    return markup


# 🧹 Очистка слова от знаков препинания
def clean_word(word):
    return word.strip(string.punctuation + "—–«»'\"() ").lower()


# 🚀 Старт — КРАСИВОЕ ПРИВЕТСТВИЕ
@bot.message_handler(commands=['start'])
def send_welcome(message):
    welcome_text = (
        "🎮 **Добро пожаловать в QuoteButWrong!**\n\n"
        "🧠 **Проверь свои знания великих цитат!**\n\n"
        "📝 **Как это работает:**\n"
        "• Я покажу цитату с пропущенным словом\n"
        "• Ты угадаешь, какое слово скрыто\n"
        "• Собери 10 очков и стань чемпионом!\n\n"
        "🏆 **Особенности:**\n"
        "• Цитаты от Декарта до Джейсона Стэтхема\n"
        "• Прогресс сохраняется\n"
        "• Шуточные подсказки при ошибках\n\n"
        "🎯 **Жми '🎮 Новая игра' и поехали!**"
    )
    # Отправляем клавиатуру в зависимости от прогресса
    bot.reply_to(message, welcome_text, parse_mode="Markdown", reply_markup=get_reply_keyboard(message.chat.id))


# ⌨️ Обработчик кнопок Reply Keyboard (над строкой ввода)
@bot.message_handler(func=lambda message: message.text in ["🎮 Новая игра", "▶️ Продолжить", "📊 Мой счёт", "ℹ️ Помощь"])
def handle_reply_buttons(message):
    chat_id = message.chat.id

    if message.text == "🎮 Новая игра":
        user_scores[chat_id] = 0
        used_quotes[chat_id] = set()
        bot.send_message(chat_id, "🔄 Прогресс сброшен! Начинаем заново.",
                         reply_markup=get_reply_keyboard_with_progress(chat_id))
        start_quiz(chat_id, None)

    elif message.text == "▶️ Продолжить":
        if chat_id not in user_scores:
            user_scores[chat_id] = 0
        if chat_id not in used_quotes:
            used_quotes[chat_id] = set()
        bot.send_message(chat_id, "▶️ Продолжаем игру!",
                         reply_markup=get_reply_keyboard_with_progress(chat_id))
        start_quiz(chat_id, None)

    elif message.text == "📊 Мой счёт":
        score = user_scores.get(chat_id, 0)
        bot.send_message(chat_id, f"📊 Твой счёт: {score} очков!",
                         reply_markup=get_reply_keyboard(chat_id))

    elif message.text == "ℹ️ Помощь":
        help_text = (
            "📚 **Как играть:**\n\n"
            "1. Я покажу цитату с пропущенным словом (......)\n"
            "2. Ты увидишь 4 варианта ответа\n"
            "3. Выбери правильное слово!\n"
            "4. Если ошибёшься — попробуешь ещё раз (-1 балл)\n"
            "5. За правильный ответ +1 балл\n"
            "6. **Цель:** Набрать 10 очков! 🏆\n\n"
            "🔹 'Новая игра' — сброс очков и цитат\n"
            "🔹 'Продолжить' — сохранение прогресса"
        )
        bot.send_message(chat_id, help_text, parse_mode="Markdown",
                         reply_markup=get_reply_keyboard(chat_id))


# 🔘 Обработчик Inline-кнопок
@bot.callback_query_handler(func=lambda call: True)
def callback_handler(call):
    chat_id = call.message.chat.id

    if call.data == "new_game":
        user_scores[chat_id] = 0
        used_quotes[chat_id] = set()
        bot.answer_callback_query(call.id, "🔄 Прогресс сброшен! Начинаем заново.")
        start_quiz(chat_id, call.message.message_id)

    elif call.data == "continue":
        if chat_id not in user_scores:
            user_scores[chat_id] = 0
        if chat_id not in used_quotes:
            used_quotes[chat_id] = set()
        start_quiz(chat_id, call.message.message_id)

    elif call.data == "score":
        score = user_scores.get(chat_id, 0)
        bot.answer_callback_query(call.id, f"📊 Твой счёт: {score} очков!")

    elif call.data == "help":
        help_text = (
            "📚 **Как играть:**\n\n"
            "1. Я покажу цитату с пропущенным словом (......)\n"
            "2. Ты увидишь 4 варианта ответа\n"
            "3. Выбери правильное слово!\n"
            "4. Если ошибёшься — попробуешь ещё раз (-1 балл)\n"
            "5. За правильный ответ +1 балл\n"
            "6. **Цель:** Набрать 10 очков! 🏆\n\n"
            "🔹 'Новая игра' — сброс очков и цитат\n"
            "🔹 'Продолжить' — сохранение прогресса"
        )
        bot.send_message(chat_id, help_text, parse_mode="Markdown", reply_markup=get_help_inline_menu(chat_id))

    elif call.data == "menu":
        bot.edit_message_text("🎮 Главное меню:", chat_id, call.message.message_id,
                              reply_markup=get_inline_main_menu(chat_id))

    elif call.data == "skip":
        start_quiz(chat_id, call.message.message_id)

    elif call.data.startswith("answer_"):
        check_answer(chat_id, call.data, call.message.message_id)


# 🎮 Логика викторины
def start_quiz(chat_id, msg_id):
    all_indices = set(range(len(QUOTES)))
    used = used_quotes.get(chat_id, set())
    available_indices = list(all_indices - used)

    if not available_indices:
        final_msg = (
            f"🎉 **Вы прошли все цитаты!**\n\n"
            f"Ваш итоговый счёт: **{user_scores.get(chat_id, 0)}**\n"
            f"Нажмите '🎮 Новая игра', чтобы начать заново."
        )
        if msg_id:
            bot.edit_message_text(final_msg, chat_id, msg_id, parse_mode="Markdown",
                                  reply_markup=get_inline_main_menu(chat_id))
        else:
            bot.send_message(chat_id, final_msg, parse_mode="Markdown",
                             reply_markup=get_reply_keyboard_with_progress(chat_id))
        return

    quote_idx = random.choice(available_indices)
    used_quotes[chat_id].add(quote_idx)

    correct_quote, author = QUOTES[quote_idx]
    words = correct_quote.split()

    suitable_words = [(i, w) for i, w in enumerate(words) if len(clean_word(w)) > 3]
    if not suitable_words:
        suitable_words = list(enumerate(words))

    idx, original_word = random.choice(suitable_words)
    clean_correct = clean_word(original_word)

    words_with_blank = []
    for i, word in enumerate(words):
        if i == idx:
            words_with_blank.append("......")
        else:
            words_with_blank.append(word)
    quote_with_blank = " ".join(words_with_blank)

    wrong_clean = get_wrong_words(clean_correct, 3)
    all_clean = wrong_clean + [clean_correct]
    random.shuffle(all_clean)

    final_options = []
    is_first_word = (idx == 0)

    for word in all_clean:
        if is_first_word:
            display_text = word.capitalize()
        else:
            display_text = word.lower()
        final_options.append((display_text, word))

    random.shuffle(final_options)

    user_game[chat_id] = {
        "correct_word": clean_correct,
        "quote": correct_quote,
        "blank_quote": quote_with_blank,
        "author": author,
        "options": final_options
    }

    keyboard = types.InlineKeyboardMarkup(row_width=2)
    for display_w, clean_w in final_options:
        keyboard.add(types.InlineKeyboardButton(display_w, callback_data=f"answer_{clean_w}"))

    nav = get_game_nav()
    for row in nav.keyboard:
        for btn in row:
            keyboard.add(btn)

    text = f"📝 **Цитата:**\n\"{quote_with_blank}\"\n\n👤 {author}\n\n❓ Какое слово пропущено?"

    if msg_id:
        try:
            bot.edit_message_text(text, chat_id, msg_id, parse_mode="Markdown", reply_markup=keyboard)
        except:
            bot.send_message(chat_id, text, parse_mode="Markdown", reply_markup=keyboard)
    else:
        bot.send_message(chat_id, text, parse_mode="Markdown", reply_markup=keyboard)


# 🔀 Получение неправильных слов
def get_wrong_words(correct_word, count):
    all_words = set()
    for quote, _ in QUOTES:
        for w in quote.split():
            cleaned = clean_word(w)
            if cleaned:
                all_words.add(cleaned)

    all_words.discard(correct_word)
    suitable = [w for w in all_words if abs(len(w) - len(correct_word)) <= 3]

    if len(suitable) >= count:
        return random.sample(suitable, count)
    else:
        fallback = ["слово", "время", "дело", "жизнь", "ум", "сила"]
        return list(suitable) + fallback[:count - len(suitable)]


# ✅ Проверка ответа
def check_answer(chat_id, callback_data, msg_id):
    if chat_id not in user_game:
        return

    game = user_game[chat_id]
    selected_clean = callback_data.replace("answer_", "").lower()
    correct = game["correct_word"]

    if selected_clean == correct:
        user_scores[chat_id] = user_scores.get(chat_id, 0) + 1
        score = user_scores[chat_id]

        if score == 10:
            victory_text = (
                f"🏆 **ПОБЕДА!**\n\n"
                f"Ты набрал **10 очков**!\n"
                f"Поздравляем! 🥳\n\n"
                f"Можешь продолжить играть на результат!"
            )
            bot.send_message(chat_id, victory_text, parse_mode="Markdown")

        success_text = (
            f"🎉 **Правильно!**\n\n"
            f"✨ \"{game['quote']}\"\n\n"
            f"📊 Твой счёт: **{score}** очков!\n\n"
            f"Что делаем дальше?"
        )
        # После правильного ответа показываем клавиатуру с кнопкой "Продолжить"
        bot.send_message(chat_id, success_text, parse_mode="Markdown",
                         reply_markup=get_reply_keyboard_with_progress(chat_id))

    else:
        current_score = user_scores.get(chat_id, 0)
        user_scores[chat_id] = max(0, current_score - 1)
        new_score = user_scores[chat_id]

        error_msg = random.choice(ERROR_MESSAGES)

        bot.send_message(chat_id,
                         f"{error_msg}\n\n"
                         f"📊 Твой счёт: {new_score} очков\n\n"
                         f"Попробуй ещё раз!",
                         parse_mode="Markdown"
                         )

        options = game["options"]
        keyboard = types.InlineKeyboardMarkup(row_width=2)
        for display_w, clean_w in options:
            keyboard.add(types.InlineKeyboardButton(display_w, callback_data=f"answer_{clean_w}"))

        nav = get_game_nav()
        for row in nav.keyboard:
            for btn in row:
                keyboard.add(btn)

        text = f"📝 **Цитата:**\n\"{game['blank_quote']}\"\n\n👤 {game['author']}\n\n❓ Какое слово пропущено?"
        bot.send_message(chat_id, text, parse_mode="Markdown", reply_markup=keyboard)


# 🚀 Запуск
if __name__ == "__main__":
    print("🤖 QuoteButWrong запущен...")
    bot.infinity_polling()
