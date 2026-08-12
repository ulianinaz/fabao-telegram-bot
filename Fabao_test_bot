import telebot
from telebot import types
user_data = {}
def get_user(user_id):
    if user_id not in user_data:
        user_data[user_id] = {"ответы": {},"баллы": {
                "101G": 0,
                "101B": 0,
                "101F": 0 }}
        return user_data[user_id]

bot = telebot.TeleBot ('токен')

@bot.message_handler(commands=['start'])
def start(message):
    markup = types.InlineKeyboardMarkup()
    markup.add(types.InlineKeyboardButton("Начать тест 🧪", callback_data="start_test"))
    bot.send_message(message.chat.id, '<b>👋 Привет! Я бот-помощник Fabao</b> \n Я помогу тебе разобраться, какое средство лучше подходит для твоих волос. \n Ответь на несколько вопросов - и я подберу вариант, который лучше всего соответствует твоей ситуации. \n Готов начать? Нажми на кнопку ниже 👇', reply_markup=markup, parse_mode = 'html')

@bot.callback_query_handler(func=lambda call: call.data == "start_test")
def start_test (call):
    bot.answer_callback_query(call.id)
    markup = types.InlineKeyboardMarkup()
    markup.add(types.InlineKeyboardButton("мужской", callback_data="gender_male"))
    markup.add(types.InlineKeyboardButton("женский", callback_data="gender_female"))
    bot.send_message(call.message.chat.id, 'Чтобы подобрать тоник точнее, скажите, пожалуйста, ваш пол:',reply_markup=markup)
    
    
@bot.callback_query_handler(func=lambda call: call.data in ["gender_male", "gender_female"])
def handle_gender (call):
    bot.answer_callback_query(call.id)
    user_id = call.from_user.id
    user = get_user(user_id)
    if call.data == 'gender_male':
        user_data[user_id]["ответы"]["пол"] = "Мужской"
        bot.edit_message_text(f"1. Ваш пол: ✅ {user_data[user_id]['ответы']['пол']}", call.message.chat.id, call.message.message_id, reply_markup=None)
        send_male_duration_question(call.message.chat.id)
    else:
        user_data[user_id]["ответы"]["пол"] = "Женский"
        bot.edit_message_text(f"1. Ваш пол: ✅ {user_data[user_id]['ответы']['пол']}", call.message.chat.id, call.message.message_id, reply_markup=None)
        send_female_oiliness_question(call.message.chat.id)   
        
def send_male_duration_question(chat_id):   
    markup = types.InlineKeyboardMarkup()
    markup.add(types.InlineKeyboardButton("Менее 1 года", callback_data="loss_less_year"))
    markup.add(types.InlineKeyboardButton("1-3 года", callback_data="loss_1_3_years"))
    markup.add(types.InlineKeyboardButton("больше 3 лет", callback_data="loss_more_3_years"))
    bot.send_message(chat_id, "Как давно вы заметили выпадение или поредение волос?",reply_markup=markup)
        
@bot.callback_query_handler(func=lambda call: call.data in ["loss_less_year", "loss_1_3_years", "loss_more_3_years"])
def male_duration_question(call):
    bot.answer_callback_query(call.id)
    user_id = call.from_user.id
    user = get_user(user_id)
        
    if call.data == "loss_less_year":
        user_data[user_id]["ответы"]["давность"] = "Менее 1 года"
        user_data[user_id]["баллы"]["101B"] += 2
    elif call.data == "loss_1_3_years":
        user_data[user_id]["ответы"]["давность"] = "1-3 года"
        user_data[user_id]["баллы"]["101G"] += 2
    else:
        user_data[user_id]["ответы"]["давность"] = "больше 3 лет"
        user_data[user_id]["баллы"]["101F"] += 1
    bot.edit_message_text(f"Как давно вы заметили выпадение или поредение волос?: ✅ {user_data[user_id]['ответы']['давность']}", call.message.chat.id, call.message.message_id, reply_markup=None)
    heredity_send_male(call.message.chat.id)
    
    
