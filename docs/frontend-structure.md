# Структура фронтенда

В этом документе описана структура фронтенд-части проекта с использованием диаграмм Mermaid.js.

## Общая архитектура фронтенда

```mermaid
graph TD
    Client[Браузер клиента] --> App[Приложение React]
    App --> Router[React Router]
    Router --> Pages[Страницы]
    Router --> Auth[Авторизация]
    
    App --> Redux[Redux Store]
    Redux --> API[API Services]
    API --> Backend[Бэкенд API]
    
    Pages --> Components[Компоненты UI]
    Components --> Hooks[Custom Hooks]
    Components --> Redux
    
    classDef browser fill:#42b883,stroke:#333,stroke-width:1px;
    classDef core fill:#4e79a7,stroke:#333,stroke-width:1px;
    classDef ui fill:#59a14f,stroke:#333,stroke-width:1px;
    classDef state fill:#f28e2b,stroke:#333,stroke-width:1px;
    classDef logic fill:#76b7b2,stroke:#333,stroke-width:1px;
    classDef backend fill:#e15759,stroke:#333,stroke-width:1px;
    
    class Client browser;
    class App,Router core;
    class Pages,Components ui;
    class Redux state;
    class API,Hooks,Auth logic;
    class Backend backend;
```

## Структура компонентов

```mermaid
graph TD
    App[App] --> Layout[Layout]
    Layout --> Header[Header]
    Layout --> Sidebar[Sidebar]
    Layout --> Content[Content]
    Layout --> Footer[Footer]
    
    Content --> Routes[Routes]
    Routes --> HomePage[HomePage]
    Routes --> LoginPage[LoginPage]
    Routes --> RegisterPage[RegisterPage]
    Routes --> ProductsPage[ProductsPage]
    Routes --> ProductDetailPage[ProductDetailPage]
    Routes --> CartPage[CartPage]
    Routes --> CheckoutPage[CheckoutPage]
    Routes --> OrdersPage[OrdersPage]
    Routes --> ProfilePage[ProfilePage]
    Routes --> NotFoundPage[404 Page]
    
    HomePage --> FeaturedProducts[FeaturedProducts]
    HomePage --> CategoryList[CategoryList]
    HomePage --> Hero[Hero Banner]
    
    ProductsPage --> ProductList[ProductList]
    ProductsPage --> Filters[Filters]
    ProductsPage --> Pagination[Pagination]
    
    ProductDetailPage --> ProductImages[ProductImages]
    ProductDetailPage --> ProductInfo[ProductInfo]
    ProductDetailPage --> AddToCart[AddToCart]
    ProductDetailPage --> Reviews[Reviews]
    
    CartPage --> CartItems[CartItems]
    CartPage --> CartSummary[CartSummary]
    
    CheckoutPage --> ShippingForm[ShippingForm]
    CheckoutPage --> PaymentForm[PaymentForm]
    CheckoutPage --> OrderSummary[OrderSummary]
    
    ProfilePage --> UserDetails[UserDetails]
    ProfilePage --> AddressList[AddressList]
    ProfilePage --> OrderHistory[OrderHistory]
    
    classDef page fill:#4e79a7,stroke:#333,stroke-width:1px;
    classDef layout fill:#f28e2b,stroke:#333,stroke-width:1px;
    classDef component fill:#59a14f,stroke:#333,stroke-width:1px;
    
    class App,Layout,Header,Sidebar,Content,Footer layout;
    class HomePage,LoginPage,RegisterPage,ProductsPage,ProductDetailPage,CartPage,CheckoutPage,OrdersPage,ProfilePage,NotFoundPage page;
    class Routes,FeaturedProducts,CategoryList,Hero,ProductList,Filters,Pagination,ProductImages,ProductInfo,AddToCart,Reviews,CartItems,CartSummary,ShippingForm,PaymentForm,OrderSummary,UserDetails,AddressList,OrderHistory component;
```

## Структура файлов

