## Задача 1:

### **User Story**:

Я как клиент банка, хочу иметь возможность обновлять информацию о своем профиле, включая имя, фамилию, возраст, электронную почту и номер телефона, чтобы поддерживать актуальные данные.

### **Функциональные требования**:
    
    Реализовать функционал для возможности редактирования пользовательского профиля.
        1. Создать валидацию введенных данных в полях почтовый адрес и номер телефона
        2. Создать возможность сохранения обновленных данных в профиле

### **Ограничения на UI**:

        1. Доступные поля для редактирования: имя, фамилия, возраст, электронная почта и номер телефона
        2. Недоступные поля для редактирования будут выделены серым цветам

### **Use Case**:
    Редактирование информации в профиле клиента:

    Краткое описание: У пользователя должна быть возможность отредактировать данные в профиле приложения банка
    Действующие лица: Пользователь банка, приложение банка (веб, мобильное), сервис клиентов
    Триггер: У пользователя обновились контактные данные
    Предусловие: Пользователь авторизовался в приложении
    Постусловие: ---
    Результат: В случае успешного выполнения основного потока, внесенные изменения сохранены в БД сервиса, пользователь увидит изменения в приложении

    Основной поток: Внести изменения в профиле клиента
        1. Пользователь перешел к функционалу просмотра и внесения изменений в профиле приложения
        2. Пользователь отредактировал доступные поля для редактирования
        3. Пользователь выбирает "Сохранить" изменения 
        5. Система сохраняет изменения в БД сервиса клиентов
        6. Система отображет нотификацию "Изменения сохранены"
    
    Альтернативный поток 1: Пользователь не подтвердил сохранение изменений
        2а. Пользователь отредактировал данные в поле 
        3а. Пользователь не сохранил изменения
        4а. Система закрывает режим редактирования строки, внесенные изменения не сохраняются

    Альтернативный поток 2: Пользователь отменил сохранение изменений
        2а. Пользователь отредактировал данные в поле 
        3а. Пользователь отменил изменения
        4а. Система закрывает режим редактирования строки, внесенные изменения не сохраняются

    Поток исключения 1: Пользователь не заполнил все обязательные поля
        2а. Пользователь отредактировал данные в поле, но оставил хотя бы одно поле пустым
        3а. Система выделяет инпут красным и отображает подпись "Обязательное поле" 

    Требования: Перечень полей доступных для редактирования
        - Имя
        - Фамилию
        - Возраст
        - Электронная почта
        - Номер телефона

### **Интеграция Rest API**:

Эндпоинт для обновления профиля - ***PATCH api/v1/user/{user_id}***

Полная схема json:
```json
{
    "userId": "string",
    "firstName": "string",
    "lastName": "string",
    "age": "int",
    "emailAddress": "string",
    "phoneNumber": "string"
}
```

1.1. Пример запроса в случае изменения имени:
```json
{
    "userId": "f1731421-f97b-4343-b076-a7460131cf3f",
    "firstName": "Ivan"
}
```

1.2. Пример ответа:
```json
{
    "message": "Данные успешно изменены"
}
```

2.1. Пример запроса в случае изменения имени и фамилии:
```json
{
    "userId": "f1731421-f97b-4343-b076-a7460131cf3f",
    "firstName": "Ivan",
    "lastName": "Ivanov"
}
```

2.2. Пример ответа:
```json
{
    "message": "Данные успешно изменены"
}
```

### **Модель данных в БД**:

Данные о клиенте (таблица Users)

| Поле  | Наименвоание | Тип данных  | Кратность |
| ------------- | ------------- | ------------- | ------------- |
| `userId`  | Идентификатор клиента  | uuid  | 1..1  |
| `firstName`  | Имя клиента  | nvarchar(250)  | 1..1  |
| `lastName`  | Фамилия клиента  | nvarchar(250)  | 1..1  |
| `age`  | Возраст  | int  | 1..1  |
| `emailAddress`  | Почтовый адрес  | nvarchar(500)  | 1..1  |
| `phoneNumber`  | Номер телефона  | nvarchar(25)  | 1..1  |
| `createdAt`  | Дата и время создания записи  | datetime  | 1..1  |
| `modifiedAt`  | Дата и время обновления записи  | datetime  | 1..1  |

### **Диаграмма последовательности**:

![sequence_diagram_ser_profile](user_profile.png)