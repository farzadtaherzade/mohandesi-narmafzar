# Meeting 4 — Design Concepts (مفاهیم پایه طراحی)

- **Course:** Software Engineering (مهندسی نرم‌افزار) — Pressman, Chapter 9
- **Student:** Farzad Taherzadeh
- **Teacher:** Rasoulzadeh
- **Project:** Online Food Ordering System — "FoodExpress" (سیستم سفارش آنلاین غذا)

Same system as Meetings 1–3. Design answers **how** the system is built;
analysis (Meetings 2–3) answered **what** it must do.

---

## 1. Modules of the System (ماژول‌ها)

The system is split into modules so that each module has one clear job.
A module is a group of classes and functions that work together.

| # | Module | Responsibility (one job) | Main classes |
| --- | --- | --- | --- |
| 1 | **User Module** | Register, log in, profiles, addresses | Customer, Courier, Admin |
| 2 | **Catalog Module** | Restaurants and menus: list, search, prices | Restaurant, MenuItem |
| 3 | **Order Module** | Cart, order creation, order status, order history | Order, Cart, OrderController |
| 4 | **Payment Module** | Pay, refund, receipts — talks to the bank gateway | Payment, PaymentGateway |
| 5 | **Delivery Module** | Assign courier, live tracking, delivery confirmation | Courier, Delivery, Tracking |
| 6 | **Notification Module** | SMS / push messages to customer, restaurant, courier | NotificationService |
| 7 | **Restaurant Module** | Accept/reject orders, manage menu, see sales | Restaurant, MenuItem |
| 8 | **Admin & Report Module** | Manage users, reports (orders per day, income) | Admin, Report |

**Design rule used:** a module should do one thing well. If a module's
job cannot be written in one short sentence, it must be split.

---

## 2. Architecture Layering (لایه‌بندی — short version)

The modules sit in layers (full diagram in Meeting 5, `ARCHITECTURE.md`):

1. **Presentation layer** — app screens and web pages
2. **Application layer** — controllers (e.g. `OrderController`)
3. **Business layer** — rules: prices, discounts, order status changes
4. **Data layer** — database access: save and read orders, users, menus

A layer only talks to the layer below it. The presentation layer never
touches the database directly.

---

## 3. Cohesion Analysis (همبستگی — Cohesion)

Cohesion = how closely the parts **inside** one module belong together.
**High cohesion is good.** (Pressman: functional cohesion is the best level.)

### Example 1 — High cohesion: Order Module (good)

Everything in the Order Module is about one thing: **an order's life**.
`Order` holds the data, `Cart` holds the items, `OrderController`
runs the steps (check cart → total → pay → create order → notify).
If the delivery fee rule changes, only this module changes.
No other module needs to know how an order total is calculated.

- **Level:** Functional cohesion — all parts work together for one
  single task (managing an order).
- **Result:** easy to understand, easy to test, easy to change.

### Example 2 — Low cohesion: a "Utils" module (bad, avoided)

If all helper functions (send SMS, format money, resize photos,
validate phone numbers) were put in one `Utils` module, that module
would have **coincidental cohesion** — its parts have nothing in
common except being "helpers". A change to SMS code could break photo
code by accident. This project does **not** do that: each helper lives
in the module that uses it (SMS code in Notification Module, money
formatting in Order Module).

---

## 4. Coupling Analysis (تزویج — Coupling)

Coupling = how much one module **depends on** another module.
**Low coupling is good.** (Pressman: data coupling is the loosest useful level.)

### Example 1 — Low coupling: Payment Module (good)

The Order Module does **not** call the bank gateway directly. It calls
the interface `IPaymentGateway` with only two pieces of data:
`pay(amount)` and it gets back `success / failed`.
The real gateway class (Zarinpal, or any other bank) sits behind that
interface. If the bank changes, only the Payment Module changes —
the Order Module does not even notice.

- **Level:** Data coupling — modules share only simple data
  (an amount and a yes/no result), not internal details.
- **Result:** the payment provider can be replaced without touching
  the rest of the system.

### Example 2 — High coupling to avoid: Order calling the courier's phone (bad)

A bad design would be: the Order Module directly calls the courier's
SMS API and reads the courier's internal database table to find who
is free. Then the Order Module depends on the courier's table
structure, the SMS provider, and the courier's code — change any of
them and the Order Module breaks. This project does **not** do that:
the Order Module asks the Delivery Module `assignCourier(order)` and
gets back a courier. How the Delivery Module finds the courier is
its own business.

- **Level (avoided):** Content / common coupling — one module uses
  another module's internal data directly.
- **Rule for this project:** modules talk through small, clear
  interfaces, never through each other's database tables.

---

## 5. Refactoring Scenario (سناریوی بازآفرینی کد)

**Problem found:** in the first version, the class `OrderService`
grew into a "god class" with 600 lines: it calculated prices, called
the payment gateway, sent SMS messages, assigned couriers, and saved
orders. One class, five jobs — low cohesion, hard to test.

**Refactoring steps (behavior stays exactly the same):**

1. **Extract Class** — move price calculation to `OrderPricing`
   (already planned in Meeting 1, `AGILE.md` §5).
2. **Extract Class** — move SMS/push code to `NotificationService`.
3. **Extract Class** — move courier assignment to `DeliveryService`.
4. `OrderService` keeps only one job: the order steps
   (check → total → pay → create → notify).

**After refactoring:** 4 small classes, each with one job and one
reason to change. The `OrderService` test becomes short: it checks
the steps, and each small class is tested alone. No feature changes —
the customer sees nothing different, but the code is clean and safe
to change in the next sprint.