```
/frontend
├── public/               # Статические файлы
│   ├── index.html        # Основной HTML файл
│   ├── favicon.ico       # Иконка сайта
│   └── assets/           # Статические ресурсы (изображения, шрифты)
│
├── src/                  # Исходный код приложения
│   ├── index.js          # Точка входа в приложение
│   ├── App.js            # Корневой компонент приложения
│   ├── routes/           # Настройка маршрутизации
│   │   └── index.js      # Определение маршрутов
│   │
│   ├── pages/            # Компоненты страниц
│   │   ├── HomePage/     
│   │   ├── LoginPage/
│   │   ├── RegisterPage/
│   │   ├── ProductsPage/
│   │   ├── ProductDetailPage/
│   │   ├── CartPage/
│   │   ├── CheckoutPage/
│   │   ├── OrdersPage/
│   │   ├── ProfilePage/
│   │   └── NotFoundPage/
│   │
│   ├── components/       # Многоразовые компоненты UI
│   │   ├── Layout/       # Компоненты для разметки
│   │   │   ├── Header/
│   │   │   ├── Sidebar/
│   │   │   ├── Footer/
│   │   │   └── index.js
│   │   │
│   │   ├── UI/           # Базовые компоненты UI
│   │   │   ├── Button/
│   │   │   ├── Input/
│   │   │   ├── Card/
│   │   │   ├── Modal/
│   │   │   └── ...
│   │   │
│   │   ├── Products/     # Компоненты для работы с продуктами
│   │   │   ├── ProductList/
│   │   │   ├── ProductCard/
│   │   │   ├── ProductFilters/
│   │   │   └── ...
│   │   │
│   │   ├── Cart/         # Компоненты для корзины
│   │   │   ├── CartItem/
│   │   │   ├── CartSummary/
│   │   │   └── ...
│   │   │
│   │   └── User/         # Компоненты для пользователя
│   │       ├── LoginForm/
│   │       ├── RegisterForm/
│   │       └── ...
│   │
│   ├── hooks/            # Пользовательские React хуки
│   │   ├── useAuth.js
│   │   ├── useCart.js
│   │   ├── useForm.js
│   │   └── ...
│   │
│   ├── store/            # Redux store
│   │   ├── index.js      # Конфигурация store
│   │   ├── actions/      # Action creators
│   │   │   ├── authActions.js
│   │   │   ├── productActions.js
│   │   │   ├── cartActions.js
│   │   │   └── ...
│   │   │
│   │   ├── reducers/     # Reducers
│   │   │   ├── authReducer.js
│   │   │   ├── productReducer.js
│   │   │   ├── cartReducer.js
│   │   │   └── ...
│   │   │
│   │   └── types.js      # Константы типов действий
│   │
│   ├── services/         # Сервисы API
│   │   ├── api.js        # Базовая настройка axios
│   │   ├── authService.js
│   │   ├── productService.js
│   │   ├── orderService.js
│   │   └── ...
│   │
│   ├── utils/            # Вспомогательные функции
│   │   ├── helpers.js
│   │   ├── validation.js
│   │   ├── formatting.js
│   │   └── ...
│   │
│   ├── assets/           # Локальные ресурсы
│   │   ├── images/
│   │   ├── styles/
│   │   └── ...
│   │
│   └── constants/        # Константы приложения
│       ├── routes.js
│       ├── api.js
│       └── ...
│
├── tests/                # Тесты
│   ├── unit/             # Модульные тесты
│   ├── integration/      # Интеграционные тесты
│   └── e2e/              # End-to-end тесты
│
├── .env                  # Переменные окружения
├── .env.development      # Переменные для разработки
├── .env.production       # Переменные для продакшна
├── .eslintrc             # Конфигурация ESLint
├── .prettierrc           # Конфигурация Prettier
├── package.json          # Зависимости и скрипты
└── README.md             # Документация фронтенда
```

## Поток данных (Redux Flow)

```mermaid
sequenceDiagram
    participant User as Пользователь
    participant Component as React компонент
    participant Action as Action Creator
    participant API as API Service
    participant Reducer as Redux Reducer
    participant Store as Redux Store
    
    User->>Component: Взаимодействие (клик, ввод)
    Component->>Action: Вызов action creator
    Action->>API: Запрос к API (если нужен)
    API-->>Action: Ответ от API
    Action->>Reducer: Dispatch действия с payload
    Reducer->>Store: Обновление состояния
    Store-->>Component: Получение обновленного состояния
    Component-->>User: Обновление интерфейса
```

## Авторизация и защита маршрутов

