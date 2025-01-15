# Задание

Команда разработала сервис сотрудников Employee. Это внутренний сервис, который поможет централизованно работать командам разработки внутри продукта с данными пользователя. Коллеги ждут понятную документацию: они хотят разобраться в схеме работы Employee сервиса и узнать, какие методы есть в клиенте.

Разработчик сервиса рассказал тебе, как работает Employee. Тебе необходимо разобраться в сумбурном потоке мысли разработчика и создать документацию для коллег.

[Полный текст задания](https://docs.google.com/document/d/1ZW9sGcHRCy6ZVWe3B0KCqVHXLQEZ8yNSB2OooKMPbTE/edit?usp=sharing)

# Employee

Employee — это WebAPI-приложение для управления данными сотрудников. Сервис позволяет создавать, получать, обновлять и удалять информацию о сотрудниках через REST API.

Ключевые особенности Employee:

- Централизованное хранение данных. Все данные о сотруднике можно получить или обновить через сервис и они общие для всех организаций.
- Предотвращение дублирования данных между организациями.
- Возможность интеграции с другими сервисами.

> :information_source: Примечание<br>
> Все сущности в Employee привязаны к пользователю и являются общими для всех организаций, связанных с этим пользователем. Это значит, что у каждого пользователя будет собственная версия сущности одного и того же сотрудника.


# API Методы

## Person

Операции над сущностью сотрудника.

<details>
<summary><b>Создать или обновить сотрудника</b></summary>

**Запрос:**

```HTTP
PUT /person/{personGuid}
```

- `personGuid` — уникальный идентификатор сотрудника.
- Метод идемпотентный.

**Тело запроса:**

```JSON
{
  "FullName": "Semyon Semyonovich",
  "InnF1": "1234567890",
  "Gender": 1,
  "Citizenship": "RUSSIAN FEDERATION",
  "BirthDate": "01.01.2000",
  "BirthPlace": "Volgograd",
  "Phone": "+79610878544",
  "Email": "snkot@gmail.com",
  "RegistrationAddress": {
    ...
  },
  "IdentityCard": {
    "DocumentType" : "Passport",
    "Series" : "1234567890",
    "Number" : "1234567890",
    "Issuer" : "MVD",
    "IssuanceDate" : "01.01.2000",
    "IssuerCode" : "1234567890"
  },
  "ForeignAddress": "Armenia, Yerevan, Shirak St"
}
```

</details>

<details>
<summary><b>Получить данные о сотруднике</b></summary>

**Запрос:**

```HTTP
GET /person/{personGuid}
```

`personGuid` — уникальный идентификатор сотрудника.

**Ответ:**

```JSON
{
  "FullName": "Semyon Semyonovich",
  "InnF1": "1234567890",
  "Gender": 1,
  "Citizenship": "RUSSIAN FEDERATION",
  "BirthDate": "01.01.1802",
  "BirthPlace": "Volgograd",
  "Phone": "+79610878544",
  "Email": "snkot@gmail.com",
  "RegistrationAddress": {
    ...
  },
  "IdentityCard": {
    "DocumentType" : "Passport",
    "Series" : "1234567890",
    "Number" : "1234567890",
    "Issuer" : "MVD",
    "IssuanceDate" : "01.01.2000",
    "IssuerCode" : "1234567890"
  },
  "ForeignAddress": "Armenia, Yerevan, Shirak St"
}
```

</details>

<details>
<summary><b>Получить сотрудников, которых создавал пользователь</b></summary>

**Запрос:**

```HTTP
GET /person
```

**Ответ:**

```JSON
[
  {
    "Guid" : "q-12-345-w-67890",
    "FullName": "Semyon Semyonovich",
    "InnF1": "1234567890",
    "Gender": 1,
    "Citizenship": "RUSSIAN FEDERATION",
    "BirthDate": "01.01.1802",
    "BirthPlace": "Volgograd",
    "Phone": "+79610878544",
    "Email": "snkot@gmail.com",
    "RegistrationAddress": {
      ...
    },
    "IdentityCard": {
      "DocumentType" : "Passport",
      "Series" : "1234567890",
      "Number" : "1234567890",
      "Issuer" : "MVD",
      "IssuanceDate" : "01.01.2000",
      "IssuerCode" : "1234567890"
    },
    "ForeignAddress": "Armenia, Yerevan, Shirak St"
  }
]
```

</details>

<details>
<summary><b>Удалить сотрудника</b></summary>

**Запрос:**

```HTTP
DELETE /person/{personGuid}
```

`personGuid` — уникальный идентификатор сотрудника.

</details>

## Position

Методы для получения и обновления должности сотрудника.

<details>
<summary><b>Создать или обновить должность сотрудника</b></summary>

**Запрос:**

```HTTP
PUT /position/{personGuid}/{organizationGuid}
```

- `personGuid` — уникальный идентификатор сотрудника.
- `organizationGuid` — идентификатор организации, в которой нужно создать или обновить должность.
- Метод идемпотентный.

**Тело запроса:**

```JSON
{
  "Title" : "Technical Writer"
}
```

</details>

<details>
<summary><b>Получить должность сотрудника</b></summary>

**Запрос:**

```HTTP
GET /position/{personGuid}/{organizationGuid}
```

- `personGuid` — уникальный идентификатор сотрудника.
- `organizationGuid` — идентификатор организации, в которой нужно получить должность сотрудника.

**Ответ:**

```JSON
{
  "Title" : "Technical Writer"
}
```

</details>

## Role

Операции над ролями сотрудника.

<details>
<summary><b>Назначить или обновить роль сотрудника</b></summary>

**Запрос:**

```HTTP
PUT /role/{organizationGuid}
```

- `organizationGuid` — идентификатор организации, в которой нужно назначить или обновить роль.
- Метод идемпотентный.

**Тело запроса:**

```JSON
{
  "Type" : 4,
  "Person" : "q-12-345-w-67890",
  "PositionOrganization" : "q-12-345-w-67890"
}
```

</details>

<details>
<summary><b>Получить все роли сотрудника</b></summary>

**Запрос:**

```HTTP
GET /role/{personGuid}
```

`personGuid` — уникальный идентификатор сотрудника.

**Ответ:**

```JSON
[
  {
    "Organization" : "q-12-345-w-67890",
    "Type" : 4,
    "PositionOrganization" : "q-12-345-w-67890"
  }
]
```

</details>

<details>
<summary><b>Получить все роли в организации</b></summary>

**Запрос:**

```HTTP
GET /role/{organizationGuid}
```

`organizationGuid` — идентификатор организации, в которой нужно получить роли.

**Ответ:**

```JSON
[
  {
    "Type" : 4,
    "Person" : "q-12-345-w-67890",
    "PositionOrganization" : "q-12-345-w-67890"
  }
]
```

</details>

<details>
<summary><b>Получить роли, которые создавал пользователь</b></summary>

**Запрос:**

```HTTP
GET /role
```

**Ответ:**

```JSON
[
  {
    "Organization" : "q-12-345-w-67890", 
    "Type" : 4,
    "Person" : "q-12-345-w-67890",
    "PositionOrganization" : "q-12-345-w-67890"
  }
]
```

</details>

<details>
<summary><b>Удалить роль в организации</b></summary>

**Запрос:**

```HTTP
DELETE /role/{organizationGuid}/{roleType}
```

- `organizationGuid` — идентификатор организации, в которой нужно удалить роль.
- `roleType` — тип роли, которую нужно удалить.

</details>

<details>
<summary><b>Удалить все роли в организации</b></summary>

**Запрос:**

```HTTP
DELETE /role/{organizationGuid}
```

`organizationGuid` — идентификатор организации, в которой нужно удалить роли.

</details>


# Модели

## Person

Представляет отдельного сотрудника. Данные о сотрудниках общие для всех организаций в рамках одного пользователя.

**Идентификатор:** `Guid`

**Таблица:** `employee.persons`

| Поле | Тип | Описание |
| :----- | :----- | :------------- |
| Guid | Guid | Уникальный идентификатор сотрудника. |
| FullName | FullName | Имя и фамилия. |
| InnF1 | string | ИНН физического лица. |
| Gender | enum | Пол. |
| Citizenship | string | Гражданство. |
| BirthDate | DateTime | Дата рождения. |
| BirthPlace | string | Место рождения. |
| Phone | string | Номер телефона. |
| Email | string | Электронная почта. |
| RegistrationAddress | ExtendedAddress | Адрес регистрации. |
| IdentityCard | IdentityCard | Паспортные данные. |
| ForeignAddress | string | Адрес проживания за границей. |

## Position

Должность сотрудника в конкретной организации.

> :information_source: Примечание<br>
> Сотрудник может иметь только одну должность внутри одной организации.

**Ключ:** `Person Guid + Organization Guid`

**Таблица:** `employee.positions`

| Поле | Тип | Описание |
| :----- | :----- | :------------- |
| Person | Guid | Идентификатор сотрудника, которому назначена должность. |
| Organization | Guid | Идентификатор организации, в которой сотрудник назначен на должность. |
| Title | string | Название должности сотрудника в организации. |

## Role

Представляет задачу или обязанность, которую сотрудник выполняет в организации.

**Ключ:** `Organization Guid + Role Type`

**Таблица:** `employee.roles`

| Поле | Тип | Описание |
| :----- | :----- | :------------- |
| Organization | Guid | Идентификатор организации, в которой назначена роль. |
| Type | enum | Тип роли:<br>1 — руководитель.<br>2 — главный бухгалтер.<br>3 — уполномоченный представитель (отправитель).<br>4 — индивидуальный предприниматель. |
| Person | Guid | Идентификатор сотрудника, которому назначена роль в Organization. |
| PositionOrganization | Guid |  Идентификатор организации, в которой у сотрудника есть должность. Может совпадать с Organization. |


# Вопросы разработчику

- Верно, что каждый пользователь в сервисе обладает своим набором данных о сотруднике? Например, каждый из нас создаст одного и того же сотрудника Петю — у каждого будет своя сущность Пети?
- "Сущности, хранимые в сервисе сотрудников, используются для формирования отчетов и СОПа." Под "СОП" имелось в виду "стандартные операционные процедуры"?
- Зачем модели Role нужно поле PositionOrganization? Какую задачу оно решает?
- В одной организации может быть несколько ролей одного типа, например, два руководителя?
- Разработчикам важно знать, что метод идемпотентный?
- Какие значения в поле Person.Gender соответствуют каждому полу?
- Какие данные и какого типа хранит Person.RegistrationAddress?
- Как получить доступ к API? Нужна ли инструкция по подключению?