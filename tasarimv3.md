# Kafeterya Sipariş Sistemi - Tasarım Dökümanı

## 1. Giriş

Bu döküman, "Kafeterya Sipariş Sistemi" (Cafeteria Ordering System - COS) için yazılım tasarımını detaylandırmaktadır. Döküman, **`2025-11-13-BIDB-CafeteriaOrdering-RequirementsDefinition-V04.md`** numaralı gereksinim analiz raporuna dayanarak hazırlanmıştır. Amacı, geliştirme ekibine yol gösterecek teknik mimariyi, API tasarımını, veri modelini ve temel uygulama bileşenlerini tanımlamaktır.

## 2. Mimari Yaklaşım

Sistem, modern web teknolojileri kullanılarak **Client-Server (İstemci-Sunucu)** mimarisine uygun olarak geliştirilecektir. Bu mimari, sorumlulukların net bir şekilde ayrılmasını sağlayarak esneklik ve ölçeklenebilirlik sunar.

- **Frontend (İstemci):** Kullanıcı arayüzü, **React** kütüphanesi kullanılarak geliştirilecektir. Bu, dinamik ve etkileşimli bir kullanıcı deneyimi sağlar. Bileşen tabanlı yapı, kodun yeniden kullanılabilirliğini artırır.
- **Backend (Sunucu):** Sunucu tarafı iş mantığı, **Node.js** ve **Express.js** framework'ü ile geliştirilecektir. Bu platform, asenkron yapısı sayesinde yüksek performanslı I/O işlemleri sunar. API, RESTful prensiplerine uygun olarak tasarlanacaktır.
- **Veritabanı:** Veri kalıcılığı için ilişkisel bir veritabanı olan **PostgreSQL** tercih edilmiştir. PostgreSQL, veri bütünlüğü, güvenilirlik ve geniş özellik seti ile bu proje için uygun bir seçimdir.
- **Katmanlı Mimari (Backend):** Sunucu uygulaması, sorumlulukları ayırmak için katmanlı bir yapıya sahip olacaktır:
    - **Controllers (Kontrolcüler):** HTTP isteklerini karşılar, verileri doğrular ve ilgili servislere yönlendirir.
    - **Services (Servisler):** Ana iş mantığını içerir. Gerekli hesaplamaları, validasyonları ve işlemleri yürütür.
    - **Repositories (Depolar):** Veritabanı sorgularını yönetir ve veri erişim mantığını soyutlar.
    - **Models (Modeller):** Veritabanı tablolarını temsil eden veri yapılarıdır (örn: Sequelize modelleri).

## 3. Veri Modeli Tasarımı (ER Diyagramı)

Veritabanı, sistemdeki temel varlıkları ve aralarındaki ilişkileri yansıtacak şekilde tasarlanmıştır. Gereksinim dökümanındaki mantıksal model burada fiziksel bir ER diyagramı olarak detaylandırılmıştır.

```mermaid
erDiagram
    "User" {
        int id PK
        varchar name
        varchar email
        varchar password_hash
        varchar role
    }

    "MealOrder" {
        int id PK
        int user_id FK
        datetime order_date
        varchar status
        decimal total_price
        datetime delivery_time
        varchar delivery_location
    }

    "MealPayment" {
        int id PK
        int order_id FK
        varchar payment_method
        datetime payment_date
        varchar status
    }

    "OrderItem" {
        int id PK
        int order_id FK
        int menu_item_id FK
        int quantity
        decimal price_per_item
    }

    "MenuItem" {
        int id PK
        varchar name
        varchar description
        decimal price
        boolean is_available
    }

    "Menu" {
        int id PK
        date menu_date
    }

    "Menu_MenuItem" {
        int menu_id FK
        int menu_item_id FK
    }

    "User" ||--o{ "MealOrder" : "places"
    "MealOrder" ||--|| "MealPayment" : "has"
    "MealOrder" }o--|| "OrderItem" : "contains"
    "MenuItem" }o--|| "OrderItem" : "is"
    "Menu" }o--o{ "Menu_MenuItem" : "has"
    "MenuItem" }o--o{ "Menu_MenuItem" : "belongs to"

```
**Açıklamalar:**
- **User:** Sisteme kayıtlı kullanıcıları (Müşteri, Menü Yöneticisi vb.) temsil eder. Rol tabanlı yetkilendirme için `role` alanı içerir.
- **MealOrder:** Bir kullanıcının verdiği tek bir siparişi temsil eder.
- **MealPayment:** Siparişe ait ödeme bilgilerini tutar. `MealOrder` ile bire-bir ilişkilidir.
- **MenuItem:** Menüde yer alan her bir yemek seçeneğidir.
- **OrderItem:** Bir siparişin içinde hangi `MenuItem`'dan kaç adet olduğunu belirtir. `MealOrder` ve `MenuItem` arasında bir "çok-a-çok" ilişki kurar.
- **Menu:** Belirli bir tarihte sunulan yemek listesini gruplamak için kullanılır.
- **Menu_MenuItem:** Hangi yemeğin hangi günün menüsünde olduğunu belirten bir ara tablodur.

## 4. API Uç Noktaları Tasarımı (RESTful API)

Backend sunucusu, frontend ile iletişim kurmak için aşağıdaki RESTful API uç noktalarını sunacaktır.