def heredity_send_male (chat_id):
    markup = types.InlineKeyboardMarkup()
    markup.add(types.InlineKeyboardButton("Равномерно по всей голове", callback_data="all_over_the_head_male"))
    markup.add(types.InlineKeyboardButton("На макушке и всиках", callback_data="on_the_crown_male"))
    markup.add(types.InlineKeyboardButton("Круглые очаги", callback_data="circular_hearths_male"))
    bot.send_message(chat_id, "Как проявляется выпадение?", reply_markup=markup)
            
@bot.callback_query_handler(func=lambda call: call.data in ["all_over_the_head_male", "on_the_crown_male","circular_hearths_male"])
def loss_area (call):
    bot.answer_callback_query(call.id)
    user_id = call.from_user.id
    user = get_user(user_id)      
    if call.data == "all_over_the_head_male":
        user_data[user_id]["ответы"]["место"] = "Равномерно по всей голове"
        user_data[user_id]["баллы"]["101B"] += 2
    elif call.data == "on_the_crown_male":
        user_data[user_id]["ответы"]["место"] = "На макушке и всиках"
        user_data[user_id]["баллы"]["101G"] += 3
    else:
        user_data[user_id]["ответы"]["место"] = "Круглые очаги"
        user_data[user_id]["баллы"]["101F"] += 3 
    bot.edit_message_text(f"Как проявляется выпадение?: ✅ {user_data[user_id]['ответы']['место']}", call.message.chat.id, call.message.message_id, reply_markup=None)
    send_male_heredity(call.message.chat.id)
    
def send_male_heredity (chat_id) :   
    markup = types.InlineKeyboardMarkup()
    markup.add(types.InlineKeyboardButton("Да", callback_data="Yes_male"))
    markup.add(types.InlineKeyboardButton("Нет", callback_data="No_male"))
    bot.send_message(chat_id, "Есть ли у отца/деда облысение?", reply_markup=markup)
            
@bot.callback_query_handler(func=lambda call: call.data in ["Yes_male", "No_male"])
def heredity (call):
    bot.answer_callback_query(call.id)
    user_id = call.from_user.id
    user = get_user(user_id) 
    if call.data == "Yes_male":
        user_data[user_id]["ответы"]["наследственность"] = "Да"
        user_data[user_id]["баллы"]["101G"] += 3
    else:
        user_data[user_id]["ответы"]["наследственность"] = "Нет"
        user_data[user_id]["баллы"]["101B"] += 2
    bot.edit_message_text(f"Есть ли у отца/деда облысение?: ✅ {user_data[user_id]['ответы']['наследственность']}", call.message.chat.id, call.message.message_id, reply_markup=None)
    send_male_oiliness(call.message.chat.id)
    
def send_male_oiliness(chat_id):
    markup = types.InlineKeyboardMarkup()
    markup.add(types.InlineKeyboardButton("За 1 день", callback_data="1_day_male"))
    markup.add(types.InlineKeyboardButton("Через 2–3 дня", callback_data="less_3_days_male"))
    markup.add(types.InlineKeyboardButton("Могу не мыть 4+ дней", callback_data="More_4_days_male"))
    bot.send_message(chat_id, "Как быстро после мытья волосы у корней теряют свежесть и становятся жирными на вид?", reply_markup=markup)
            
@bot.callback_query_handler (func=lambda call: call.data in ["1_day_male", "less_3_days_male", "More_4_days_male"])
def oiliness (call):
    bot.answer_callback_query(call.id)
    user_id = call.from_user.id
    user = get_user(user_id) 
    if call.data == "1_day_male":
        user_data[user_id]["ответы"]["загрязнение"] = "За 1 день"
        user_data[user_id]["баллы"]["101G"] += 2
    elif call.data == "less_3_days_male":
        user_data[user_id]["ответы"]["загрязнение"] = "Через 2–3 дня"
        user_data[user_id]["баллы"]["101B"] += 2
    else:
        user_data[user_id]["ответы"]["загрязнение"] = "Могу не мыть 4+ дней"
        user_data[user_id]["баллы"]["101F"] += 2
    bot.edit_message_text(f"Как быстро жирнеют волосы?: ✅ {user_data[user_id]['ответы']['загрязнение']}", call.message.chat.id, call.message.message_id, reply_markup=None)
    send_male_minoxidil(call.message.chat.id) 
     
