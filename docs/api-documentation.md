# Документация API

В этом документе описаны основные API-эндпоинты и их взаимодействие с компонентами системы.

## Обзор API

```mermaid
graph LR
    Client[Клиент]
    Auth[/api/auth/*]
    Users[/api/users/*]
    Products[/api/products/*]
    Orders[/api/orders/*]
    AuthService[Сервис Аутентификации]
    UserService[Сервис Пользователей]
    ProductService[Сервис Продуктов]
    OrderService[Сервис Заказов]
    DB[(База данных)]
    NotificationService[Сервис Уведомлений]
    
    Client --> Auth
    Client --> Users
    Client --> Products
    Client --> Orders
    
    Auth --> AuthService
    Users --> UserService
    Products --> ProductService
    Orders --> OrderService
    
    AuthService --> DB
    UserService --> DB
    ProductService --> DB
    OrderService --> DB
    OrderService --> NotificationService
    
    classDef client fill:#42b883,stroke:#333,stroke-width:1px;
    classDef endpoint fill:#4e79a7,stroke:#333,stroke-width:1px;
    classDef service fill:#59a14f,stroke:#333,stroke-width:1px;
    classDef database fill:#f28e2b,stroke:#333,stroke-width:1px;
    
    class Client client;
    class Auth,Users,Products,Orders endpoint;
    class AuthService,UserService,ProductService,OrderService,NotificationService service;
    class DB database;
```

## Диаграмма последовательности авторизации

```mermaid
sequenceDiagram
    participant Client as Клиент
    participant API as API Gateway
    participant Auth as Сервис авторизации
    participant DB as База данных
    
    Client->>API: POST /api/auth/login
    Note right of Client: {email, password}
    API->>Auth: Передача данных авторизации
    Auth->>DB: Проверка учетных данных
    DB-->>Auth: Данные пользователя
    
    alt Успешная авторизация
        Auth-->>API: Пользователь найден + JWT токены
        API-->>Client: 200 OK + {access_token, refresh_token, user}
    else Ошибка авторизации
        Auth-->>API: Ошибка авторизации
        API-->>Client: 401 Unauthorized
    end
```

## Диаграмма последовательности получения данных

```mermaid
sequenceDiagram
    participant Client as Клиент
    participant API as API Gateway
    participant Auth as Middleware авторизации
    participant Service as Сервис
    participant DB as База данных
    
    Client->>API: GET /api/resource
    Note right of Client: Headers: {Authorization: Bearer {token}}
    API->>Auth: Проверка JWT токена
    
    alt Валидный токен
        Auth-->>API: Токен валиден
        API->>Service: Запрос данных
        Service->>DB: Запрос к базе данных
        DB-->>Service: Данные
        Service-->>API: Обработанные данные
        API-->>Client: 200 OK + данные
    else Невалидный токен
        Auth-->>API: Токен невалиден
        API-->>Client: 401 Unauthorized
    end
```

## Диаграмма последовательности создания ресурса

```mermaid
sequenceDiagram
    participant Client as Клиент
    participant API as API Gateway
    participant Auth as Middleware авторизации
    participant Service as Сервис
    participant DB as База данных
    participant Notify as Сервис уведомлений
    
    Client->>API: POST /api/resource
    Note right of Client: Headers: {Authorization: Bearer {token}}<br>Body: {данные ресурса}
    API->>Auth: Проверка JWT токена и прав доступа
    
    alt Доступ разрешен
        Auth-->>API: Доступ разрешен
        API->>Service: Передача данных
        Service->>DB: Сохранение в базе данных
        DB-->>Service: ID созданного ресурса
        Service->>Notify: Уведомление о создании
        Service-->>API: Данные созданного ресурса
        API-->>Client: 201 Created + данные
    else Доступ запрещен
        Auth-->>API: Доступ запрещен
        API-->>Client: 403 Forbidden
    end
```

## Структура API эндпоинтов

### Аутентификация

