# ТЗ на страницу оформления и покупки билета для front-end

## 1. Макеты страницы/функции/фичи

![Оформление билета](./Снимок%20экрана%202024-10-15%20150308.png)
![Покупка билета](./Снимок%20экрана%202024-10-15%20153821.png)


## 2. SEO

URL страницы: /purchase-ticket

Хлебные крошки: Главная > Выбор рейса > Оформление билета > Покупка билета

## 3. JSON при инициализации

```json
{
  "flightInfo": {
    "departureCity": "Москва",
    "arrivalCity": "Астрахань",
    "departureDate": "18 окт",
    "arrivalDate": "21 нояб",
    "flightNumbers": ["16:30 SVO", "22:05 ASF"]
  },
  "price": 13272,
  "passengerInfo": {
    "firstName": "",
    "lastName": "",
    "middleName": "",
    "documentType": "Российский паспорт",
    "documentNumber": "",
    "birthDate": "",
    "gender": ""
  },
  "contactInfo": {
    "email": "",
    "phone": ""
  },
  "luggage": {
    "handLuggage": {
      "included": true,
      "quantity": 1,
      "weight": 10
    },
    "checkedBaggage": {
      "included": false,
      "quantity": 1,
      "weight": 23,
      "price": 6750
    }
  }
}
```

## 4. Маппинг данных

### 4.1 Форма оформления билета

Скриншот элемента: ![Изображение формы оформления билета](./Снимок%20экрана%202024-10-15%20150308.png)

Условие отображения: Всегда

Описание элемента:

| Элемент | Тип элемента | Поле из JSON | Примечание |
|---------|--------------|--------------|------------|
| Заголовок "Оформление билета" | Текст | - | Статический текст |
| Подзаголовок | Текст | - | "Билет почти ваш! Осталось заполнить данные и оплатить — это быстро" |
| Поле "Адрес электронной почты" | Input, обязательное | contactInfo.email | Валидация формата email |
| Поле "Контактный телефон" | Input, обязательное | contactInfo.phone | Маска ввода телефона |
| Заголовок "1-й пассажир, взрослый" | Текст | - | Статический текст |
| Выпадающий список "Страна" | Select | - | По умолчанию "Россия" |
| Выпадающий список "Тип документа" | Select | passengerInfo.documentType | По умолчанию "Российский паспорт" |
| Поле "Фамилия" | Input, обязательное | passengerInfo.lastName | - |
| Поле "Имя" | Input, обязательное | passengerInfo.firstName | - |
| Поле "Отчество" | Input | passengerInfo.middleName | Необязательное поле |
| Поле "Дата рождения" | Date Input, обязательное | passengerInfo.birthDate | Календарь для выбора даты |
| Поле "№ Документа" | Input, обязательное | passengerInfo.documentNumber | - |
| Переключатель пола | Toggle | passengerInfo.gender | Выбор между М/Ж |
| Чекбокс "Ручная кладь" | Checkbox | luggage.handLuggage.included | По умолчанию включен, нередактируемый |
| Информация о ручной клади | Текст | luggage.handLuggage | "1 место до 10 кг" |
| Чекбокс "Багаж" | Checkbox | luggage.checkedBaggage.included | По умолчанию выключен |
| Информация о багаже | Текст | luggage.checkedBaggage | "1 место до 23 кг" |
| Стоимость багажа | Текст | luggage.checkedBaggage.price | "6 750 ₽" |
| Кнопка "Добавить" (багаж) | Button | - | При нажатии добавляет багаж |
| Информация о рейсе | Текст | flightInfo | Отображение информации о рейсе |
| Итоговая стоимость | Текст | price | Отображение итоговой стоимости |
| Кнопка "Далее" | Button | - | При нажатии переход к оплате |
| Кнопка "Добавить ещё пассажиров" | Button | - | При нажатии добавляет форму для нового пассажира |

### 4.2 Форма оплаты билета

Скриншот элемента: ![Изображение с формой оплаты](./Снимок%20экрана%202024-10-15%20153821.png)

Условие отображения: После заполнения формы оформления билета

Описание элемента:

| Элемент | Тип элемента | Поле из JSON | Примечание |
|---------|--------------|--------------|------------|
| Заголовок "Осталось только оплатить" | Текст | - | Статический текст |
| Радио-кнопка "Система быстрых платежей" | Radio button | - | Опция способа оплаты |
| Радио-кнопка "Карта" | Radio button | - | Опция способа оплаты, по умолчанию выбрана |
| Поле "Номер карты" | Input | - | Появляется при выборе оплаты картой |
| Поле "Имя на карте" | Input | - | Появляется при выборе оплаты картой |
| Поле "Срок до" | Input | - | Появляется при выборе оплаты картой |
| Поле "CVC" | Input | - | Появляется при выборе оплаты картой |
| Информация о маршруте | Текст | flightInfo | Отображение краткой информации о рейсе |
| Информация о пассажире | Текст | passengerInfo | Отображение имени пассажира |
| Сумма к оплате | Текст | price | Отображение итоговой стоимости |
| Кнопка "Купить" | Button | - | При нажатии происходит оплата |

## 5. Действия на странице/в функции

| Действие | Формат вызова функции | Эндпоинт | Примечание |
|----------|------------------------|----------|------------|
| Заполнение формы оформления билета | Ввод данных в форму | - | Валидация полей при вводе |
| Добавление багажа | Клик по кнопке "Добавить" | POST api/add-baggage | Обновление стоимости и данных о багаже |
| Добавление пассажира | Клик по кнопке "Добавить ещё пассажиров" | - | Добавление новой формы пассажира на страницу |
| Переход к оплате | Клик по кнопке "Далее" | POST api/validate-booking | Отправка данных формы для валидации |

