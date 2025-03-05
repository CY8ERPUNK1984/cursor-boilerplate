# Схема базы данных

В этом документе представлена схема базы данных проекта с использованием диаграмм Mermaid.js.

## Общая ER-диаграмма

```mermaid
erDiagram
    USERS ||--o{ ORDERS : "размещает"
    USERS {
        int id PK
        string email UK
        string password
        string name
        string role
        datetime created_at
        datetime updated_at
    }
    
    PRODUCTS ||--o{ ORDER_ITEMS : "содержится в"
    PRODUCTS {
        int id PK
        string name
        text description
        decimal price
        int category_id FK
        int stock_quantity
        bool is_active
        datetime created_at
        datetime updated_at
    }
    
    CATEGORIES ||--o{ PRODUCTS : "имеет"
    CATEGORIES {
        int id PK
        string name
        string slug UK
        text description
        int parent_id FK
        datetime created_at
        datetime updated_at
    }
    
    ORDERS ||--|{ ORDER_ITEMS : "содержит"
    ORDERS {
        int id PK
        int user_id FK
        string status
        decimal total_amount
        datetime order_date
        string shipping_address
        string payment_method
        string tracking_number
        datetime created_at
        datetime updated_at
    }
    
    ORDER_ITEMS {
        int id PK
        int order_id FK
        int product_id FK
        int quantity
        decimal unit_price
        decimal subtotal
        datetime created_at
        datetime updated_at
    }
    
    USERS ||--o{ USER_ADDRESSES : "имеет"
    USER_ADDRESSES {
        int id PK
        int user_id FK
        string address_line1
        string address_line2
        string city
        string state
        string postal_code
        string country
        bool is_default
        datetime created_at
        datetime updated_at
    }
    
    PRODUCTS ||--o{ PRODUCT_IMAGES : "имеет"
    PRODUCT_IMAGES {
        int id PK
        int product_id FK
        string image_url
        int sort_order
        bool is_primary
        datetime created_at
        datetime updated_at
    }
    
    USERS ||--o{ REVIEWS : "оставляет"
    PRODUCTS ||--o{ REVIEWS : "получает"
    REVIEWS {
        int id PK
        int user_id FK
        int product_id FK
        int rating
        text comment
        datetime created_at
        datetime updated_at
    }
```

## Детализация отношений

### Таблица пользователей и заказов

```mermaid
erDiagram
    USERS ||--o{ ORDERS : "размещает"
    USERS ||--o{ USER_ADDRESSES : "имеет"
    
    USERS {
        int id PK
        string email UK
        string password
        string name
        string role "enum(admin,user)"
        datetime created_at
        datetime updated_at
    }
    
    ORDERS {
        int id PK
        int user_id FK
        string status "enum(pending,paid,shipped,delivered,cancelled)"
        decimal total_amount
        datetime order_date
        string shipping_address
        string payment_method
        string tracking_number
        datetime created_at
        datetime updated_at
    }
    
    USER_ADDRESSES {
        int id PK
        int user_id FK
        string address_line1
        string address_line2
        string city
        string state
        string postal_code
        string country
        bool is_default
        datetime created_at
        datetime updated_at
    }
```

### Таблица продуктов и категорий

```mermaid
erDiagram
    CATEGORIES ||--o{ PRODUCTS : "имеет"
    PRODUCTS ||--o{ PRODUCT_IMAGES : "имеет"
    
    CATEGORIES {
        int id PK
        string name
        string slug UK
        text description
        int parent_id FK
        datetime created_at
        datetime updated_at
    }
    
    PRODUCTS {
        int id PK
        string name
        text description
        decimal price
        int category_id FK
        int stock_quantity
        bool is_active
        datetime created_at
        datetime updated_at
    }
    
    PRODUCT_IMAGES {
        int id PK
        int product_id FK
        string image_url
        int sort_order
        bool is_primary
        datetime created_at
        datetime updated_at
    }
```

### Таблица заказов и позиций заказа

```mermaid
erDiagram
    ORDERS ||--|{ ORDER_ITEMS : "содержит"
    PRODUCTS ||--o{ ORDER_ITEMS : "входит в"
    
    ORDERS {
        int id PK
        int user_id FK
        string status
        decimal total_amount
        datetime order_date
        string shipping_address
        string payment_method
        string tracking_number
        datetime created_at
        datetime updated_at
    }
    
    ORDER_ITEMS {
        int id PK
        int order_id FK
        int product_id FK
        int quantity
        decimal unit_price
        decimal subtotal
        datetime created_at
        datetime updated_at
    }
    
    PRODUCTS {
        int id PK
        string name
        text description
        decimal price
        int category_id FK
        int stock_quantity
        bool is_active
        datetime created_at
        datetime updated_at
    }
```

