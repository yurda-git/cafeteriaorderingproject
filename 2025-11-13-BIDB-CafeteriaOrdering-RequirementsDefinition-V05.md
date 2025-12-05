
# METU COMPUTER CENTER
## Software Requirements Specification Document
### Cafeteria Ordering System

**Hazırlayan:** Yurdagül ÖZTÜRK  
**Onaylayan:** Dr. Özgür Kaya  
**Yürürlük Tarihi:** 14.11.2025  
**Revizyon Tarihi:** 14.11.2025  
**Revizyon Numarası:** 1  

---

## Table of Contents
- 1. Giriş (Introduction)
  - 1.1 Amaç (Purpose)
  - 1.2 Kapsam (Project scope)
- 2. Overall description
  - 2.1 Ürün Perspektifi (Product perspective)
  - 2.2 Ürün Yaşam Döngüsü (State Transition)
  - 2.3 Kullanıcı Sınıfları ve Özellikleri (User classes and characteristics)
- 3. Sistem Özellikleri (System features)
  - 3.1 İş Süreçleri
    - 3.1.1 İş Gereksinimleri
    - 3.1.2 Ana İş Süreci
    - 3.1.x Alt İş Süreci x
  - 3.2 Kullanıcıların Sistemde Gerçekleştireceği İşlemler
    - 3.2.1 Genel Kullanıcı Senaryoları
    - 3.2.2.1 Kullanım Senaryosu Tanımı (Use Case Definition)
    - 3.2.x.2 Kullanıcı Arayüzleri (User Interfaces)
    - 3.2.x.3 Akış Diyagramları (Flowchart)
- 4. Veri Gereksinimleri (Data requirements)
  - 4.1 Mantıksal Veri Modeli (Logical data model)
  - 4.2 Veri Sözlüğü (Data dictionary)
- 8. Gereksinimler [Requirements]
  - 8.1.x Kullanım Senaryosunun Fonksiyonel Gereksinimleri
  - 8.2.x İş Gereksinimleri (Business Rules)
  - 8.3. Diğer Gereksinimler [Other requirements]

---

## 1. Giriş (Introduction)
The introduction presents an overview to help the reader understand how the SRS is organized and how to use it.

### 1.1 Amaç (Purpose)
This SRS describes the functional and nonfunctional requirements for the Cafeteria Ordering System (COS). This document is intended to be used by the members of the project team who will implement and verify the correct functioning of the system.

### 1.2 Kapsam (Project scope)
The COS will permit METU employees to order meals from the METU cafeteria online to be delivered to specified campus locations.

Only “Müşteri Yemek Siparişi Verir” use case whith its business rules will be developed in this scope, other use cases and busiess rules will be developed in the following releases. “Ödemenin Yapılması” use case will also be developed in the following releases

| Aktör | Kullanım Senaryosu | Release |
| :--- | :--- | :--- |
| Müşteri | 1. Yemek Siparişi Verir | 1.st release |
| | 2. Yemek Siparişini Değiştirir | 1.st release |
| | 3. Yemek Siparişini İptal Eder | Following releases |
| | 4. Menüyü Görüntüler | 1.st release |
| | 5. Ödemesini Yapabilir | Following releases |
| Menü Yöneticisi | 8. Menü Oluşturabilir | 1.st release |
| | 9. Menüleri Arşivleyebilir | Following releases |
| | 10. Yemek Spesiyali Tanımlayabilir | Following releases |
| Kafeterya Personeli | 11. Yemek Teslimatı Talep Edebilir | Following releases |
| | 12. Raporlar Oluşturabilir | Following releases |
| Yemek Dağıtımcısı | 13. Yemek Teslimatını Kaydedebilir | Following releases |
| | 14. Teslimat Talimatlarını Yazdırabilir | Following releases |

---

## 2. Overall description

### 2.1 Ürün Perspektifi (Product perspective)
The Cafeteria Ordering System is a new software system that replaces the current manual and smart card processes for ordering and picking up meals in the Process Impact cafeteria. The system is expected to evolve over several releases, ultimately connecting to the Internet ordering services for several local restaurants and to credit and debit card authorization services.

*(Use case diagram was present in the source document)*

