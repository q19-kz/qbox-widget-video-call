# qbox-widget-video-call

# Шаг 1. Добавление виджета (frontend)

Добавить JavaScript библиотеку в ```<body></body>```. Запуск виджета в зависимости от требований бизнес-логики

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Video call sample</title>

  <style>
    html, body {
      margin: 0;
      padding: 0;
      height: 100%;
    }
  </style>
</head>
<body>

  <!-- Подключение JavaScript скрипта -->
  <script src="https://<domain>/static/js/wc-video-call.min.js"></script>  

  <!-- Пустой контейнер для будущего виджета с видеозвонком -->
  <div id="video-call-wrapper"></div>
  
  <!-- Запуск виджета с видеозвонком -->
  <script>
    let videoCallElem = document.createElement("wc-video-call");  // Обязательное название 'wc-video-call'

    // Основные
    videoCallElem.setAttribute("host-name", "https://<domain>");  // Обязательное поле
    videoCallElem.setAttribute("domain", "<domain>");  // Идентификатор интеграционной системы
    // videoCallElem.setAttribute("environment", "<environment>");  // Среда разработки (пример, dev или prod)
    // videoCallElem.setAttribute("extra", JSON.stringify({"foo": "bar"});  // Динамические параметры

    // Машрутизация звонка
    videoCallElem.setAttribute("topic", "test");  // Тема звонка для маршрутизации между операторами
    // videoCallElem.setAttribute("target-operator-id", 1);  // ID сотрудника в системе для направленного звонка к конкретному оператору (без передачи этого параметра звонок будет маршрутизирован рандомно к свободному оператору)

    // Идентификация клиента
    // videoCallElem.setAttribute("user-id", 1);  // ID абонента (клиента) в системе, от кого совершается звонок (без передачи этого параметра звонок будет совершаться анонимно)

    // UI
    // videoCallElem.setAttribute("height", "100vh");  // Высота страницы виджета (по умолчанию = "100vh")
    // videoCallElem.setAttribute("head-title", "<head-title>");  // Название вкладки
    // videoCallElem.setAttribute("main-title", "<main-title>");  // Название секции на главной странице звонка
    // videoCallElem.setAttribute("main-description", "<main-description>");  // Описание секции на главной странице звонка
    // videoCallElem.setAttribute("readiness-check-text", "<readiness-check-text>");  // Текст предупреждающего блока на главной странице звонка
    // videoCallElem.setAttribute("request-call-feedback", 0);  // Запрос отзыва о звонке после завершения (0 = не спрашивать, 1 = спросить; по умолчанию = 1)
    // videoCallElem.setAttribute("end-screen-new-call-button", 0);  // Показ кнопки "Новый звонок" на странице завершенного звонка (0 = скрыть, 1 = показать; по умолчанию = 0)
    
    document.getElementById("video-call-wrapper").appendChild(videoCallElem);
  </script>  
  
</body>
</html>
```

# Шаг 2. Создание клиента (backend)

> при наличии клиента по указанным критериям выполнится только обновление данных и возврат ID клиента (upsert)

```
POST https://<domain>/bridge/api/v1/customers
```

Headers:
```json
{
    "X-API-Secret": "abcdefghijklmnopq"
}
```

JSON Body:
```json
{
    "first_name": "<Имя>",
    "last_name": "<Фамилия>",
    "patronymic": "<Отчество>",
    "sex": "<Пол: мужчина = 0, женщина = 1>",
    "iin": "<ИИН>",
    "email": "<Электронная почта>",
    "phone": "<Номер телефона в формате 77771234567>",
    "birthdate": "<День рождения в формате DD.MM.YYYY>",
}
```

* first_name, last_name, iin, phone являются обязательными параметрами, остальные опциональные

Ответ:
```json
{
    "data": {
        "customer": {
            "id": "<ID клиента в системе>"
        }
    },
    "_success": true
}
```

# Шаг 3. Запуск виджета (frontend)

- Полученное ID клиента из [Шаг 2]() передать в качестве ```user-id``` как указано в [Шаг 1]()
- Запуск