def send_male_minoxidil(chat_id):                            
    markup = types.InlineKeyboardMarkup()
    markup.add(types.InlineKeyboardButton("Да, не помогло", callback_data="minoxidil_no_help"))
    markup.add(types.InlineKeyboardButton("Да, помогло, но бросил", callback_data="minoxidil_helped"))
    markup.add(types.InlineKeyboardButton("Нет", callback_data="minoxidil_not_used"))
    bot.send_message(chat_id, " Пробовали миноксидил/финастерид?", reply_markup=markup)
        
@bot.callback_query_handler(func=lambda call: call.data in["minoxidil_no_help", "minoxidil_helped", "minoxidil_not_used"])
def minoxidil(call):
    bot.answer_callback_query(call.id)
    user_id = call.from_user.id
    user = get_user(user_id)
    if call.data == "minoxidil_no_help":
        user_data[user_id]["ответы"]["миноксидил"] = "Да, не помогло"
        user_data[user_id]["баллы"]["101G"] += 2
    elif call.data == "minoxidil_helped":
        user_data[user_id]["ответы"]["миноксидил"] = "Да, помогло, но бросил"
        user_data[user_id]["баллы"]["101B"] += 1
    else:
        user_data[user_id]["ответы"]["миноксидил"] = "Нет"
        user_data[user_id]["баллы"]["101F"] += 1
    bot.edit_message_text(f"Пробовали миноксидил?: ✅ {user_data[user_id]['ответы']['миноксидил']}", call.message.chat.id, call.message.message_id, reply_markup=None)
    show_result(call.message.chat.id, user_id)



def send_female_oiliness_question(chat_id):
    markup = types.InlineKeyboardMarkup()
    markup.add(types.InlineKeyboardButton("за 1 день и меньше", callback_data="less_day"))
    markup.add(types.InlineKeyboardButton("2-3 дня", callback_data="less_3_days"))
    markup.add(types.InlineKeyboardButton("4 и более", callback_data="More_4_days"))
    bot.send_message(chat_id, "Как быстро жирнеют корни?",reply_markup=markup)
        
@bot.callback_query_handler(func=lambda call: call.data in ["less_day", "less_3_days", "More_4_days"])
def female_oiliness (call):
    bot.answer_callback_query(call.id)
    user_id = call.from_user.id
    user = get_user(user_id)
    if call.data == "less_day":
        user_data[user_id]["ответы"]["корни"] = "за 1 день и меньше"
        user_data[user_id]["баллы"]["101G"] += 2
    elif call.data == "less_3_days":
        user_data[user_id]["ответы"]["корни"] = "2-3 дня"
        user_data[user_id]["баллы"]["101B"] += 2
    else:
        user_data[user_id]["ответы"]["корни"] = "4 и более"
        user_data[user_id]["баллы"]["101F"] += 2
    bot.edit_message_text(f"Как быстро жирнеют корни?: ✅ {user_data[user_id]['ответы']['корни']}", call.message.chat.id, call.message.message_id, reply_markup=None)
    send_female_dandruff(call.message.chat.id)
    
    
def send_female_dandruff(chat_id):   
    markup = types.InlineKeyboardMarkup()
    markup.add(types.InlineKeyboardButton("да", callback_data="dandruff_Yes"))
    markup.add(types.InlineKeyboardButton("нет", callback_data="dandruff_No"))
    bot.send_message(chat_id, "Есть ли перхоть/зуд?", reply_markup=markup)
            
