# 🤖 RAG Telegram Bot на n8n

Telegram-бот с системой RAG (Retrieval-Augmented Generation) для ответов на вопросы по содержимому документов Google Docs. Построен на платформе n8n с использованием Docker.

## 📋 Содержание

- [Описание проекта](#описание-проекта)
- [Технологии](#технологии)
- [Требования](#требования)
- [Быстрый старт](#быстрый-старт)
- [Подробная инструкция по установке](#подробная-инструкция-по-установке)
- [Как использовать бота](#как-использовать-бота)
- [Архитектура](#архитектура)


---

## 📖 Описание проекта

Этот проект представляет собой Telegram-бота, который:
- Индексирует документы из Google Docs
- Отвечает на вопросы по содержимому документа
- Использует RAG (Retrieval-Augmented Generation) для точных ответов
- Работает полностью на вашем компьютере через Docker

### Как это работает (RAG пайплайн):

1. **Индексация**: Документ скачивается → разбивается на части (chunks) → конвертируется в векторные представления (embeddings) → сохраняется в памяти
2. **Ответ на вопрос**: Вопрос → конвертируется в вектор → ищутся похожие части документа → LLM генерирует ответ на основе найденных частей

---

## 🛠 Технологии

- **n8n** - платформа для автоматизации workflow
- **Docker** - контейнеризация приложения
- **Telegram Bot API** - интерфейс взаимодействия
- **Groq (LLaMA 3.1)** - языковая модель для генерации ответов
- **HuggingFace** - модель для создания эмбеддингов
- **LangChain** - фреймворк для RAG
- **Google Docs API** - источник документов

---

## ✅ Требования

### Программное обеспечение:
- [Docker Desktop](https://www.docker.com/products/docker-desktop) (Windows/Mac/Linux)
- [Git](https://git-scm.com/downloads)
- Текстовый редактор (Notepad, VS Code и т.д.)

### API ключи:

1. **Telegram Bot Token**
   - Откройте Telegram → найдите [@BotFather](https://t.me/BotFather)
   - Отправьте `/newbot`
   - Следуйте инструкциям
   - Скопируйте токен (формат: `123456:ABC-DEF1234ghIkl-zyx57W2v1u123ew11`)

2. **Groq API Key**
   - Перейдите на [console.groq.com](https://console.groq.com)
   - Зарегистрируйтесь (бесплатно)
   - Перейдите в "API Keys" → "Create API Key"
   - Скопируйте ключ (формат: `gsk_...`)

3. **HuggingFace Token**
   - Перейдите на [huggingface.co](https://huggingface.co)
   - Зарегистрируйтесь
   - Settings → Access Tokens → "Create new token"
   - Выберите "Read" access
   - Скопируйте токен (формат: `hf_...`)

4. **Google Document URL**
   - Создайте документ в Google Docs
   - File → Share → "Anyone with the link can view"
   - Скопируйте ссылку
   - Преобразуйте в формат экспорта: `https://docs.google.com/document/d/DOCUMENT_ID/export?format=txt`

---

## 🚀 Быстрый старт

```bash
# 1. Клонируйте репозиторий
git clone https://github.com/ваш-username/rag-telegram-bot.git
cd rag-telegram-bot

# 2. Создайте файл .env из шаблона
copy .env.example .env    # Windows
# или
cp .env.example .env      # Linux/Mac

# 3. Отредактируйте .env и вставьте ваши API ключи
notepad .env              # Windows
# или
nano .env                 # Linux/Mac

# 4. Запустите Docker контейнер
docker-compose up -d

# 5. Откройте n8n в браузере
# http://localhost:5678

# 6. Следуйте инструкциям по настройке workflow
```

---

## 📝 Подробная инструкция по установке

### Шаг 1: Установка Docker

#### Windows:
1. Скачайте [Docker Desktop для Windows](https://www.docker.com/products/docker-desktop)
2. Запустите установщик
3. После установки перезагрузите компьютер
4. Запустите Docker Desktop
5. Дождитесь запуска (иконка Docker в трее станет зеленой)

#### Linux:
```bash
# Ubuntu/Debian
sudo apt update
sudo apt install docker.io docker-compose
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker $USER
# Перелогиньтесь после этого
```

#### Mac:
1. Скачайте [Docker Desktop для Mac](https://www.docker.com/products/docker-desktop)
2. Перетащите Docker в Applications
3. Запустите Docker из Applications
4. Дождитесь запуска

### Шаг 2: Клонирование репозитория

```bash
# Откройте терминал (PowerShell на Windows, Terminal на Mac/Linux)

# Перейдите в папку где хотите разместить проект
cd Desktop    # или любая другая папка

# Клонируйте репозиторий
git clone https://github.com/ваш-username/rag-telegram-bot.git

# Перейдите в папку проекта
cd rag-telegram-bot
```

### Шаг 3: Настройка переменных окружения

1. **Создайте файл `.env`:**
   ```bash
   # Windows PowerShell
   Copy-Item .env.example .env
   
   # Linux/Mac
   cp .env.example .env
   ```

2. **Откройте `.env` в редакторе:**
   ```bash
   # Windows
   notepad .env
   
   # Linux/Mac
   nano .env
   # или
   code .env   # если установлен VS Code
   ```

3. **Заполните все поля:**
   ```env
   TELEGRAM_BOT_TOKEN=ваш_токен_от_BotFather
   GROQ_API_KEY=ваш_ключ_от_Groq
   HUGGINGFACE_TOKEN=ваш_токен_от_HuggingFace
   GOOGLE_DOC_URL=https://docs.google.com/document/d/ВАШ_ID/export?format=txt
   ```

4. **Сохраните файл** (Ctrl+S в Windows, Cmd+S на Mac)

### Шаг 4: Запуск Docker контейнера

```bash
# Убедитесь что Docker запущен
docker ps

# Если ошибка - запустите Docker Desktop

# Запустите контейнер
docker-compose up -d

# Проверьте что контейнер запущен
docker ps

# Должна быть строка с "n8n_rag_bot"
```

**Ожидание запуска:** Подождите 15-20 секунд пока n8n полностью запустится.

### Шаг 5: Первоначальная настройка n8n

1. **Откройте браузер** и перейдите на: `http://localhost:5678`

2. **Регистрация:**
   - Email: любой (например: `test@example.com`)
   - Password: любой пароль
   - First Name / Last Name: любые данные
   - Нажмите "Get Started"

3. **Импорт workflow:**
   - Нажмите значок "+" в правом верхнем углу
   - Выберите "Import from File"
   - Найдите и выберите файл `RAG_polling_google.json`
   - Нажмите "Import"

### Шаг 6: Настройка Credentials

После импорта workflow нужно настроить учетные данные:

#### 6.1 Telegram Credential:

1. Найдите любой узел с Telegram (например "Send Telegram Response")
2. Кликните на поле **Credential to connect with**
3. Нажмите **"+ Create New"**
4. В поле **Access Token** вставьте ваш `TELEGRAM_BOT_TOKEN` из файла `.env`
5. Нажмите **"Save"**
6. Примените этот же credential ко всем узлам Telegram в workflow

#### 6.2 Groq Credential:

1. Найдите узел **"Groq Chat Model"**
2. Кликните на поле **Credential to connect with**
3. Нажмите **"+ Create New"**
4. В поле **API Key** вставьте ваш `GROQ_API_KEY` из файла `.env`
5. Нажмите **"Save"**

#### 6.3 HuggingFace Credential:

1. Найдите узел **"Embeddings HuggingFace Inference"**
2. Кликните на поле **Credential to connect with**
3. Нажмите **"+ Create New"**
4. В поле **API Key** вставьте ваш `HUGGINGFACE_TOKEN` из файла `.env`
5. Нажмите **"Save"**

### Шаг 7: Обновление URL документа

1. Найдите узел **"Download Document for Indexing"**
2. Кликните на него
3. В поле **URL** замените на ваш `GOOGLE_DOC_URL` из файла `.env`
4. Нажмите **"Save"**

### Шаг 8: Обновление токена в Get Telegram Updates

1. Найдите узел **"Get Telegram Updates"**
2. В поле **URL** замените `   ` на токен из txt
4. Нажмите **"Save"**

### Шаг 9: Активация workflow

1. В правом верхнем углу найдите переключатель **"Active"**
2. Переключите его в положение **ON** (станет зеленым)
3. Workflow начнет работать автоматически каждые 3 секунды

### Шаг 10: Тестирование

1. Откройте Telegram
2. Найдите вашего бота (по имени которое дали в BotFather)
3. Отправьте команду: `/index`
4. Бот должен ответить: "Document indexed successfully! You can now ask questions about it."
5. Задайте любой вопрос по документу
6. Через 3-6 секунд бот должен ответить

---

## 💬 Как использовать бота

### Команды:

- **`/index`** - проиндексировать документ (нужно сделать первым делом)

### Задавать вопросы:

После индексации просто напишите боту любой вопрос:
- "Что говорится о машинном обучении?"
- "Какие основные темы в документе?"
- "Объясни концепцию RAG"

Бот найдет релевантные части документа и сформирует ответ.

---



### Workflow узлы:

1. **Poll Every 3 Seconds** - Запускает проверку новых сообщений каждые 3 секунды
2. **Get Telegram Updates** - Получает новые сообщения через Telegram API
3. **Process Messages** - Извлекает текст сообщения
4. **Mark Message As Read** - Помечает сообщение как обработанное
5. **Check Message Type** - Определяет тип команды (/index или вопрос)
6. **Download Document** - Скачивает Google Doc
7. **Document Loader** - Загружает текст в систему
8. **Recursive Text Splitter** - Разбивает на части (chunks)
9. **Embeddings HuggingFace** - Создает векторные представления
10. **Index Documents** - Сохраняет в векторное хранилище
11. **Query Documents Tool** - Ищет релевантные части
12. **Question Answering Agent** - Генерирует ответ с помощью Groq LLM
13. **Send Telegram Response** - Отправляет ответ пользователю

---

## 🛑 Управление контейнером

### Основные команды:

```bash
# Запустить контейнер
docker-compose up -d

# Остановить контейнер
docker-compose down

# Перезапустить контейнер
docker-compose restart

# Посмотреть логи
docker logs n8n_rag_bot -f

# Остановить и удалить все данные
docker-compose down -v

# Проверить статус
docker ps
```



### Детали:

- **RAG Pipeline:** Recursive Character Text Splitter → HuggingFace Embeddings → In-Memory Vector Store → Semantic Search → Groq LLM
- **Chunk Size:** 1000 символов с overlap 200
- **Embedding Model:** HuggingFace Inference API (по умолчанию)
- **LLM:** Groq (LLaMA 3.1 8B Instant)
- **Polling Interval:** 3 секунды
- **Vector Store:** In-Memory (данные не сохраняются после перезапуска)

---

## 📚 Полезные ссылки

- [Документация n8n](https://docs.n8n.io)
- [Telegram Bot API](https://core.telegram.org/bots/api)
- [Что такое RAG](https://www.pinecone.io/learn/retrieval-augmented-generation/)
- [LangChain документация](https://python.langchain.com/)
- [Groq API](https://console.groq.com/docs)
- [HuggingFace](https://huggingface.co/docs)