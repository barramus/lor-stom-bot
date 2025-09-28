# Бот «Стоматолог → ЛОР» (готов к деплою в Yandex Cloud)

Бот помогает стоматологам быстро передавать данные о пациенте ЛОР-врачу.

## ✨ Возможности

- ✅ **Регистрация стоматолога**: ФИО, место работы, телефон, username (для обратной связи).
- ✅ **Создание заявки**: жалобы, анамнез, план лечения.
- ✅ **Приём нескольких вложений** (фото и документы).
- ✅ **Автоматическая сборка ZIP-архива**  
  (summary.txt + вложения) и отправка ЛОР-врачу одним сообщением.
- ✅ **Гибкое редактирование профиля** (команды `/set_name`, `/set_phone`, `/set_workplace`).
- ✅ **Список заявок** с возможностью просмотреть каждую через inline-кнопку.
- ✅ Постоянное **главное меню** (кнопки внизу чата).

---

## 🚀 Локальный запуск

1. Установите Python **3.11+**
2. Установите зависимости:
   ```bash
   pip install -r requirements.txt
Создайте .env (копия из .env.example):
   ```bash
BOT_TOKEN=ваш_токен_бота
LOR_TARGET_CHAT_ID=123456789
ADMIN_IDS=123456789
BOT_TOKEN — токен от @BotFather

LOR_TARGET_CHAT_ID — chat_id ЛОР-врача (узнать через @userinfobot)

ADMIN_IDS — список chat_id администраторов через запятую (опционально)

   ```bash
Запустите бот:

python -m app.bot

☁️ Развёртывание на VPS (Яндекс Облако)

1. Подключение к серверу

Создайте ВМ (Ubuntu 22.04 LTS), добавьте свой SSH-ключ при создании.

Подключение:

ssh <имя_пользователя>@<IP_VM>

2. Установка Python и зависимостей
sudo apt update && sudo apt upgrade -y
sudo apt install -y python3.11 python3.11-venv git

3. Загрузка проекта

На локальном компьютере:

cd путь/к/проекту
git remote add origin git@github.com:ваш_логин/lor-stom-bot.git
git push origin main

На сервере:

git clone https://github.com/<ваш_логин>/lor-stom-bot.git
cd lor-stom-bot
python3.11 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
nano .env  # заполните токен и chat_id

4. Запуск
source venv/bin/activate
python -m app.bot

Чтобы бот работал постоянно — используйте tmux или systemd.

Пример systemd-сервиса
sudo nano /etc/systemd/system/lor-bot.service

[Unit]

Description=LOR Telegram Bot
After=network.target

[Service]

User=ubuntu
WorkingDirectory=/home/ubuntu/lor-stom-bot
ExecStart=/home/ubuntu/lor-stom-bot/venv/bin/python -m app.bot
Restart=always

[Install]

WantedBy=multi-user.target
sudo systemctl daemon-reload
sudo systemctl enable lor-bot
sudo systemctl start lor-bot
sudo systemctl status lor-bot

🔧 Доступные команды

Команда	Описание
/start	Главное меню и регистрация, если вы новый пользователь
/new	Создать новую консультацию
/me	Показать свои данные
/set_name	Изменить ФИО
/set_phone	Изменить номер телефона
/set_workplace	Изменить место работы
/list	Показать список ваших заявок
/cancel	Отменить текущий ввод

🔒 Безопасность

В БД сохраняются только текстовые данные и file_id Telegram — файлы не хранятся на сервере.

ZIP собирается во временной директории и удаляется после отправки.

Все токены и идентификаторы держите только в .env (не коммитите их в GitHub).