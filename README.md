# Выполнение Дз-2

## Изменения в проекте 

В рамках текущего дедлайна реализована модель на основе свёрточной нейронной сети (CNN) 
для классификации дорожных знаков с использованием датасета
[GTSRB](https://www.kaggle.com/datasets/meowmeowmeowmeowmeow/gtsrb-german-traffic-sign)

При выборе изначальной темы проекта я ***переоценил свои возможности***: сложность задачи, 
дедлайны и приближение Новогодних праздников вынудили меня пересмотреть планы. 
Чтобы успеть к сдаче ДЗ-2 и получить ***важные для меня комментарии***, я решил упростить проект.

Если текущая модель покажется ***слишком простой***,
то я постараюсь её усложнить к следующему дедлайну ДЗ-3.

## Структура проекта

```bash 
Detection-and-classification-of-road-signs
├── data
│   ├── data_infer            # Данные для инференса
│       └── id_4.png
├── sign_recognizer           # Код проекта
│   ├── constants.py          # Константы и гиперпараметры
│   ├── dataset.py            # Определение датасета
│   ├── infer.py              # Скрипт для инференса
│   ├── model.py              # Определение архитектуры модели (GTSRB_model)
│   ├── test.py               # Скрипт для тестирования
│   ├── train.py              # Скрипт для обучения
│   └── trainer.py            # Lightning Trainer
├── .gitignore               
├── .pre-commit-config.yaml   # Конфигурация для pre-commit 
├── commands.py               # Скрипт для запуска train, test, infer через CLI
├── Dockerfile               
├── poetry.lock               # Зафиксированные версии зависимостей
├── pyproject.toml            # Зависимости проекта и их версии
└── README.md                 
 ```

---
# Описание проекта
### Детекция и классификация дорожных знаков на изображениях

**Цель проекта**: разработать модель глубокого обучения, которая будет способна обнаруживать и классифицировать российские дорожные знаки на изображениях.

---

### Данные

1. **RTSD (Russian Traffic Sign Dataset)**:
   - **Ссылка:** [Kaggle: RTSD Dataset](https://www.kaggle.com/datasets/watchman/rtsd-dataset/data) и [RTSD на официальном сайте](https://graphics.cs.msu.ru/projects/traffic-sign-recognition.html).
   - Датасет состоит из более чем 40,000 изображений дорожных знаков, собранных в реальных условиях России.
   - **Формат данных:**
     - Изображения: RGB-формат, высокого разрешения.
     - Аннотации: Присутствуют метки классов и координаты ограничивающих рамок в формате XML.

### Проблемы

- **Дисбаланс классов:** Некоторые типы знаков встречаются реже.
- **Вариативность условий:** Различные погодные условия и углы съёмки.
- **Масштабирование объектов:** Знаки на изображениях имеют разный размер.

---

### Архитектура

- **Faster R-CNN**: современная двухэтапная модель для детекции объектов. Она выполняет генерацию регионов интереса (RPN) и классификацию объектов в рамках этих регионов.
- **Backbone:** ResNet50 или ResNet101 для извлечения признаков.

Библиотеки: PyTorch, torchvision.

---

### Этапы моделирования

1. **Подготовка данных:**
   - Разделение на обучающую, валидационную и тестовую выборки.
   - Применение аугментации.

2. **Моделирование:**
   - Загрузка предобученной модели Faster R-CNN с backbone ResNet50.
   - Fine-tuning на датасете RTSD.

3. **Оценка модели:**
   - Метрики: mAP, Precision, Recall, F1-score.

---

### Продакшен пайплайн

1. **Входные данные:**
   - Принимаются изображения или видеопоток с камер.
2. **Обработка:**
   - Модель обрабатывает изображение и выделяет дорожные знаки, возвращая рамки и классы.
3. **Вывод:**
   - Выводятся координаты знаков и их тип.
   - Визуализация на изображении.

---

### Финальное применение

- **Веб-приложение:** Пользователь загружает изображение дороги, получает разметку дорожных знаков с указанием классов.