```mermaid
graph TD
    Start[Запрос к защищенному маршруту] --> Check{Проверка наличия токена}
    Check -->|Токен есть| ValidateToken{Проверка валидности токена}
    Check -->|Токен отсутствует| RedirectLogin[Перенаправление на страницу входа]
    
    ValidateToken -->|Токен валиден| AccessGranted[Доступ разрешен]
    ValidateToken -->|Токен просрочен| RefreshToken[Попытка обновить токен]
    
    RefreshToken --> RefreshCheck{Успешно?}
    RefreshCheck -->|Да| AccessGranted
    RefreshCheck -->|Нет| RedirectLogin
    
    classDef start fill:#42b883,stroke:#333,stroke-width:1px;
    classDef check fill:#4e79a7,stroke:#333,stroke-width:1px;
    classDef success fill:#59a14f,stroke:#333,stroke-width:1px;
    classDef error fill:#e15759,stroke:#333,stroke-width:1px;
    
    class Start start;
    class Check,ValidateToken,RefreshCheck check;
    class AccessGranted success;
    class RedirectLogin error;
```

## Управление состоянием загрузки и ошибками

```mermaid
stateDiagram-v2
    [*] --> Idle: Инициализация
    Idle --> Loading: Начало запроса
    Loading --> Success: Успешное выполнение
    Loading --> Error: Ошибка
    Success --> Idle: Сброс состояния
    Error --> Idle: Сброс состояния
    
    state Loading {
        [*] --> FetchingData
        FetchingData --> ProcessingData
        ProcessingData --> [*]
    }
    
    state Error {
        [*] --> NetworkError
        [*] --> ValidationError
        [*] --> ServerError
    }
```

## Обработка форм

```mermaid
graph TD
    FormInit[Инициализация формы] --> FormValues[Начальные значения]
    FormInit --> FormValidation[Валидация]
    
    FormValues --> HandleChange[Обработка изменений]
    HandleChange --> FormValidation
    
    FormValidation --> IsValid{Данные валидны?}
    IsValid -->|Да| HandleSubmit[Отправка формы]
    IsValid -->|Нет| ShowErrors[Отображение ошибок]
    
    HandleSubmit --> SubmitAPI[Отправка на API]
    SubmitAPI --> SubmitResult{Успешно?}
    SubmitResult -->|Да| ShowSuccess[Сообщение об успехе]
    SubmitResult -->|Нет| ShowSubmitErrors[Отображение ошибок сервера]
    
    classDef init fill:#42b883,stroke:#333,stroke-width:1px;
    classDef process fill:#4e79a7,stroke:#333,stroke-width:1px;
    classDef decision fill:#f28e2b,stroke:#333,stroke-width:1px;
    classDef success fill:#59a14f,stroke:#333,stroke-width:1px;
    classDef error fill:#e15759,stroke:#333,stroke-width:1px;
    
    class FormInit,FormValues init;
    class HandleChange,FormValidation,HandleSubmit,SubmitAPI process;
    class IsValid,SubmitResult decision;
    class ShowSuccess success;
    class ShowErrors,ShowSubmitErrors error;
```

## Оптимизация производительности

Для оптимизации производительности фронтенд-приложения рекомендуется использовать следующие подходы:

1. **Code Splitting** - разделение кода на более мелкие части для загрузки по требованию:

```mermaid
graph TD
    App[App] -->|Базовый бандл| MainBundle[Основной бандл]
    App -->|Ленивая загрузка| LazyRoutes[Lazy Routes]
    
    LazyRoutes -->|import()| Admin[Админ-панель]
    LazyRoutes -->|import()| ProductDetail[Детальная страница продукта]
    LazyRoutes -->|import()| Checkout[Оформление заказа]
    
    classDef main fill:#42b883,stroke:#333,stroke-width:1px;
    classDef lazy fill:#4e79a7,stroke:#333,stroke-width:1px;
    classDef chunk fill:#f28e2b,stroke:#333,stroke-width:1px;
    
    class App,MainBundle main;
    class LazyRoutes lazy;
    class Admin,ProductDetail,Checkout chunk;
```

2. **Memoization** - использование React.memo, useMemo и useCallback для предотвращения лишних рендеров.
3. **Virtualization** - для длинных списков использовать виртуализацию с помощью библиотек типа react-window.
4. **Progressive Loading** - загрузка изображений и других ресурсов постепенно или по мере необходимости.

Эта документация поможет начинающему разработчику понять структуру фронтенд-приложения, его компоненты и взаимодействие между ними. Диаграммы Mermaid.js визуально представляют поток данных и архитектуру, что упрощает понимание сложных концепций. 