@bot.callback_query_handler(func=lambda call: call.data in ["dandruff_Yes", "dandruff_No"])
def female_dandruff (call):
    bot.answer_callback_query(call.id)
    user_id = call.from_user.id
    user = get_user(user_id)
    if call.data == "dandruff_Yes":
        user_data[user_id]["ответы"]["перхоть"] = "Да"
        user_data[user_id]["баллы"]["101G"] += 3
    else:
        user_data[user_id]["ответы"]["перхоть"] = "Нет"
        user_data[user_id]["баллы"]["101B"] += 1
    bot.edit_message_text(f"Есть ли перхоть/зуд?: ✅ {user_data[user_id]['ответы']['перхоть']}", call.message.chat.id, call.message.message_id, reply_markup=None)
    send_loss_type(call.message.chat.id)
    
    
def send_loss_type (chat_id):
    markup = types.InlineKeyboardMarkup()
    markup.add(types.InlineKeyboardButton("Равномерно по всей голове", callback_data="all_over_the_head"))
    markup.add(types.InlineKeyboardButton("На макушке/пробор", callback_data="on_the_crown"))
    markup.add(types.InlineKeyboardButton("Круглые очаги", callback_data="circular_hearths"))
    markup.add(types.InlineKeyboardButton("Истощение волос", callback_data="hair_thinning"))
    bot.send_message(chat_id, "Как выпадают волосы?", reply_markup=markup)
        
@bot.callback_query_handler(func=lambda call: call.data in ["all_over_the_head", "on_the_crown", "circular_hearths", "hair_thinning"])
def loss_type (call):
    bot.answer_callback_query(call.id)
    user_id = call.from_user.id
    user = get_user(user_id)
    if call.data == "all_over_the_head":
        user_data[user_id]["ответы"]["как"] = "Равномерно по всей голове"
        user_data[user_id]["баллы"]["101B"] += 3
    elif call.data == "on_the_crown":
        user_data[user_id]["ответы"]["как"] = "На макушке/пробор"
        user_data[user_id]["баллы"]["101G"] += 3
    elif call.data == "circular_hearths":
        user_data[user_id]["ответы"]["как"] = "Круглые очаги"
        user_data[user_id]["баллы"]["101F"] += 3
    else:
        user_data[user_id]["ответы"]["как"] = "Истощение волос"
        user_data[user_id]["баллы"]["101B"] += 1
    bot.edit_message_text(f"Как выпадают волосы?: ✅ {user_data[user_id]['ответы']['как']}", call.message.chat.id, call.message.message_id, reply_markup=None)
    send_female_triggers(call.message.chat.id)
    
    
def send_female_triggers (chat_id):         
    markup = types.InlineKeyboardMarkup()
    markup.add(types.InlineKeyboardButton("Да", callback_data="stimulating_situations_yes"))
    markup.add(types.InlineKeyboardButton("Нет", callback_data="stimulating_situations_no"))
    bot.send_message(chat_id, "Были ли роды/отмена ОК/стресс?", reply_markup=markup)
            
@bot.callback_query_handler(func=lambda call: call.data in ["stimulating_situations_yes", "stimulating_situations_no"])
def female_triggers (call):
    bot.answer_callback_query(call.id)
    user_id = call.from_user.id
    user = get_user(user_id)
    if call.data == "stimulating_situations_yes":
        user_data[user_id]["ответы"]["роды"] = "Да"
        user_data[user_id]["баллы"]["101B"] += 3
    else:
        user_data[user_id]["ответы"]["роды"] = "Нет"
        user_data[user_id]["баллы"]["101G"] += 1
    bot.edit_message_text(f"Были ли роды/отмена ОК/стресс?: ✅ {user_data[user_id]['ответы']['роды']}", call.message.chat.id, call.message.message_id, reply_markup=None)
    send_female_heredity(call.message.chat.id)
    
    