| Method | Endpoint | Açıklama | Yetki |
| :--- | :--- | :--- | :--- |
| `POST` | `/api/auth/register` | Yeni kullanıcı kaydı oluşturur. | Herkese Açık |
| `POST` | `/api/auth/login` | Kullanıcı girişi yapar ve token döner. | Herkese Açık |
| `GET` | `/api/menu` | Belirli bir tarihteki menüyü getirir. (`?date=...`) | Müşteri |
| `POST`| `/api/menu` | Yeni bir menü ve menü öğeleri oluşturur. | Menü Yöneticisi|
| `GET` | `/api/orders` | Kullanıcının geçmiş siparişlerini listeler. | Müşteri |
| `POST` | `/api/orders` | Yeni bir yemek siparişi oluşturur. | Müşteri |
| `GET` | `/api/orders/:id` | Belirli bir siparişin detayını getirir. | Müşteri |
| `PUT` | `/api/orders/:id` | Bir siparişi günceller (örn: iptal). | Müşteri / Admin |
| `GET` | `/api/delivery` | Teslim edilecek siparişleri listeler. | Yemek Dağıtımcısı|
| `PUT` | `/api/delivery/:id/status`| Bir siparişin teslimat durumunu günceller.| Yemek Dağıtımcısı|
| `GET` | `/api/reports` | Satış/sipariş raporlarını oluşturur. | Kafeterya Personeli|


## 5. Sınıf Diyagramı

Sunucu tarafı uygulaması, aşağıdaki temel sınıflar etrafında şekillenecektir. Bu diyagram, sorumlulukların katmanlı mimariye göre nasıl dağıtıldığını gösterir.

```mermaid
classDiagram
    class AuthController {
      +login(req, res)
      +register(req, res)
    }
    class OrderController {
        +placeOrder(req, res)
        +getUserOrders(req, res)
        +cancelOrder(req, res)
    }
    class MenuController {
        +getMenuByDate(req, res)
        +createMenu(req, res)
    }

    class AuthService {
        +register(userData)
        +login(email, password)
    }
    class OrderService {
        +placeOrder(userId, items, deliveryDetails)
        +calculateTotalPrice(items)
        +updateOrderStatus(orderId, status)
    }
    class MenuService {
        +getAvailableMenu(date)
        +addMenuItem(itemData)
    }

    class UserRepository {
        +findById(id)
        +findByEmail(email)
        +create(user)
    }
    class OrderRepository {
        +create(orderData)
        +findById(id)
        +findByUserId(userId)
    }
    class MenuItemRepository {
        +findAllAvailable()
        +findById(id)
    }

    AuthController ..> AuthService
    OrderController ..> OrderService
    MenuController ..> MenuService

    AuthService ..> UserRepository
    OrderService ..> OrderRepository
    OrderService ..> MenuItemRepository
    MenuService ..> MenuItemRepository

```

## 6. Kullanıcı Akış Diyagramı (Sekans Diyagramı)

Aşağıdaki diyagram, sistemin en temel kullanım senaryosu olan "Yemek Siparişi Verme" sürecini adımlar halinde göstermektedir.

```mermaid
sequenceDiagram
    participant User as Kullanıcı
    participant Frontend as React UI
    participant Backend as Node.js API
    participant DB as PostgreSQL DB

    User->>Frontend: Menüyü görüntülemek ister
    Frontend->>Backend: GET /api/menu?date=YYYY-MM-DD
    Backend->>DB: SELECT * FROM "MenuItem" WHERE ...
    DB-->>Backend: Menü öğeleri listesi
    Backend-->>Frontend: Menü verisi (JSON)
    Frontend-->>User: Güncel menüyü gösterir

    User->>Frontend: Yemekleri seçer ve siparişi onaylar
    Frontend->>Backend: POST /api/orders (Sipariş Detayları)
    Backend->>Backend: İş mantığını çalıştırır (Fiyat hesaplama vb.)
    Backend->>DB: INSERT INTO "MealOrder" (yeni sipariş)
    DB-->>Backend: Yeni sipariş ID'si
    Backend->>DB: INSERT INTO "OrderItem" (sipariş kalemleri)
    DB-->>Backend: Kayıt başarılı
    Backend-->>Frontend: Sipariş onayı (JSON)
    Frontend-->>User: "Siparişiniz alındı" mesajı gösterir
```

## 7. Ekran Akış Diyagramı

Bu diyagram, sistemdeki temel kullanıcı rollerinin (Müşteri ve Menü Yöneticisi) uygulama içindeki ana gezinme yollarını göstermektedir.

```mermaid
graph TD
    subgraph "Genel Akış"
        A(Başlangıç) --> B{Giriş Yapılmış mı?};
        B -- Hayır --> C[Login Sayfası];
        B -- Evet --> D{Kullanıcı Rolü?};
        C --> E{Giriş Başarılı};
        E --> D;
    end

    subgraph "Müşteri Akışı"
        D -- Müşteri --> F[Menü Sayfası];
        F --> G[Siparişlerim Sayfası];
        F --> H[Sepet ve Sipariş Verme];
        G --> I[Sipariş Detay Sayfası];
        H -- Sipariş Başarılı --> G;
    end

    subgraph "Menü Yöneticisi Akışı"
        D -- Menü Yöneticisi --> J[Menü Yönetimi Sayfası];
        J --> K[Yeni Menü Oluşturma];
        J --> L[Mevcut Menüyü Düzenleme];
        J --> M[Menü Öğesi Ekleme/Düzenleme];
    end

    style C fill:#f9f,stroke:#333,stroke-width:2px
    style F fill:#ccf,stroke:#333,stroke-width:2px
    style J fill:#cfc,stroke:#333,stroke-width:2px
```