### 2.2 Ürün Yaşam Döngüsü (State Transition)
```mermaid
stateDiagram-v2
    [*] --> Incomplete
    Incomplete --> Accepted : System accepts completed order
    Incomplete --> Canceled : Patron cancels do not charge
    
    Accepted --> Prepared : Cafeteria Staff prepares food
    Accepted --> Canceled : Patron cancels do not charge
    
    Prepared --> PendingDelivery : Cafeteria Staff requests delivery
    Prepared --> Canceled : Patron cancels charge payment
    
    PendingDelivery --> Delivered : Meal Deliverer delivers meal
    PendingDelivery --> Accepted : Patron refuses to accept delivery

    Canceled --> [*]
    Delivered --> [*]
```

### 2.3 Kullanıcı Sınıfları ve Özellikleri (User classes and characteristics)

| User class | Description |
| :--- | :--- |
| **Patron (favored)** | A Patron is a Process Impact employee who wants to order meals to be delivered from the company cafeteria. There are about 600 potential Patrons, of which 300 are expected to use the COS an average of 5 times per week each. Patrons will sometimes order multiple meals for group events or guests. An estimated 60 percent of orders will be placed using the corporate intranet, with 40 percent of orders being placed from home or by smartphone or tablet apps. |
| **Cafeteria Staff** | The Process Impact cafeteria employs about 20 Cafeteria Staff who will receive orders from the COS, prepare meals, package them for delivery, and request delivery. Most of the Cafeteria Staff will need training in the use of the hardware and software for the COS. |
| **Menu Manager** | The Menu Manager is a cafeteria employee who establishes and maintains daily menus of the food items available from the cafeteria. Some menu items may not be available for delivery. The Menu Manager will also define the cafeteria's daily specials. The Menu Manager will need to edit existing menus periodically. |
| **Meal Deliverer** | As the Cafeteria Staff prepare orders for delivery, they will issue delivery requests to a Meal Deliverer's smartphone. The Meal Deliverer will pick up the food and deliver it to the Patron. A Meal Deliverer's other interactions with the COS will be to confirm that a meal was (or was not) delivered. |

---

## 3. Sistem Özellikleri (System features)

### 3.1 İş Süreçleri

#### 3.1.1 İş Gereksinimleri
1. Kafeterya karını arttırmak
2. Müşteri memnuniyetini arttırmak
3. Daha fazla müşteriye ulaşmak

#### 3.1.2 Ana İş Süreci
Definition:
Model:

#### 3.1.x Alt İş Süreci x

### 3.2 Kullanıcıların Sistemde Gerçekleştireceği İşlemler

#### 3.2.1 Genel Kullanıcı Senaryoları (Use Cases)


```mermaid
graph LR
    subgraph "Kullanıcılar"
        Musteri["Müşteri"]
        MenuManager["Menü Yöneticisi"]
        CafeteriaStaff["Kafeterya Personeli"]
        Delivery["Yemek Dağıtımcısı"]
    end

    subgraph "Sistem ve Banka"
        Banka["Banka"]
        Sistem["Sistem"]
    end
    
    subgraph "Kullanım Senaryoları"
        YSV["Yemek Siparişi Verir"]
        YSD["Yemek Siparişini Değiştirir"]
        YSI["Yemek Siparişini İptal Eder"]
        MG["Menüyü Görüntüler"]
        OY["Ödemesini Yapabilir"]
        ISGO["İptal edilen siparişlerin geri ödemesi yapılır"]
        MO["Menüyü Oluşturabilir"]
        MA["Menüleri Arşivleyebilir"]
        YST["Yemek Spesiyali Tanımlayabilir"]
        YTTE["Yemek Teslimatı Talep Edebilir"]
        RO["Raporlar Oluşturabilir"]
        YTK["Yemek Teslimatını Kaydedebilir"]
        TTY["Teslimat Talimatlarını Yazdırabilir"]
    end

    Musteri --> YSV
    Musteri --> YSD
    Musteri --> YSI
    Musteri --> MG

    MenuManager --> MO
    MenuManager --> MA
    MenuManager --> YST

    CafeteriaStaff --> YTTE
    CafeteriaStaff --> RO

    Delivery --> YTK
    Delivery --> TTY

    YSV -.->|include| OY
    YSD -.->|include| OY
    YSI -.->|include| ISGO

    OY --> Banka
    ISGO --> Sistem
```


---

### 3.2.2 Müşteri Yemek Siparişi Verir
#### 3.2.2.1 Kullanım Senaryosu Tanımı (Use Case Definition)

