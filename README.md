# 📊 Система учета показаний счетчиков на Node-RED

> Умная система для сбора, валидации и анализа показаний электросчетчиков с возможностью прогнозирования потребления и ретроспективного ввода данных.

## 🎯 Описание проекта

Система предназначена для автоматизации учета электроэнергии в частных домах и небольших предприятиях. Обеспечивает полный цикл работы с данными: от ввода показаний до анализа потребления с интеллектуальными прогнозами.

## ✨ Основные возможности

- 📈 **Умный анализ потребления** - расчет по завершенным дням и прогноз текущего дня
- 🔒 **Многоуровневая валидация** - проверка корректности показаний
- 📅 **Исторические данные** - ретроспективный ввод пропущенных показаний
- 🎨 **Интерактивный dashboard** - современный веб-интерфейс
- 📊 **Детальная статистика** - среднее потребление, тренды, KPI
- 🔐 **Система авторизации** - доступ только для доверенных лиц
- 💾 **Надежное хранение** - множественное резервирование данных
- 🔄 **Модульная архитектура** - легкое расширение функциональности

## 🏗️ Архитектура системы

### Структура Flow

```
Flow 1 (Сбор данных)     → Flow 2 (Анализ)      → Flow 3-5 (Визуализация)
├── Валидация            ├── Группировка по дням  ├── Завершенные дни
├── Сохранение           ├── Расчет потребления   ├── Текущий день  
└── Уведомления          └── Статистика           └── Общая статистика

Flow 6 (Исторические данные)
├── Календарный ввод
├── Авторизация
├── Аудит изменений
└── Интеграция с основной базой
```

## 📋 Требования

### Системные требования
- **Node-RED** версии 3.0+
- **Node.js** версии 14+
- **npm** версии 6+

### Необходимые палитры Node-RED
```bash
# Dashboard для веб-интерфейса
npm install node-red-dashboard

# Дополнительные узлы (опционально)
npm install node-red-contrib-ui-level
npm install node-red-contrib-moment
```

## 🚀 Установка и настройка

### 1. Клонирование проекта

```bash
git clone https://github.com/your-username/meter-readings-system.git
cd meter-readings-system
```

### 2. Импорт в Node-RED

