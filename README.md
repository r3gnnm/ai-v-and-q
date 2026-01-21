# ai-v-and-q
# 🎓 Video Quiz System - AI-Powered Interactive Learning

Интеллектуальная система, которая автоматически анализирует видео, делит его на смысловые сегменты и генерирует интерактивные тесты прямо в видеоплеере.

## 📋 Содержание

- [Возможности](#возможности)
- [Архитектура](#архитектура)
- [Технологический стек](#технологический-стек)
- [Быстрый старт](#быстрый-старт)
- [Установка и настройка](#установка-и-настройка)
- [API Документация](#api-документация)
- [Browser Extension](#browser-extension)
- [Примеры использования](#примеры-использования)
- [Troubleshooting](#troubleshooting)

---

## ✨ Возможности

### Backend (Python FastAPI)
- ✅ Загрузка и обработка видео (.mp4, .mov, .avi)
- ✅ Извлечение аудиодорожки и ключевых фреймов
- ✅ Распознавание речи с временными метками (Whisper)
- ✅ Автоматическая сегментация по темам (LLM-powered)
- ✅ Генерация тестов для каждого сегмента
- ✅ REST API с Swagger документацией
- ✅ Отслеживание прогресса пользователей

### Browser Extension
- ✅ Интеграция с HTML5 video и YouTube
- ✅ In-video overlay для квизов
- ✅ Автоматическая пауза в конце сегмента
- ✅ Блокировка перемотки до прохождения теста
- ✅ Визуальная обратная связь (успех/ошибка)

---

## 🏗 Архитектура

```
┌─────────────────────────────────────────────────────┐
│                    VIDEO INPUT                       │
│              (.mp4, .mov, .avi, URL)                 │
└──────────────────────┬──────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────┐
│              VIDEO PROCESSING                        │
│  ┌──────────────┐  ┌──────────────┐                │
│  │ Frame Extract│  │ Audio Extract│                 │
│  │   (OpenCV)   │  │   (ffmpeg)   │                 │
│  └──────────────┘  └──────────────┘                 │
└──────────────────────┬──────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────┐
│         SPEECH RECOGNITION (Whisper)                 │
│              Audio → Text + Timestamps               │
└──────────────────────┬──────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────┐
│        TOPIC SEGMENTATION (LLM)                      │
│  ┌──────────────────────────────────────┐           │
│  │  Text Analysis + Scene Detection     │           │
│  │  → Thematic Segments                 │           │
│  └──────────────────────────────────────┘           │
└──────────────────────┬──────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────┐
│          QUIZ GENERATION (LLM)                       │
│    Multiple Choice + True/False + Short Answer      │
└──────────────────────┬──────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────┐
│              REST API + DATABASE                     │
│         ┌────────────┬────────────┐                 │
│         │  Segments  │   Quizzes  │                 │
│         │  Progress  │   Videos   │                 │
│         └────────────┴────────────┘                 │
└──────────────────────┬──────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────┐
│           BROWSER EXTENSION                          │
│  ┌──────────────────────────────────────┐           │
│  │   Video Player + Quiz Overlay        │           │
│  │   - Auto-pause at segment end        │           │
│  │   - Quiz display & validation        │           │
│  │   - Progress tracking                │           │
│  └──────────────────────────────────────┘           │
└─────────────────────────────────────────────────────┘
```

---

## 🛠 Технологический стек

### Backend
- **Framework:** FastAPI 0.109.0
- **Video:** OpenCV, ffmpeg
- **ASR:** Faster-Whisper (open-source)
- **LLM:** Ollama (llama3/mistral) - локально
- **Database:** PostgreSQL / SQLite
- **Async Tasks:** Celery + Redis (опционально)

### Frontend (Browser Extension)
- **Manifest:** V3 (Chrome/Firefox)
- **Vanilla JavaScript:** Без фреймворков
- **CSS3:** Современные анимации и градиенты

---

## 🚀 Быстрый старт

### Вариант 1: Docker (Рекомендуется)

```bash
# Клонировать репозиторий
git clone https://github.com/your-repo/video-quiz-system.git
cd video-quiz-system

# Запустить все сервисы
docker-compose up -d

# Дождаться готовности (1-2 минуты)
docker-compose logs -f backend

# Установить модель Ollama
docker exec -it video-quiz-ollama ollama pull llama3

# API доступен на http://localhost:8000
# Swagger UI: http://localhost:8000/docs
```

### Вариант 2: Локальная установка

```bash
# 1. Установить зависимости системы
sudo apt-get install ffmpeg python3-dev

# 2. Создать виртуальное окружение
python3 -m venv venv
source venv/bin/activate

# 3. Установить Python пакеты
pip install -r requirements.txt

# 4. Установить и запустить Ollama
curl https://ollama.ai/install.sh | sh
ollama serve &
ollama pull llama3

# 5. Запустить backend
cd backend
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000

# 6. (Опционально) Создать тестовые данные
curl -X POST http://localhost:8000/debug/create_test_data
```

---

## 📦 Установка и настройка

### Backend Setup

#### 1. Структура проекта
```
video-quiz-system/
├── backend/
│   ├── app/
│   │   ├── api/
│   │   │   └── endpoints/
│   │   ├── services/
│   │   │   ├── video_processor.py
│   │   │   ├── asr_service.py
│   │   │   ├── segmentation_service.py
│   │   │   └── quiz_generator.py
│   │   └── main.py
│   ├── uploads/
│   ├── processing/
│   ├── requirements.txt
│   └── Dockerfile
├── extension/
│   ├── manifest.json
│   ├── content.js
│   ├── popup.html
│   ├── popup.js
│   └── styles.css
├── docker-compose.yml
└── README.md
```

#### 2. Переменные окружения (.env)
```env
# Database
DATABASE_URL=postgresql://user:password@localhost:5432/videoquiz

# Redis
REDIS_URL=redis://localhost:6379/0

# Ollama
OLLAMA_ENDPOINT=http://localhost:11434/api/generate
OLLAMA_MODEL=llama3

# Whisper
WHISPER_MODEL=base  # tiny, base, small, medium, large

# Storage
UPLOAD_DIR=uploads
PROCESSING_DIR=processing
```

#### 3. База данных (опционально)
```bash
# Создать базу PostgreSQL
createdb videoquiz

# Запустить миграции (если используете Alembic)
alembic upgrade head
```

### Browser Extension Setup

#### 1. Установка в Chrome
```
1. Откройте chrome://extensions/
2. Включите "Developer mode"
3. Нажмите "Load unpacked"
4. Выберите папку extension/
5. Готово! Расширение установлено
```

#### 2. Установка в Firefox
```
1. Откройте about:debugging#/runtime/this-firefox
2. Нажмите "Load Temporary Add-on"
3. Выберите manifest.json в папке extension/
4. Готово!
```

---

## 📚 API Документация

### Основные эндпоинты

#### 1. Загрузка видео
```http
POST /video/upload
Content-Type: multipart/form-data

{
  "file": <binary>
}
```

**Response:**
```json
{
  "id": "uuid-string",
  "filename": "lecture.mp4",
  "duration": 0.0,
  "status": "processing",
  "segments_count": 0
}
```

#### 2. Получить сегменты
```http
GET /video/{video_id}/segments
```

**Response:**
```json
[
  {
    "id": "seg-001",
    "video_id": "uuid",
    "start_time": "00:00:00",
    "end_time": "00:03:45",
    "topic_title": "Introduction to ML",
    "short_summary": "Overview of machine learning concepts",
    "keywords": ["machine learning", "AI"],
    "quiz_id": "quiz-001"
  }
]
```

#### 3. Получить тест
```http
GET /segment/{segment_id}/quiz
```

**Response:**
```json
{
  "id": "quiz-001",
  "segment_id": "seg-001",
  "questions": [
    {
      "id": "q1",
      "type": "multiple_choice",
      "question": "What is machine learning?",
      "options": ["A", "B", "C", "D"],
      "correct_answer": "A"
    }
  ]
}
```

#### 4. Проверить ответ
```http
POST /segment/{segment_id}/answer
Content-Type: application/json

{
  "question_id": "q1",
  "user_answer": "A"
}
```

**Response:**
```json
{
  "correct": true,
  "correct_answer": null,
  "message": "Correct! You may continue."
}
```

#### 5. Прогресс пользователя
```http
GET /video/{video_id}/progress/{user_id}
```

**Response:**
```json
{
  "user_id": "user-123",
  "video_id": "uuid",
  "completed_segments": [0, 1],
  "current_segment": 2,
  "locked": false
}
```

### Swagger UI
Полная интерактивная документация доступна на:
```
http://localhost:8000/docs
```

---

## 🌐 Browser Extension

### Функциональность

#### In-Video Quiz Flow
1. **Просмотр видео** → система отслеживает время
2. **Конец сегмента** → автоматическая пауза
3. **Показ квиза** → overlay поверх видео
4. **Ответ пользователя** → валидация
5. **Результат:**
   - ✅ **Правильно:** разблокировка следующего сегмента
   - ❌ **Неправильно:** перемотка к началу сегмента

#### Блокировка перемотки
- Пользователь не может перемотать вперёд дальше пройденных сегментов
- При попытке → сообщение "Complete the current quiz to unlock"

### Кастомизация

Отредактируйте `content.js` для изменения поведения:

```javascript
// Минимальное время сегмента (секунды)
min_segment_duration: 60.0

// Порог для детекции смены сцены
scene_change_threshold: 25.0

// Количество вопросов на сегмент
questions_per_segment: 4
```

---

## 🎬 Примеры использования

### Пример 1: Обработка видео через API

```python
import requests

# 1. Загрузить видео
with open('lecture.mp4', 'rb') as f:
    files = {'file': f}
    response = requests.post('http://localhost:8000/video/upload', files=files)
    video_data = response.json()
    video_id = video_data['id']

# 2. Дождаться обработки (обычно 2-5 минут для 10-минутного видео)
import time
while True:
    response = requests.get(f'http://localhost:8000/video/{video_id}')
    status = response.json()['status']
    if status == 'ready':
        break
    print(f'Status: {status}')
    time.sleep(10)

# 3. Получить сегменты
segments = requests.get(f'http://localhost:8000/video/{video_id}/segments').json()
print(f'Found {len(segments)} segments')

for seg in segments:
    print(f"- {seg['topic_title']} ({seg['start_time']} - {seg['end_time']})")
```

### Пример 2: Использование с YouTube

```javascript
// 1. Открыть YouTube видео
// 2. Extension автоматически активируется
// 3. Открыть popup расширения
// 4. Нажать "Upload & Process Video"
// 5. Выбрать локальное видео или дождаться автообнаружения
```

### Пример 3: Прямая обработка через Pipeline

```python
from video_pipeline import VideoPipeline

pipeline = VideoPipeline(
    work_dir="processing",
    whisper_model="base",
    llm_model="llama3"
)

results = pipeline.process_video(
    video_path="path/to/video.mp4",
    extract_frames=True,
    questions_per_segment=4
)

print(f"Segments: {len(results['segments'])}")
print(f"Quizzes: {len(results['quizzes'])}")
```

---

## 🐛 Troubleshooting

### Backend проблемы

#### 1. Whisper не устанавливается
```bash
# Проблема с CUDA/GPU
pip install faster-whisper --no-binary :all:

# Или использовать CPU версию
pip install faster-whisper
```

#### 2. ffmpeg не найден
```bash
# Ubuntu/Debian
sudo apt-get install ffmpeg

# macOS
brew install ffmpeg

# Windows
# Скачать с https://ffmpeg.org/download.html
```

#### 3. Ollama не отвечает
```bash
# Проверить статус
ollama list

# Перезапустить
pkill ollama
ollama serve &

# Убедиться, что модель загружена
ollama pull llama3
```

#### 4. Медленная обработка
```bash
# Использовать меньшую модель Whisper
WHISPER_MODEL=tiny  # быстрее, но менее точно

# Или использовать GPU (если доступно)
pip install faster-whisper[cuda]
```

### Extension проблемы

#### 1. Overlay не появляется
- Проверьте консоль браузера (F12) на ошибки
- Убедитесь, что backend запущен
- Перезагрузите страницу с видео

#### 2. Backend недоступен
- Проверьте, что API запущен: `curl http://localhost:8000`
- Проверьте CORS настройки в `main.py`
- Убедитесь, что URL правильный в `content.js`

#### 3. Квизы не загружаются
```javascript
// Откройте консоль и проверьте:
console.log('Video ID:', videoId);
console.log('Segments:', segments);
console.log('Quizzes:', quizzes);
```

---

## 📊 Performance Tips

### Оптимизация обработки видео

1. **Whisper модель:**
   - `tiny`: ~1GB RAM, быстро, 75% точность
   - `base`: ~2GB RAM, средне, 85% точность ✅ рекомендуется
   - `small`: ~5GB RAM, медленно, 90% точность
   - `medium/large`: >10GB RAM, очень медленно, 95% точность

2. **Параллельная обработка:**
```python
# Используйте Celery для фоновой обработки
celery -A app.celery_app worker --concurrency=4
```

3. **Кэширование:**
- Результаты транскрипции сохраняются
- Повторная обработка не требуется

---

## 🔐 Security Notes

- ⚠️ Текущая реализация БЕЗ аутентификации (только для разработки)
- Добавьте JWT/OAuth для продакшена
- Валидация загружаемых файлов
- Rate limiting для API

---

## 📝 License

MIT License - свободное использование

---

**Создано для хакатона по AI-powered образованию 🎓**