**ID and Name:** UC-1: Order a Meal (Müşteri Yemek Siparişi Verir)  
**Created By:** Prithvi Raj  
**Date Created:** October 4, 2013  
**Primary Actor:** Patron  
**Secondary Actors:** Cafeteria Inventory System  

**Description:**
A Patron accesses the Cafeteria Ordering System from either the corporate intranet or external Internet, views the menu for a specific date, selects food items, and places an order for a meal to be picked up in the cafeteria or delivered to a specified location within a specified 15-minute time window.

**Trigger:**
A Patron indicates that he wants to order a meal.

**Preconditions:**
- PRE-1. Patron is logged into COS.
- PRE-2. Patron is registered for meal payments by payroll deduction

**Postconditions:**
- POST-1. Meal order is stored in COS with a status of “Accepted.”
- POST-2. Inventory of available food items is updated to reflect items in this order.
- POST-3. Remaining delivery capacity for the requested time window is updated.

**Normal Flow: 1.0 Order a Single Meal**
1. Patron asks to view menu for a specific date. (see 1.0.E1, 1.0.E2)
2. COS displays menu of available food items and the daily special.
3. Patron selects one or more food items from menu. (see 1.1)
4. Patron indicates that meal order is complete. (see 1.2)
5. COS displays ordered menu items, individual prices, and total price, including taxes and delivery charge.
6. Patron either confirms meal order (continue normal flow) or requests to modify meal order (return to step 2).
7. COS displays available delivery times for the delivery date.
8. Patron selects a delivery time and specifies the delivery location.
9. Patron specifies payment method.
10. COS confirms acceptance of the order.
11. COS sends Patron an email message confirming order details, price, and delivery instructions.
12. COS stores order, sends food item information to Cafeteria Inventory System, and updates available delivery times.

**Alternative Flows:**
- **1.1 Order multiple identical meals**
  1. Patron requests a specified number of identical meals. (see 1.1.E1)
  2. Return to step 4 of normal flow.
- **1.2 Order multiple meals**
  1. Patron asks to order another meal.
  2. Return to step 1 of normal flow.

**Exceptions:**
- **1.0.E1 Requested date is today and current time is after today's order cutoff time**
  1. COS informs Patron that it's too late to place an order for today.
  2a. If Patron cancels the meal ordering process, then COS terminates use case.
  2b. Else if Patron requests another date, then COS restarts use case.
- **1.0.E2 No delivery times left**
  1. COS informs Patron that no delivery times are available for the meal date.
  2a. If Patron cancels the meal ordering process, then COS terminates use case.
  2b. Else if Patron requests to pick the order up at the cafeteria, then continue with normal flow, but skip steps 7 and 8.
- **1.1.E1 Insufficient inventory to fulfill multiple meal order**
  1. COS informs Patron of the maximum number of identical meals he can order, based on current available inventory.
  2a. If Patron modifies number of meals ordered, then return to step 4 of normal flow.
  2b. Else if Patron cancels the meal ordering process, then COS terminates use case.

**Priority:** High  
**Frequency of Use:** Approximately 300 users, average of one usage per day. Peak usage load for this use case is between 9:00 A.M. and 10:00 A.M. local time.  
**Business Rules:** BR-1, BR-2, BR-3, BR-4, BR-11, BR-12, BR-33  

**Other Information:**
1. Patron shall be able to cancel the meal ordering process at any time prior to confirming it.
2. Patron shall be able to view all meals he ordered within the previous six months and repeat one of those meals as the new order, provided that all food items are available on the menu for the requested delivery date. (Priority = medium) [Note: You could also show this as an alternative flow for the use case.]
3. The default date is the current date if the Patron is using the system before today's order cutoff time. Otherwise, the default date is the next day that the cafeteria is open.

**Assumptions:**
Assume that 15 percent of Patrons will order the daily special (Source: previous 6 months of cafeteria data).

---

### 3.2.x.2 Kullanıcı Arayüzleri (User Interfaces)
Ekran arayüzleri. Buraya eklenebilir veya referans verilebilir.

### 3.2.x.3 Akış Diyagramları (Flowchart)
Detay İş akışları. Buraya eklenebilir veya referans verilebilir.

---

## 4. Veri Gereksinimleri (Data requirements)