1. Откройте Node-RED (обычно http://localhost:1880)
2. Меню → Import → Clipboard
3. Скопируйте содержимое файла `flows.json`
4. Нажмите "Import"

### 3. Первоначальная настройка

```javascript
// Настройка доверенных пользователей (в Flow 6)
const trustedUsers = {
    'admin': 'Администратор',
    'owner': 'Владелец дома',
    'manager': 'Управляющий',
    'technician': 'Технический специалист'
};
```

### 4. Настройка Dashboard

1. Перейдите в http://localhost:1880/ui
2. Настройте группы и вкладки по необходимости
3. Проверьте отображение всех элементов

## 📊 Описание Flow

### Flow 1 - Сбор данных 📥

**Назначение:** Валидация и первичная обработка показаний

**Компоненты:**
- **Text Input** - ввод показаний счетчика
- **Text Input** - имя оператора
- **Button** - сохранение данных
- **Function: Валидация** - проверка корректности
- **Function: Сохранение** - запись в хранилище

**Валидация включает:**
- Проверка формата числа
- Сравнение с предыдущими показаниями
- Проверка заполненности полей
- Контроль логичности данных

### Flow 2 - Анализ данных 🧮

**Назначение:** Обработка и расчет потребления

**Алгоритм работы:**
1. **Группировка по дням** - сортировка показаний по календарным дням
2. **Завершенные дни** - расчет `потребление = макс_дня - макс_предыдущего_дня`
3. **Текущий день** - прогноз на основе времени и текущего потребления
4. **Статистика** - общие показатели и тренды

**Формулы расчета:**
```javascript
// Потребление за день
consumption = maxReadingToday - maxReadingYesterday

// Прогноз на день
projectedDaily = (currentConsumption / hoursElapsed) * 24

// Среднее потребление
avgDaily = totalConsumption / completedDaysCount
```

### Flow 3 - Завершенные дни 📈

**Назначение:** Визуализация исторических данных

**Выходы:**
- **Chart** - график потребления по дням
- **Table** - табличное представление
- **Summary** - сводная информация

### Flow 4 - Текущий день ⚡

**Назначение:** Мониторинг в реальном времени

**Компоненты:**
- **Gauge** - текущее потребление
- **Text** - прогноз с пометкой "предварительно"
- **Notifications** - уведомления о превышениях

### Flow 5 - Статистика 📊

**Назначение:** Аналитика и отчетность

**Показатели:**
- KPI Dashboard
- Анализ трендов
- Сводные отчеты
- Рекомендации по оптимизации

### Flow 6 - Исторические данные 📅

**Назначение:** Ввод пропущенных показаний

**Возможности:**
- Календарный выбор даты и времени
- Авторизация пользователей
- Проверка на дублирование
- Аудит изменений
- Интеграция с основной базой

## 🎮 Использование

### Ежедневный ввод показаний

1. Откройте dashboard: `http://localhost:1880/ui`
2. Введите текущие показания счетчика
3. Укажите ваше имя
4. Нажмите "Сохранить показания"
5. Проверьте результат в разделе статистики

### Ввод исторических данных

1. Перейдите на вкладку "Исторические данные"
2. Выберите дату и время
3. Введите показания
4. Выберите свое имя из списка
5. Добавьте комментарий (опционально)
6. Нажмите "Добавить историческую запись"

### Просмотр аналитики

- **Завершенные дни** - график и таблица потребления
- **Текущий день** - live-мониторинг с прогнозом
- **Статистика** - общие показатели и тренды

## ⚙️ Конфигурация

### Настройка пользователей

```javascript
// В Function node "Валидация исторических данных"
const trustedUsers = {
    'admin': 'Администратор',
    'john': 'Иван Петров',
    'mary': 'Мария Иванова'
    // Добавьте своих пользователей
};
```

### Настройка уведомлений

```javascript
// Пороги для alerts
const thresholds = {
    highConsumption: 40,    // кВт·ч в день
    lowConsumption: 5,      // кВт·ч в день
    dataGap: 24            // часов без данных
};
```

### Настройка хранения

```javascript
// Лимиты хранения
const storageLimits = {
    maxReadings: 1000,     // максимум показаний
    maxAuditLog: 500,      // максимум записей аудита
    backupFrequency: 24    // часов между резервными копиями
};
```

## 📁 Структура данных

### Формат записи показаний

```json
{
  "id": "reading_1750080875524",
  "meterId": "main_electric",
  "value": 1472.5,
  "operator": "john",
  "timestamp": 1750080875524,
  "date": "16.06.2025, 16:34:35",
  "source": "dashboard",
  "validated": true,
  "isHistorical": false
}
```

### Формат исторической записи

```json
{
  "id": "reading_hist_1750000000000",
  "meterId": "main_electric", 
  "value": 1450.0,
  "operator": "admin",
  "timestamp": 1750000000000,
  "date": "15.06.2025, 20:00:00",
  "source": "historical_input",
  "validated": true,
  "isHistorical": true,
  "inputBy": "admin",
  "inputByName": "Администратор",
  "inputDate": "2025-06-16T16:30:00.000Z",
  "comment": "Восстановление данных",
  "warnings": ["Показания меньше предыдущих"]
}
```

### Формат данных анализа

```json
{
  "type": "completed",
  "data": [
    {
      "date": "2025-06-15",
      "consumption": 25.5,
      "startReading": 1420.0,
      "endReading": 1445.5
    }
  ],
  "metadata": {
    "totalConsumption": 255.0,
    "avgConsumption": 25.5
  }
}
```

## 🔧 API и интеграции

### HTTP эндпоинты (опционально)

```javascript
// GET /api/readings - получить все показания
// POST /api/readings - добавить показание
// GET /api/stats - получить статистику
// GET /api/consumption/:date - потребление за дату
```

### Веб-хуки для внешних систем

```javascript
// Настройка webhook для отправки данных
const webhookUrl = "https://your-system.com/api/webhook";
const webhook = {
    url: webhookUrl,
    method: "POST",
    headers: { "Authorization": "Bearer YOUR_TOKEN" }
};
```

## 📊 Примеры использования

### Сценарий 1: Ежедневный мониторинг

```
09:00 - Автоматическое напоминание о снятии показаний
12:00 - Ввод показаний: 1472.5 кВт·ч
12:01 - Система рассчитала: потребление 12.5 кВт·ч за 12 часов
12:01 - Прогноз на день: 25.0 кВт·ч
20:00 - Обновление: потребление 18.2 кВт·ч, прогноз: 21.8 кВт·ч
```

### Сценарий 2: Восстановление данных

```
Ситуация: Забыли внести показания за 3 дня
Решение:
1. Открыть "Исторические данные"
2. Ввести показания за каждый пропущенный день
3. Система пересчитает всю статистику
4. Проверить корректность в общих отчетах
```

### Сценарий 3: Анализ аномалий

```
Обнаружение: Потребление 45 кВт·ч в день (норма 25)
Анализ:
1. Проверить график по дням
2. Найти день с аномальным потреблением
3. Проверить комментарии в исторических данных
4. Принять меры по оптимизации
```

## 🚨 Устранение неполадок

### Проблема: Данные не сохраняются

**Симптомы:** Показания вводятся, но не отображаются в статистике

**Решения:**
1. Проверить логи Node-RED на ошибки
2. Убедиться в правильности подключения узлов
3. Проверить права доступа к файловой системе
4. Перезапустить Node-RED

```bash
# Проверка логов
tail -f ~/.node-red/logs/node-red.log

# Перезапуск Node-RED
pm2 restart node-red
# или
sudo systemctl restart nodered
```

### Проблема: Неверные расчеты потребления

**Симптомы:** Отрицательное или слишком большое потребление

**Решения:**
1. Проверить корректность показаний
2. Убедиться в правильности группировки по дням
3. Проверить временные зоны
4. Пересортировать данные по timestamp

```javascript
// Ручная пересортировка данных
let readings = global.get('readings_main_electric') || [];
readings.sort((a, b) => a.timestamp - b.timestamp);
global.set('readings_main_electric', readings);
```

### Проблема: Dashboard не отображается

**Симптомы:** Белая страница или ошибка 404

**Решения:**
1. Убедиться, что установлен node-red-dashboard
2. Проверить настройки групп и вкладок
3. Очистить кэш браузера
4. Проверить порт Node-RED

```bash
# Установка dashboard
cd ~/.node-red
npm install node-red-dashboard
```

## 🔄 Резервное копирование

### Автоматическое резервное копирование

```javascript
// Function node для автоматического бэкапа
const fs = require('fs');
const path = require('path');

const backup = {
    readings: global.get('readings_main_electric'),
    audit: global.get('historical_audit_log'),
    timestamp: new Date().toISOString()
};

const backupPath = path.join(process.env.HOME, '.node-red', 'backups');
const filename = `backup_${Date.now()}.json`;

fs.writeFileSync(path.join(backupPath, filename), JSON.stringify(backup, null, 2));
```

### Восстановление из резервной копии

```javascript
// Восстановление данных
const fs = require('fs');
const backupFile = '/path/to/backup_file.json';
const backup = JSON.parse(fs.readFileSync(backupFile, 'utf8'));

global.set('readings_main_electric', backup.readings);
global.set('historical_audit_log', backup.audit);
```

## 🔮 Планы развития

### Версия 2.0 (Q3 2025)
- 📱 Мобильное приложение
- 🤖 Машинное обучение для прогнозов
- 📧 Email/SMS уведомления
- 🌐 REST API
- 📊 Расширенная аналитика

### Версия 3.0 (Q1 2026)
- ☁️ Облачная синхронизация
- 🏠 IoT интеграция (умные счетчики)
- 👥 Многопользовательский режим
- 💰 Расчет стоимости
- 📈 Сравнение с соседями

## 🤝 Участие в проекте

### Как внести вклад

1. **Fork** проекта
2. Создайте **feature branch** (`git checkout -b feature/AmazingFeature`)
3. **Commit** изменения (`git commit -m 'Add AmazingFeature'`)
4. **Push** в branch (`git push origin feature/AmazingFeature`)
5. Откройте **Pull Request**

### Сообщение об ошибках

Используйте [GitHub Issues](https://github.com/your-username/meter-readings-system/issues) для сообщений об ошибках и предложений.

## 📄 Лицензия

Этот проект распространяется под лицензией MIT. Подробности в файле [LICENSE](LICENSE).

## 👥 Авторы

- **Wlad** - *Основной разработчик* - [GitHub](https://github.com/your-username)

## 🙏 Благодарности

- Команде Node-RED за отличную платформу
- Сообществу Node-RED за полезные палитры узлов
- Всем тестировщикам и контрибьюторам

---

## 📞 Поддержка

Если у вас есть вопросы или нужна помощь:

- 📧 Email: support@meter-system.com
- 💬 Telegram: @meter_support
- 🐛 Issues: [GitHub Issues](https://github.com/your-username/meter-readings-system/issues)
- 📖 Wiki: [Документация](https://github.com/your-username/meter-readings-system/wiki)

---

⭐ **Если проект оказался полезным, поставьте звезду на GitHub!**
