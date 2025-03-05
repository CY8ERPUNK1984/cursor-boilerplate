# Общая архитектура системы

В этом документе описана общая архитектура системы с использованием диаграмм Mermaid.js.

## Высокоуровневая архитектура

```mermaid
graph TD
    Client[Клиент/Браузер] --> Frontend[Фронтенд]
    Frontend --> API[API Gateway]
    API --> Auth[Сервис Аутентификации]
    API --> Core[Основной Сервис]
    API --> Notification[Сервис Уведомлений]
    Auth --> DB[(База данных)]
    Core --> DB
    Notification --> DB
    Notification --> Queue[Очередь сообщений]
    
    classDef frontend fill:#42b883,stroke:#333,stroke-width:1px;
    classDef backend fill:#4e79a7,stroke:#333,stroke-width:1px;
    classDef database fill:#f28e2b,stroke:#333,stroke-width:1px;
    classDef queue fill:#76b7b2,stroke:#333,stroke-width:1px;
    
    class Client,Frontend frontend;
    class API,Auth,Core,Notification backend;
    class DB database;
    class Queue queue;
```

## Детальная архитектура

### Фронтенд

```mermaid
graph TD
    App[App Component] --> Router[Router]
    Router --> Home[Главная страница]
    Router --> Auth[Страница Авторизации]
    Router --> Dashboard[Панель управления]
    
    Dashboard --> Header[Компонент Заголовок]
    Dashboard --> Sidebar[Компонент Боковая панель]
    Dashboard --> Content[Компонент Контент]
    
    Content --> DataTable[Компонент Таблица данных]
    Content --> Chart[Компонент График]
    Content --> Form[Компонент Форма]
    
    DataTable --> Store[Хранилище данных]
    Chart --> Store
    Form --> Store
    
    Store --> ApiService[Сервис API]
    
    classDef component fill:#42b883,stroke:#333,stroke-width:1px;
    classDef service fill:#78cdd7,stroke:#333,stroke-width:1px;
    
    class App,Router,Home,Auth,Dashboard,Header,Sidebar,Content,DataTable,Chart,Form component;
    class Store,ApiService service;
```

### Бэкенд

```mermaid
graph TD
    API[API Gateway] --> AuthService[Сервис Аутентификации]
    API --> CoreService[Основной Сервис]
    API --> NotificationService[Сервис Уведомлений]
    
    subgraph "Сервис Аутентификации"
        AuthController[Контроллер Auth] --> AuthLogic[Бизнес-логика Auth]
        AuthLogic --> AuthRepo[Репозиторий Auth]
        AuthRepo --> DB1[(База данных)]
    end
    
    subgraph "Основной Сервис"
        CoreController[Контроллер Core] --> CoreLogic[Бизнес-логика Core]
        CoreLogic --> CoreRepo[Репозиторий Core]
        CoreRepo --> DB2[(База данных)]
    end
    
    subgraph "Сервис Уведомлений"
        NotifController[Контроллер Notification] --> NotifLogic[Бизнес-логика Notification]
        NotifLogic --> NotifRepo[Репозиторий Notification]
        NotifRepo --> DB3[(База данных)]
        NotifLogic --> Queue[Очередь сообщений]
    end
    
    classDef gateway fill:#4e79a7,stroke:#333,stroke-width:1px;
    classDef controller fill:#a0cbe8,stroke:#333,stroke-width:1px;
    classDef logic fill:#7eb0d5,stroke:#333,stroke-width:1px;
    classDef repo fill:#b3e2cd,stroke:#333,stroke-width:1px;
    classDef database fill:#f28e2b,stroke:#333,stroke-width:1px;
    classDef queue fill:#76b7b2,stroke:#333,stroke-width:1px;
    
    class API gateway;
    class AuthController,CoreController,NotifController controller;
    class AuthLogic,CoreLogic,NotifLogic logic;
    class AuthRepo,CoreRepo,NotifRepo repo;
    class DB1,DB2,DB3 database;
    class Queue queue;
```

## Поток данных

```mermaid
sequenceDiagram
    participant Client as Клиент
    participant Frontend as Фронтенд
    participant API as API Gateway
    participant Service as Сервис
    participant DB as База данных
    
    Client->>Frontend: Действие пользователя
    Frontend->>API: HTTP запрос
    API->>Service: Перенаправление запроса
    Service->>DB: Запрос данных
    DB-->>Service: Результат запроса
    Service-->>API: Обработанный результат
    API-->>Frontend: HTTP ответ (JSON)
    Frontend-->>Client: Обновление UI
```

## Процессы развертывания

```mermaid
graph LR
    Dev[Разработка] --> Git[Git репозиторий]
    Git --> CI[CI/CD Pipeline]
    CI --> Test[Тестирование]
    Test --> Build[Сборка]
    Build --> Deploy[Развертывание]
    Deploy --> Staging[Staging среда]
    Deploy --> Prod[Production среда]
    
    classDef dev fill:#42b883,stroke:#333,stroke-width:1px;
    classDef repo fill:#4e79a7,stroke:#333,stroke-width:1px;
    classDef pipeline fill:#59a14f,stroke:#333,stroke-width:1px;
    classDef environment fill:#f28e2b,stroke:#333,stroke-width:1px;
    
    class Dev dev;
    class Git repo;
    class CI,Test,Build,Deploy pipeline;
    class Staging,Prod environment;
```

## Мониторинг и логирование

```mermaid
graph TD
    Apps[Приложения] --> Metrics[Сбор метрик]
    Apps --> Logs[Сбор логов]
    
    Metrics --> Prometheus[Prometheus]
    Logs --> ELK[ELK Stack]
    
    Prometheus --> Grafana[Grafana]
    ELK --> Kibana[Kibana]
    
    Grafana --> Alerts[Система алертов]
    Kibana --> Alerts
    
    classDef app fill:#42b883,stroke:#333,stroke-width:1px;
    classDef collector fill:#4e79a7,stroke:#333,stroke-width:1px;
    classDef storage fill:#f28e2b,stroke:#333,stroke-width:1px;
    classDef visualization fill:#76b7b2,stroke:#333,stroke-width:1px;
    classDef alerting fill:#e15759,stroke:#333,stroke-width:1px;
    
    class Apps app;
    class Metrics,Logs collector;
    class Prometheus,ELK storage;
    class Grafana,Kibana visualization;
    class Alerts alerting;
```

Эти диаграммы представляют различные аспекты архитектуры системы и могут быть адаптированы под конкретный проект. Они помогут вам, как начинающему разработчику, лучше понять структуру и взаимодействие компонентов системы. 