### 4.1 Mantıksal Veri Modeli (Logical data model)
```mermaid
graph LR
    subgraph Entities
        Patron["Patron"]
        MealOrder["Meal Order"]
        MealPayment["Meal Payment"]
        OrderedFoodItem["Ordered Food Item"]
        MenuFoodItem["Menu Food Item"]
        Menu["Menu"]
    end

    subgraph Relationships
        placing{"placing"}
        paying{"paying"}
        containing_mo{"containing"}
        choosing{"choosing"}
        containing_mfi{"containing"}
    end

    Patron -- "1" --> placing
    placing -- "m" --> MealOrder
    
    MealOrder -- "1" --> paying
    paying -- "1" --> MealPayment

    MealOrder -- "m" --> containing_mo
    containing_mo -- "m" --> OrderedFoodItem

    OrderedFoodItem -- "1" --> choosing
    choosing -- "m" --> MenuFoodItem

    Menu -- "m" --> containing_mfi
    containing_mfi -- "m" --> MenuFoodItem
```

### 4.2 Veri Sözlüğü (Data dictionary)

| Data element | Description | Composition or data type | Length | Values |
| :--- | :--- | :--- | :--- | :--- |
| delivery instruction | where and to whom a meal is to be delivered, if it isn't being picked up in the cafeteria | patron name + patron phone number + meal date + delivery location + delivery time window | | |
| delivery location | building and room to which an ordered meal is to be delivered | alphanumeric | 50 | hyphens and commas permitted |
| delivery time | beginning time of a 15-minute range on the meal date during which an ordered meal is to be delivered | time | hh:mm | |
| employee ID | company ID number of the employee who placed a meal order | integer | 6 | |
| food item description | description of a food item on a menu | alphabetic | 100 | |
| food item price | pre-tax cost of a single unit of a menu food item | numeric, dollars and TL | dd.cc | |
| meal order | details about a meal a Patron ordered | meal order number + order date + meal date + 1:m{ordered food item} + delivery instruction + meal order status | | |
| meal order number | unique ID that COS assigns to each accepted meal order | integer | 7 | Initial value is 1 |
| menu | list of food items available for purchase on a specific date | menu date + 1:m{menu food item} | | |
| menu food item | description of a menu item | food item description + food item price | | |
| patron | a Process Impact employee who is authorized to order | patron name + employee ID + patron phone number + patron location + patron email | | |
| patron email | email address of the employee who placed a meal order | alphanumeric | 50 | |
| patron location | building and room numbers of the employee who placed a meal order | alphanumeric | 50 | hyphens and commas permitted |
| patron name | name of the employee who placed a meal order | alphabetic | 30 | |
| patron phone number | telephone number of the employee who placed a meal order | AAA-EEE-NNNN XXXXX for area code (A), exchange (E), number (N), and extension (X) | 18 | |
| meal order status | status of a meal order that a Patron initiated | alphabetic | 16 | Incomplete, accepted, prepared, pending delivery, delivered, canceled |

---

## 8. Gereksinimler [Requirements]

*(Requirements table was present in the source document)*

### 8.1.x Kullanım Senaryosunun Fonksiyonel Gereksinimleri

### 8.2.x İş Gereksinimleri (Business Rules)
BR-1, BR-2, BR-3, BR-4, BR-11, BR-12, BR-33

| ID | Rule definition | Type of rule | Static or dynamic | Source |
| :--- | :--- | :--- | :--- | :--- |
| BR-1 | Delivery time windows are 15 minutes, beginning on each quarter hour. | Fact | Dynamic | Cafeteria Manager |
| BR-2 | Deliveries must be completed between 11:00 A.M. and 2:00 P.M. local time, inclusive. | Constraint | Dynamic | Cafeteria Manager |
| BR-3 | All meals in a single order must be delivered to the same location. | Constraint | Static | Cafeteria Manager |
| BR-4 | All meals in a single order must be paid for by using the same payment method. | Constraint | Static | Cafeteria Manager |
| BR-8 | Meals must be ordered within 14 calendar days of the meal date. | Constraint | Dynamic | Cafeteria Manager |
| BR-11 | If an order is to be delivered, the patron must pay by payroll deduction. | Constraint | Dynamic | Cafeteria Manager |
| BR-12 | Order price is calculated as the sum of each food item price times the quantity of that food item ordered, plus applicable sales tax, plus a delivery charge if a meal is delivered outside the free delivery zone. | Computation | Dynamic | cafeteria policy; state tax code |