def send_female_heredity (chat_id):    
    markup = types.InlineKeyboardMarkup()
    markup.add(types.InlineKeyboardButton("Да", callback_data="heredity_yes"))
    markup.add(types.InlineKeyboardButton("Нет", callback_data="heredity_no"))
    bot.send_message(chat_id, "Наследственность?", reply_markup=markup)
            
@bot.callback_query_handler(func=lambda call: call.data in ["heredity_yes", "heredity_no"])
def female_heredity (call):
    bot.answer_callback_query(call.id)
    user_id = call.from_user.id
    user = get_user(user_id)
    if call.data == "heredity_yes":
        user_data[user_id]["ответы"]["наследственность"] = "Да"
        user_data[user_id]["баллы"]["101G"] += 3
    else:
        user_data[user_id]["ответы"]["наследственность"] = "Нет"
        user_data[user_id]["баллы"]["101B"] += 2
    bot.edit_message_text(f"Наследственность?: ✅ {user_data[user_id]['ответы']['наследственность']}", call.message.chat.id, call.message.message_id, reply_markup=None)
    send_hair_condition(call.message.chat.id)
    
    
def send_hair_condition (chat_id):    
    markup = types.InlineKeyboardMarkup()
    markup.add(types.InlineKeyboardButton("Тонкие", callback_data="thin"))
    markup.add(types.InlineKeyboardButton("Нормальные", callback_data="normal"))
    bot.send_message(chat_id, "Состояние волос?", reply_markup=markup)
            
@bot.callback_query_handler(func=lambda call: call.data in["thin", "normal"])
def hair_condition(call):
    bot.answer_callback_query(call.id)
    user_id = call.from_user.id
    user = get_user(user_id)
    if call.data == "thin":
        user_data[user_id]["ответы"]["состояние"] = "Тонкие"
        user_data[user_id]["баллы"]["101G"] += 2
    else:
        user_data[user_id]["ответы"]["состояние"] = "Нормальные"
        user_data[user_id]["баллы"]["101F"] += 1
    bot.edit_message_text(f"Состояние волос?: ✅ {user_data[user_id]['ответы']['состояние']}", call.message.chat.id, call.message.message_id, reply_markup=None)
    show_result(call.message.chat.id, user_id)
    

def show_result (chat_id, user_id):
    scores = user_data[user_id]["баллы"]
    best_tonic = max(scores, key=scores.get)
    tonic_info={"101G":{"name": "Тоник 101G", "desc": "По вашим ответам больше всего подходит Тоник 101G", "image": "101G.jpeg"},
    "101B": {"name": "Тоник 101B", "desc": "По вашим ответам больше всего подходит Тоник 101B.", "image": "101B.jpeg"},
    "101F": {"name": "101 Formula", "desc": "По вашим ответам больше всего подходит Тоник 101F",  "image": "101F.jpeg"}}
    info = tonic_info[best_tonic]
    markup = types.InlineKeyboardMarkup()
    markup.add(types.InlineKeyboardButton("Перейти на сайт", url="https://fabao.ru"))
    markup.add(types.InlineKeyboardButton("Пройти тест заново", callback_data = "start_test"))
    try:
        with open(f"photo_results/{info['image']}", 'rb') as photo:
            bot.send_photo (chat_id, photo, caption=f"✅ Тест завершён!\n\nВам подходит: <b>{info['name']}</b>\n\n{info['desc']}\n\nПодробнее на сайте: fabao.ru", reply_markup=markup, parse_mode='HTML')
    except FileNotFoundError:
        bot.send_message(chat_id,f"✅ Тест завершён!\n\n" 
            f"Вам подходит: <b>{info['name']}</b>\n\n"f"{info['desc']}\n\n"
            f"<em> Результат носит информационный характер и помогает сориентирваться в ассортименте. При выраженном или длительном выпадении волос рекомендуем обратиться к специалисту.</em>\n\n"
    f"Подробнее на сайте: ", parse_mode='HTML')
bot.infinity_polling()