```mermaid
graph TD
    Auth[/api/auth] --> Register[POST /register]
    Auth --> Login[POST /login]
    Auth --> Refresh[POST /refresh]
    Auth --> Logout[POST /logout]
    Auth --> Me[GET /me]
    
    classDef endpoint fill:#4e79a7,stroke:#333,stroke-width:1px;
    class Auth,Register,Login,Refresh,Logout,Me endpoint;
```

### Пользователи

```mermaid
graph TD
    Users[/api/users] --> GetAll[GET /]
    Users --> GetById[GET /{id}]
    Users --> Create[POST /]
    Users --> Update[PUT /{id}]
    Users --> Delete[DELETE /{id}]
    
    classDef endpoint fill:#4e79a7,stroke:#333,stroke-width:1px;
    class Users,GetAll,GetById,Create,Update,Delete endpoint;
```

### Продукты

```mermaid
graph TD
    Products[/api/products] --> GetAll[GET /]
    Products --> GetById[GET /{id}]
    Products --> Create[POST /]
    Products --> Update[PUT /{id}]
    Products --> Delete[DELETE /{id}]
    
    classDef endpoint fill:#4e79a7,stroke:#333,stroke-width:1px;
    class Products,GetAll,GetById,Create,Update,Delete endpoint;
```

### Заказы

```mermaid
graph TD
    Orders[/api/orders] --> GetAll[GET /]
    Orders --> GetById[GET /{id}]
    Orders --> Create[POST /]
    Orders --> Update[PUT /{id}]
    Orders --> Delete[DELETE /{id}]
    Orders --> GetByUser[GET /user/{userId}]
    
    classDef endpoint fill:#4e79a7,stroke:#333,stroke-width:1px;
    class Orders,GetAll,GetById,Create,Update,Delete,GetByUser endpoint;
```

## Форматы запросов и ответов

### Пример запроса авторизации

```json
POST /api/auth/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "password123"
}
```

### Пример ответа авторизации

```json
200 OK
Content-Type: application/json

{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": 1,
    "email": "user@example.com",
    "name": "John Doe",
    "role": "user"
  }
}
```

### Пример запроса создания ресурса

```json
POST /api/products
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...

{
  "name": "Product Name",
  "description": "Product Description",
  "price": 99.99,
  "category_id": 1
}
```

### Пример ответа создания ресурса

```json
201 Created
Content-Type: application/json

{
  "id": 42,
  "name": "Product Name",
  "description": "Product Description",
  "price": 99.99,
  "category_id": 1,
  "created_at": "2023-06-15T14:30:00Z"
}
```

## Обработка ошибок

```mermaid
graph TD
    Errors[Обработка ошибок] --> E400[400 Bad Request]
    Errors --> E401[401 Unauthorized]
    Errors --> E403[403 Forbidden]
    Errors --> E404[404 Not Found]
    Errors --> E422[422 Unprocessable Entity]
    Errors --> E500[500 Internal Server Error]
    
    E400 --> BadRequest["Невалидные данные<br>{message, errors}"]
    E401 --> Unauthorized["Пользователь не авторизован<br>{message}"]
    E403 --> Forbidden["Нет прав доступа<br>{message}"]
    E404 --> NotFound["Ресурс не найден<br>{message}"]
    E422 --> ValidationError["Ошибка валидации<br>{message, errors}"]
    E500 --> ServerError["Ошибка сервера<br>{message}"]
    
    classDef error fill:#e15759,stroke:#333,stroke-width:1px;
    classDef response fill:#76b7b2,stroke:#333,stroke-width:1px;
    
    class Errors,E400,E401,E403,E404,E422,E500 error;
    class BadRequest,Unauthorized,Forbidden,NotFound,ValidationError,ServerError response;
```

### Пример ответа с ошибкой валидации

```json
422 Unprocessable Entity
Content-Type: application/json

{
  "message": "Validation failed",
  "errors": {
    "email": ["Email is required", "Email must be valid"],
    "password": ["Password is required", "Password must be at least 8 characters"]
  }
}
```

Эта документация API поможет вам как начинающему разработчику лучше понять структуру API, форматы запросов и ответов, а также процессы обработки ошибок. Диаграммы Mermaid.js наглядно показывают взаимодействие между различными компонентами системы. 