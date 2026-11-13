# Meeting 2 — Requirements Engineering (مهندسی نیازمندی‌ها)

- **Course:** Software Engineering (مهندسی نرم‌افزار) — Pressman, Chapter 7
- **Student:** Farzad Taherzadeh
- **Teacher:** Rasoulzadeh
- **Project:** Online Food Ordering System — "FoodExpress" (سیستم سفارش آنلاین غذا)

The same system as Meeting 1: a customer orders food from a restaurant
in the app, pays online, the restaurant confirms, and a courier delivers it.

---

## 1. Stakeholders (ذینفعان)

A stakeholder is any person or group who affects the system or is
affected by it.

| # | Stakeholder | Interest in the system | Influence |
| --- | --- | --- | --- |
| 1 | **Customer (مشتری)** | Orders food fast, pays safely, tracks delivery | High — the main user; if the app is hard to use, they leave |
| 2 | **Restaurant Owner (صاحب رستوران)** | Gets more orders, manages menu and prices, sees sales | High — supplies the food and the menu data |
| 3 | **Courier / Delivery Rider (پیک)** | Gets delivery jobs, sees the address and the route, gets paid per delivery | Medium — delivers the food, needs a clear and simple app |
| 4 | **System Admin (مدیر سیستم)** | Manages users, restaurants and couriers; solves problems; sees reports | Medium — keeps the system running and fair |
| 5 | **Payment Gateway (درگاه پرداخت)** | Processes card payments safely (external system) | Medium — the system depends on it for all payments |
| 6 | **Kitchen Staff (پرسنل آشپزخانه)** | Sees new orders and cooking time | Low — uses the restaurant panel only |

Requirements were found by: interviews with a restaurant owner and
two customers, a short questionnaire, and looking at similar apps
(SnappFood-style systems).

---

## 2. Functional Requirements (نیازمندی‌های عملکردی)

Functional requirements say **what the system must do**.

| ID | Requirement | Priority |
| --- | --- | --- |
| FR-01 | The system shall let a customer **register and log in** with phone number and password. | High |
| FR-02 | The system shall show a **list of restaurants and their menus** with name, photo, price, and delivery time, and let the customer **search by food or restaurant name**. | High |
| FR-03 | The system shall let a customer **add food to a cart**, change the quantity, and see the total price. | High |
| FR-04 | The system shall let a customer **place an order** with a delivery address and a note, and give an **order number** after payment. | High |
| FR-05 | The system shall process **online payment** through a bank payment gateway and send a receipt after a successful payment. | High |
| FR-06 | The system shall let a restaurant owner **accept or reject** a new order and update its status (preparing / ready). | High |
| FR-07 | The system shall assign an available courier to a ready order and show the courier the **delivery address and route**. | Medium |
| FR-08 | The system shall show the customer the **live order status** (preparing / on the way / delivered) and the courier's position on the map. | Medium |
| FR-09 | The system shall let a customer **rate the food and the courier** (1–5 stars) after delivery. | Low |
| FR-10 | The system shall let a restaurant owner **add, edit, and remove menu items** and change prices. | Medium |

---

## 3. Non-Functional Requirements (نیازمندی‌های غیرعملکردی)

Non-functional requirements say **how well the system must do it**.

| ID | Requirement | Type | Measure |
| --- | --- | --- | --- |
| NFR-01 | The menu and restaurant list shall load in **under 3 seconds** on a normal mobile connection. | Performance (کارایی) | Page load time < 3 s |
| NFR-02 | The system shall be **available 99% of the time** (about 7 hours of downtime per month at most). | Availability / Reliability (قابلیت اطمینان) | Uptime ≥ 99% |
| NFR-03 | Customer passwords shall be stored **hashed**, all payment data shall go through the bank gateway only (the system never stores card numbers), and all traffic shall use **HTTPS**. | Security (امنیت) | No plain-text passwords; no card numbers stored |
| NFR-04 | The app shall be **usable in Persian (RTL)** and simple enough that a new customer can place an order **without help** in under 2 minutes. | Usability (کاربردپذیری) | First order < 2 min, no training |
| NFR-05 | The system shall support **1,000 simultaneous users** and **10,000 orders per day** without slowing down. | Scalability (مقیاس‌پذیری) | Load test: 1,000 concurrent users |
| NFR-06 | The system shall run on **Android and iOS phones** and in a **web browser**. | Portability (قابلیت حمل) | Android, iOS, web |

---

## 4. Use Cases (یوزکیس‌ها) — Full Description

Two main use cases, written in the standard anatomy: name, actors,
preconditions, main flow, alternative flows, postconditions.

### UC-01 — Place an Order (ثبت سفارش)

| Part | Description |
| --- | --- |
| **Actors** | Customer (main), Restaurant Owner, Payment Gateway (external) |
| **Goal** | The customer orders food and pays for it |
| **Preconditions** | The customer is logged in; at least one restaurant is open |
| **Main flow** | 1. Customer opens the restaurant list and picks a restaurant. 2. Customer adds food to the cart. 3. Customer opens the cart, checks the total, and enters the delivery address. 4. Customer confirms the order. 5. System sends the customer to the payment gateway. 6. Customer pays; the gateway returns success. 7. System creates the order with status "waiting for restaurant" and shows the order number. 8. Restaurant owner accepts the order. |
| **Alternative flow A — payment fails** | At step 6, if the payment fails, the system shows an error, does **not** create an order, and returns the customer to the cart. |
| **Alternative flow B — restaurant rejects** | At step 8, if the restaurant rejects the order, the system refunds the payment and notifies the customer. |
| **Postconditions** | The order exists in the database with status "accepted"; the customer, restaurant, and (later) courier can see it. |

### UC-02 — Track Delivery (پیگیری تحویل)

| Part | Description |
| --- | --- |
| **Actors** | Customer (main), Courier |
| **Goal** | The customer knows where the order is and when it arrives |
| **Preconditions** | The order is accepted and a courier is assigned |
| **Main flow** | 1. Customer opens the order page. 2. System shows the status (preparing / picked up / on the way). 3. System shows the courier's live position on the map. 4. Courier marks the order "delivered". 5. System notifies the customer and asks for a rating. |
| **Alternative flow A — courier is late** | If the delivery takes more than 60 minutes, the system notifies the admin and offers the customer a discount on the next order. |
| **Postconditions** | The order status is "delivered"; the delivery time is saved for reports. |

Other use cases of the system (modeled in Meeting 3): Register / Log in,
Manage Menu, Rate Order, Manage Users (admin).
