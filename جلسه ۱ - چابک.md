# Meeting 1 — Agile and Scrum (توسعه چابک و اسکرام)

- **Course:** Software Engineering (مهندسی نرم‌افزار) — Pressman, Chapter 3
- **Student:** Farzad Taherzadeh
- **Teacher:** Rasoulzadeh
- **Project (chosen system):** Online Food Ordering System — "FoodExpress" (سیستم سفارش آنلاین غذا)

A customer opens the app, sees restaurant menus, orders food, pays online,
and a courier delivers it. The restaurant confirms the order and the customer
can track it and rate it. This same system is used in Meetings 1–5.

---

## 1. Scrum Team Roles (نقش‌های تیم اسکرام)

This is a solo student project, so one person holds the roles below.
In a real team each role is a different person.

| Role | Person | What they do |
| --- | --- | --- |
| **Product Owner (مالک محصول)** | Farzad Taherzadeh | Owns the Product Backlog, decides priorities, talks to users (customers and restaurants) and says what has business value. |
| **Scrum Master (اسکرام مستر)** | Farzad Taherzadeh | Facilitates the Scrum events, removes blocks, makes sure the team follows Scrum. |
| **Development Team (تیم توسعه)** | Farzad Taherzadeh | Builds the product: code, tests, and documents. |

Sprint length: **2 weeks**. Daily Scrum: **15 minutes**.

---

## 2. User Stories (داستان‌های کاربر)

Format: **As a** [role] **I want** [feature] **so that** [benefit].
Each story is checked with INVEST: Independent, Negotiable, Valuable,
Estimable, Small, Testable.

### US-01 — Browse restaurants and menus

> As a **customer**, I want to **see a list of restaurants and their menus
> with prices**, so that **I can choose what to eat**.

- **Acceptance criteria:** the list shows restaurant name, food photo,
  price, and delivery time; the menu loads in under 3 seconds.
- **Story points:** 5
- **Priority:** High

### US-02 — Place an order

> As a **customer**, I want to **add food to a cart and place an order
> with my address**, so that **food is delivered to my home**.

- **Acceptance criteria:** the cart keeps items after login; the order
  shows a total price before I confirm; I get an order number after I pay.
- **Story points:** 8
- **Priority:** High

### US-03 — Pay online

> As a **customer**, I want to **pay with my bank card through a payment
> gateway**, so that **I do not need cash**.

- **Acceptance criteria:** payment goes through a secure gateway;
  a failed payment does not create an order; I get a receipt.
- **Story points:** 5
- **Priority:** High

### US-04 — Confirm orders (restaurant side)

> As a **restaurant owner**, I want to **see new orders and accept or
> reject them**, so that **my kitchen only cooks what I can deliver**.

- **Acceptance criteria:** new orders appear in under 5 seconds;
  the owner can accept or reject with one tap; the customer is notified.
- **Story points:** 5
- **Priority:** High

### US-05 — Track delivery

> As a **customer**, I want to **track my courier on the map and see the
> order status (preparing / on the way / delivered)**, so that **I know
> when my food arrives**.

- **Acceptance criteria:** status changes in real time; the map shows the
  courier's position; the customer gets a message when the food is delivered.
- **Story points:** 8
- **Priority:** Medium

### US-06 — Rate the food and the courier

> As a **customer**, I want to **rate the food and the courier with stars
> after delivery**, so that **good restaurants and couriers are easy to find**.

- **Acceptance criteria:** rating is possible only after delivery;
  one rating per order; the average rating shows on the restaurant page.
- **Story points:** 3
- **Priority:** Low

---

## 3. Product Backlog (بک‌لاگ محصول) — Initial, Prioritized

Ordered by business value. Story points use the Fibonacci scale (3, 5, 8).

| # | Story | Priority | Points | Status |
| --- | --- | --- | --- | --- |
| 1 | US-01 Browse restaurants and menus | High | 5 | To do |
| 2 | US-02 Place an order | High | 8 | To do |
| 3 | US-03 Pay online | High | 5 | To do |
| 4 | US-04 Confirm orders (restaurant) | High | 5 | To do |
| 5 | US-05 Track delivery | Medium | 8 | To do |
| 6 | US-06 Rate food and courier | Low | 3 | To do |

**Total:** 34 story points. The backlog is a living list: new stories can be
added and priorities can change after each Sprint Review.

---

## 4. Sprint 1 Backlog (بک‌لاگ اسپرینت اول) — 2 Weeks

Goal of Sprint 1: **a customer can browse menus and place an order**
(without payment and tracking yet — those come in Sprint 2).

| Story | Points | Tasks |
| --- | --- | --- |
| US-01 Browse restaurants and menus | 5 | Database tables for restaurants and menu items; list page; search by food name |
| US-02 Place an order | 8 | Cart; address form; order total; order number; save order in database |

**Sprint 1 total:** 13 points.

Sprint events: Sprint Planning at the start, Daily Scrum every day
(3 questions: what did I do? what will I do? what blocks me?),
Sprint Review (demo to the Product Owner) and Sprint Retrospective
(what went well, what to improve) at the end.

---

## 5. Technical Debt — Example and Refactoring (بدهی فنی)

**Technical debt example:** in Sprint 1, to save time, the order total
(price + delivery fee + tax) is calculated in three different places:
in the cart page, in the checkout page, and in the order confirmation
email. If the delivery fee changes, all three places must change — and
one of them will be forgotten. This is duplicated logic, a typical
technical debt.

**Refactoring plan:** extract one method `calculateOrderTotal(items,
deliveryFee)` in an `OrderPricing` class. All three places call this
one method. The behavior stays the same, the code becomes one source
of truth, and the next change needs only one edit.

**Rule for this project:** every Sprint Retrospective lists new technical
debt, and one refactoring task goes into the next Sprint Backlog.
