# Warehouse Cargo Tracker (WCT)
- Отслеживание грузов на складе

WCT разработан с использованием подходов DDD (Domain-Driven Design), CQRS (Command Query Responsibility Segregation) и ES (Event Sourcing).

## Домен: Склад (Warehouse)

### Поддомены

#### Поддомен приема груза (Receiving)
##### Агрегаты
- Приходный ордер (Receipt Order)

##### Процессы
- **Приемка груза (Receiving Cargo)**
    - Прием и регистрация поступающих грузов
    - Сверка соответствия поступающих грузов сведениям в приходном ордере
    - Размещение товаров на временном складе или в зоне приемки
- **Перемещение груза в зону хранения (Cargo Placement to Storage Zone)**
    - Перемещение принятых грузов из зоны приемки в основные зоны хранения
    - Идентификация мест хранения для каждого груза
- CRUD для Приходного ордера

#### Поддомен хранения (Storage)
##### Агрегаты
- Груз (Cargo)

##### Процессы
- **Перемещение грузов (Cargo Movement)**
    - Перемещение грузов между зонами хранения в зависимости от запросов приемки и заказов
    - Обновление данных о местоположении грузов на складе
- CRUD для Груза

#### Поддомен заказа (Ordering)
##### Агрегаты
- Заказ на отбор (Pick Order)

##### Процессы
- **Отбор груза (Cargo Picking)**
    - Выбор груза со склада в соответствии с составом заказа
    - Формирование списков отобранных грузов для подготовки к отгрузке
- **Перемещение груза в зону отгрузки (Cargo Transfer to Shipping Zone)**
    - Перемещение отобранных грузов в зону отгрузки для последующей подготовки к доставке
    - Подготовка документации для отгрузки грузов
- CRUD для Заказа на отбор

### Агрегаты, сущности, значения и состояния

#### Агрегаты (Aggregate)
- **Приходный ордер (Receipt Order)**
    - Идентификатор приходного ордера
    - Поставщик
    - Перевозчик
    - Дата и время приемки
    - Статус приемки
    - Список Деталей приходного ордера
- **Заказ на отбор (Pick Order)**
    - Идентификатор заказа на отбор
    - Клиент
    - Перевозчик
    - Дата размещения заказа
    - Статус отбора
    - Список деталей заказа на отбор
- **Груз (Cargo)**
    - Идентификатор груза
    - Товар
    - Упаковка
    - Количество штук Товара
    - Местоположение
    - Статус груза

#### Сущности (Entities)
- **Деталь приходного ордера (Receipt order detail)**
    - Идентификатор Детали приходного ордера
    - Идентификатор Приходного ордера
    - Идентификатор принятого груза
    - Товар
    - Упаковка
    - Количество штук Товара
    - Статус приемки Товара
- **Деталь заказа на отбор (Pick order detail)**
    - Идентификатор Детали заказа на отбор
    - Идентификатор Заказа на отбор
    - Штрихкод Товара
    - Наименование Товара
    - Количество штук Товара
    - Тип упаковки
    - Описание упаковки
    - Дата отгрузки отобранного груза
    - Статус отгрузки отобранного груза
- **Поставщик (Vendor)**
    - Идентификатор поставщика
    - Имя поставщика
- **Заказчик (Client)**
    - Идентификатор заказчика
    - Имя заказчика
- **Перевозчик (Transporter)**
    - Идентификатор перевозчика
    - Имя перевозчика

#### Значения (Value)
- **Местоположение (Location)**
    - Название зоны
    - Название ячейки
- **Товар (Product)**
    - Штрихкод
    - Наименование
    - Описание
- **Упаковка (Packaging)**
    - Тип упаковки
    - Описание
- **Тип упаковки**
    - Контейнер/Паллета/Коробка
- **Статус груза**
    - Доступен/Заблокирован
- **Статус отбора**
    - Завершен/В работе/Ожидание
- **Статус приемки**
    - Завершена/В работе/Ожидание
- **Статус приемки груза**
    - Принят/Ожидает приемки/Не принят
- **Статус отгрузки груза**
    - Отгружен/Ожидает отгрузки/Не отгружен

### Проекции в БД
- **Приходный ордер (Receipt Order)**
    - Идентификатор Приходного ордера
    - Идентификатор поставщика
    - Имя Поставщика
    - Идентификатор перевозчика
    - Имя Перевозчика
    - Дата приемки
    - Статус приемки
- **Деталь приходного ордера**
    - Идентификатор Детали приходного ордера
    - Идентификатор Приходного ордера
    - Идентификатор принятого груза
    - Штрихкод Товара
    - Наименование Товара
    - Количество штук Товара
    - Тип упаковки
    - Описание упаковки
    - Дата поступления груза на приемке
    - Статус приемки товара
- **Заказ на отбор (Pick Order)**
    - Идентификатор Заказа на отбор
    - Идентификатор клиента
    - Имя клиента
    - Идентификатор перевозчика
    - Имя Перевозчика
    - Дата размещения заказа
    - Статус отбора
- **Деталь заказа на отбор** 
    - Идентификатор Детали заказа на отбор
    - Идентификатор Заказа на отбор
    - Штрихкод Товара
    - Наименование Товара
    - Количество штук Товара
    - Тип упаковки
    - Описание упаковки
    - Идентификатор отобранного груза
    - Дата отгрузки отобранного груза
    - Статус отгрузки отобранного груза
- **Груз (Cargo)**
    - Идентификатор
    - Штрихкод товара
    - Наименование товара
    - Описание товара
    - Тип упаковки
    - Описание упаковки
    - Количество штук Товара
    - Наименование зоны
    - Наименование ячейки
    - Статус груза