### Таблица отзывов

```mermaid
erDiagram
    USERS ||--o{ REVIEWS : "оставляет"
    PRODUCTS ||--o{ REVIEWS : "получает"
    
    USERS {
        int id PK
        string email UK
        string password
        string name
        string role
        datetime created_at
        datetime updated_at
    }
    
    PRODUCTS {
        int id PK
        string name
        text description
        decimal price
        int category_id FK
        int stock_quantity
        bool is_active
        datetime created_at
        datetime updated_at
    }
    
    REVIEWS {
        int id PK
        int user_id FK
        int product_id FK
        int rating
        text comment
        datetime created_at
        datetime updated_at
    }
```

## Индексы и оптимизация

Для оптимизации производительности базы данных рекомендуется создать следующие индексы:

```sql
-- Индексы для таблицы USERS
CREATE INDEX idx_users_email ON users (email);
CREATE INDEX idx_users_role ON users (role);

-- Индексы для таблицы PRODUCTS
CREATE INDEX idx_products_category_id ON products (category_id);
CREATE INDEX idx_products_is_active ON products (is_active);
CREATE INDEX idx_products_price ON products (price);

-- Индексы для таблицы CATEGORIES
CREATE INDEX idx_categories_parent_id ON categories (parent_id);
CREATE INDEX idx_categories_slug ON categories (slug);

-- Индексы для таблицы ORDERS
CREATE INDEX idx_orders_user_id ON orders (user_id);
CREATE INDEX idx_orders_status ON orders (status);
CREATE INDEX idx_orders_order_date ON orders (order_date);

-- Индексы для таблицы ORDER_ITEMS
CREATE INDEX idx_order_items_order_id ON order_items (order_id);
CREATE INDEX idx_order_items_product_id ON order_items (product_id);

-- Индексы для таблицы USER_ADDRESSES
CREATE INDEX idx_user_addresses_user_id ON user_addresses (user_id);
CREATE INDEX idx_user_addresses_is_default ON user_addresses (is_default);

-- Индексы для таблицы PRODUCT_IMAGES
CREATE INDEX idx_product_images_product_id ON product_images (product_id);
CREATE INDEX idx_product_images_is_primary ON product_images (is_primary);

-- Индексы для таблицы REVIEWS
CREATE INDEX idx_reviews_user_id ON reviews (user_id);
CREATE INDEX idx_reviews_product_id ON reviews (product_id);
CREATE INDEX idx_reviews_rating ON reviews (rating);
```

## Диаграмма потока данных

```mermaid
graph TD
    Client[Клиент] -->|Запрос| API[API Gateway]
    API -->|Запрос данных| DB[База данных]
    DB -->|Результаты запроса| API
    API -->|Ответ| Client
    
    subgraph "База данных"
        Users[Таблица Users]
        Products[Таблица Products]
        Categories[Таблица Categories]
        Orders[Таблица Orders]
        OrderItems[Таблица Order Items]
        Reviews[Таблица Reviews]
        
        Users -->|1:N| Orders
        Categories -->|1:N| Products
        Products -->|1:N| OrderItems
        Orders -->|1:N| OrderItems
        Users -->|1:N| Reviews
        Products -->|1:N| Reviews
    end
    
    classDef client fill:#42b883,stroke:#333,stroke-width:1px;
    classDef api fill:#4e79a7,stroke:#333,stroke-width:1px;
    classDef database fill:#f28e2b,stroke:#333,stroke-width:1px;
    classDef table fill:#76b7b2,stroke:#333,stroke-width:1px;
    
    class Client client;
    class API api;
    class DB database;
    class Users,Products,Categories,Orders,OrderItems,Reviews table;
```

## Миграции базы данных

Рекомендуется создать миграции для создания схемы базы данных. Миграции должны выполняться в следующем порядке:

1. Создание таблицы `users`
2. Создание таблицы `categories`
3. Создание таблицы `products`
4. Создание таблицы `orders`
5. Создание таблицы `order_items`
6. Создание таблицы `user_addresses`
7. Создание таблицы `product_images`
8. Создание таблицы `reviews`

Эта схема базы данных представляет стандартную структуру для интернет-магазина и может быть адаптирована под конкретные требования проекта. Диаграммы Mermaid.js помогают визуализировать структуру таблиц и их взаимосвязи, что упрощает понимание архитектуры базы данных для начинающих разработчиков. 