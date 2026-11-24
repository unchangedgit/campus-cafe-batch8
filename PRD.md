# Product Requirements Document: Campus Café Batch Delivery System

**Course:** Innovaton in Web Design and Development (BU-COS209) | **Team:** Null Void (Group 8) 

## 1. Overview

| Item | Detail |
|---|---|
| Product | Campus Café Batch Delivery (working title) |
| Platform | Mobile-first web app (PWA) with WhatsApp and SMS alerts |
| Owner | Null Void (Group 8) |
| Version | 2.0 |
| Users | Students, student deliverers, café staff and campus admins |

**Revision history**

| Version | What changed |
|---|---|
| 1.1 | First draft: problem, personas, batch model, core features and MVP scope |
| 2.0 | Added goals and non-goals, user stories, numbered requirements with priorities, batch states, business rules, non-functional requirements, data model, notifications, risks, open questions and a phased roadmap |

## 2. Problem and Objective

### 2.1 The problem

Students lose a lot of time getting food from the campus café. The queues are long, the gaps between classes are short, and the café can be a real walk from the hostel or the library. Regular delivery apps either don't operate on campus or cost too much for a student budget.

The café has a problem of its own. Orders come in one at a time throughout the day, which clogs the counter at peak hours.

### 2.2 The objective

We want to build a scheduled food delivery system that runs on student volunteers. Orders are grouped into fixed time windows called batches. Grouping them does a few things for us:

- Fewer trips for each deliverer.
- Delivery that is cheap or free for students.
- No pile-up at the café counter, since the kitchen works from one prep list per batch.
- Predictable pickup and drop-off times for volunteers.

### 2.3 Goals

1. A student can place an order in under two minutes and pick it up at a campus drop-off point.
2. The café gets one combined prep list per batch instead of a stream of separate orders.
3. Volunteers get set shifts, grouped routes and very little waiting time.
4. We never promise more than we can deliver. Each batch is capped by the number of confirmed volunteers.
5. We launch small and quickly, so we can find out whether students actually want this.

### 2.4 Non-goals for Phase 1

- On-demand delivery outside the scheduled batches.
- Delivery to individual rooms or doors. Phase 1 only uses drop-off points.
- Other vendors. Phase 1 covers the campus café only.
- Native iOS and Android apps.
- Paying deliverers through the system. We track perks and stipends but don't process payouts.

## 3. Users

### 3.1 Student customer

- **Who they are:** Busy students in hostels, the library or lecture halls who want café food without walking over or standing in a queue.
- **What they need:** A simple way to order, a clear cutoff timer, a reliable drop-off point and honest order status.
- **What gets in the way:** Back-to-back classes, long queues at peak hours, and not knowing when the food will arrive.
- **A good outcome:** They order in a few taps, know when to expect the food and pick it up at a spot they already know.

### 3.2 Student deliverer

- **Who they are:** Students who volunteer in free gaps between classes or in the evening, to earn stipends, campus perks or community hours.
- **What they need:** Predictable batch times, orders grouped by zone or hostel, a clear pickup list and very little waiting at the café.
- **What gets in the way:** Waiting for food that isn't ready, confusing drop-off lists, customers who don't show up, and unclear rewards.
- **A good outcome:** They sign up for a slot, collect a checked bundle quickly, finish a short route and see credit for the work.

### 3.3 Café staff / vendor

- **Who they are:** The kitchen and counter staff who prepare student orders.
- **What they need:** One combined prep list for each batch instead of scattered orders, and an easy handover to the right deliverer.
- **What gets in the way:** A crowded counter, interruptions from ad hoc orders, and mix-ups when bags are handed to couriers.
- **A good outcome:** They see one aggregated list, pack bags labeled per order and hand each bag to the right person.

### 3.4 Platform admin

Someone from the student union, the faculty or our own team who looks after batch schedules, zones, volunteer approvals and disputes. They need to set batch times and caps, check the numbers, handle escalations and manage accounts.

## 4. User Stories

**Student customer**

- As a student, I want to see the next batch and a live countdown, so I know whether I can still order in time.
- As a student, I want my order moved to the next batch if I miss the cutoff, so I don't have to start over.
- As a student, I want to choose a drop-off point, so my food arrives close to where I'll be.
- As a student, I want to track my order and get a PIN, so I can confirm the delivery safely.

**Student deliverer**

- As a deliverer, I want to sign up for specific batches and zones, so deliveries fit around my classes.
- As a deliverer, I want a manifest of grouped orders, so I can plan a sensible route.
- As a deliverer, I want to confirm pickup by scanning or ticking items off, so nothing gets left behind.
- As a deliverer, I want to mark an order delivered with the customer's PIN, so every delivery is verified.

**Café staff**

- As café staff, I want item totals for each locked batch, so I can prepare food efficiently.
- As café staff, I want packing slips and a handover view, so each bag goes to the right deliverer.

**Admin**

- As an admin, I want to set batch times, caps and zones, so the system matches what we can really handle.
- As an admin, I want to see the KPIs, so I can tell whether the service is working.

## 5. How the Batch Model Works

Instead of sending a courier out for every order, the system delivers in scheduled waves.

1. **Order window (open).** Students order for the upcoming batch until a strict cutoff, for example 12:30 PM for the 1:00 PM batch.
2. **Cutoff and rollover.** When the cutoff arrives, or the batch hits its maximum order count, it locks. Anyone who missed it is prompted to order for the next batch (say, 2:00 PM).
3. **Prep and assignment.** Between cutoff and pickup (12:30 to 1:00 PM in our example) the café prepares the grouped orders. The system assigns orders by drop-off zone, such as Male Hostels, Female Hostels or Library/Academic Block, to the volunteers who claimed that shift.
4. **Pickup and dispatch.** Deliverers arrive at the café at pickup time (1:00 PM), check and collect their bundle, then make their drop-offs along the route.

### 5.1 Example timeline

| Time | What happens |
|---|---|
| Before 12:30 PM | Batch is **Open**. Students order and see the countdown |
| 12:15 PM | Deliverers must be checked in (15 minutes before pickup) |
| 12:30 PM | **Cutoff.** The batch locks and late orders roll over to the 2:00 PM batch |
| 12:30 to 1:00 PM | **Preparing.** The café prepares items and the system assigns orders by zone |
| 1:00 PM | **Pickup.** Deliverers check and collect their bundles |
| After 1:00 PM | **Out for delivery.** Drop-offs with PIN confirmation |
| End of route | **Completed.** The batch closes and its numbers are recorded |

### 5.2 Batch states

`Scheduled → Open → Locked → Preparing → Ready for Pickup → Out for Delivery → Completed`

A batch can also be **Cancelled**, for instance when no deliverers are confirmed by cutoff. Customers in a cancelled batch can roll over to the next one or get a refund.

## 6. Functional Requirements

Priorities: **P0** is a must for the MVP, **P1** should be in soon after, **P2** can wait.

### 6.1 Student customer

| ID | Requirement | Priority |
|---|---|---|
| FR-C-01 | **Menu and ordering.** Browse café items, customize options (extras, portion size) and add them to a cart. Out-of-stock items are hidden or marked unavailable. | P0 |
| FR-C-02 | **Batch selector and countdown.** Show the active batch time, the estimated delivery window and a live countdown to cutoff. | P0 |
| FR-C-03 | **Rollover at checkout.** If the batch closes while someone is checking out, move them to the next available batch and ask them to confirm before going ahead. | P0 |
| FR-C-04 | **Drop-off zone.** Pick from set campus drop-off points (hostel gates, common rooms, faculty buildings) so routes can be grouped. | P0 |
| FR-C-05 | **Payment.** Bank transfer, wallet balance, card, or pay on delivery where it's allowed. | P0 |
| FR-C-06 | **Order tracking.** Order Placed, Batch Locked / Preparing, Out for Batch Delivery, Delivered. | P0 |
| FR-C-07 | **Delivery PIN.** Give the customer a 4-digit PIN to hand over at drop-off. | P0 |
| FR-C-08 | **Order history and reorder.** See past orders and reorder in one tap. | P1 |
| FR-C-09 | **Edit or cancel.** Let customers change or cancel an order until cutoff, with an automatic refund or credit. | P1 |
| FR-C-10 | **Ratings.** Rate the order and the delivery afterwards. | P2 |

### 6.2 Student deliverer

| ID | Requirement | Priority |
|---|---|---|
| FR-D-01 | **Shift sign-up.** See upcoming batch slots and how much capacity is left, then volunteer for specific batches and zones. | P0 |
| FR-D-02 | **Check-in.** Check in at least 15 minutes before pickup to confirm they're coming. | P0 |
| FR-D-03 | **Batch manifest.** Once the batch locks, show the full list of assigned orders with customer names, drop-off points and contact info. | P0 |
| FR-D-04 | **Pickup verification.** Confirm every package at the café counter using a QR code or an order ID checklist, so nothing is missed. | P0 |
| FR-D-05 | **Drop-off confirmation.** Mark each order delivered with the customer's 4-digit PIN, or a photo or confirmation prompt. | P0 |
| FR-D-06 | **Unable to deliver.** Record why (customer unavailable, wrong zone and so on) and start the grace-period rules. | P1 |
| FR-D-07 | **Volunteer history and rewards.** Show completed batches, hours, and the stipends, perks or community hours earned. | P1 |
| FR-D-08 | **Backup list.** Let volunteers sign up as standby for last-minute gaps. | P1 |

### 6.3 Café staff dashboard

| ID | Requirement | Priority |
|---|---|---|
| FR-S-01 | **Batch prep queue.** Total item counts for the locked batch (for example 14x Jollof Rice, 8x Chicken, 6x Meat Pie) plus a packing slip for each order. | P0 |
| FR-S-02 | **Handover view.** Match each packed bag to the deliverer assigned to that zone. | P0 |
| FR-S-03 | **Menu management.** Edit items, prices and stock status. | P0 |
| FR-S-04 | **Mark as ready.** Flag a batch or single orders as ready, which updates the customer's status. | P1 |
| FR-S-05 | **Batch history.** See volumes and item totals per batch and per day. | P2 |

### 6.4 Admin console

| ID | Requirement | Priority |
|---|---|---|
| FR-A-01 | Set the batch schedule, cutoff times and the order cap for each batch. | P0 |
| FR-A-02 | Manage drop-off zones and group them into routes. | P0 |
| FR-A-03 | Approve or suspend volunteer accounts. | P1 |
| FR-A-04 | View the KPI dashboard (Section 10). | P1 |
| FR-A-05 | Handle disputes and refunds, and reassign orders by hand. | P1 |

## 7. Business Rules and Edge Cases

| Situation | Rule |
|---|---|
| **Missed cutoff** | Nobody can be added to a locked batch. The screen shows the next batch time straight away and the user can continue with one tap. |
| **Deliverer no-show** | Deliverers check in 15 minutes before pickup. If someone drops out or doesn't check in, alert the backup volunteers or share their orders among the deliverers who are active for that wave. |
| **Too few volunteers** | Each batch is capped by the number of confirmed deliverers for that slot, so we never over-promise. |
| **Customer not at drop-off** | There's a grace period (5 to 10 minutes at the drop-off point) so one late customer doesn't hold up the rest of the batch. |
| **Batch fills up early** | The batch locks as soon as it reaches capacity, even before the cutoff. It shows as full and the next batch is offered. |
| **No deliverers by cutoff** | If the minimum number of confirmed deliverers isn't met, the batch is cancelled and customers can roll over or get a refund. |
| **Payment not confirmed** | An order only counts toward capacity once payment is confirmed (or pay on delivery is confirmed). Unpaid carts expire after a short hold. |
| **Wrong or missing item** | The customer reports it within a set window. An admin reviews it and issues a credit or refund. |
| **Order left unclaimed** | After the grace period the order is marked undelivered and returned to the café. What happens to the payment still needs to be decided (see Open Questions). |

## 8. Non-Functional Requirements

- **Speed:** Pages load in under 3 seconds on normal campus mobile data, and the countdown stays within a second of the server clock.
- **Reliability:** Batches lock on server time, not the phone's clock. The system has to cope with a rush of orders just before cutoff without losing or duplicating any.
- **Ease of use:** Mobile-first, as few taps as possible to order, and works on low-end phones and weak connections.
- **Security and privacy:** Accounts require login, data is encrypted in transit, deliverers only see what they need to make the delivery, and we follow the data protection rules that apply (for example NDPR in Nigeria).
- **Payments:** No card details are stored on our side. We use a trusted payment provider.
- **Accessibility:** Readable contrast, large tap targets and clear status messages.
- **Availability:** Fully available during batch hours. Maintenance happens outside them.
- **Audit trail:** Every change to a batch or order is timestamped and logged.

## 9. Notifications

WhatsApp and SMS messages (plus in-app alerts) keep everyone in the loop.

| When | Who gets it | What it says |
|---|---|---|
| About 10 minutes before cutoff | Customer | Reminder to finish the order |
| Order placed or rolled over | Customer | Confirmation with batch time and PIN |
| Batch locked and preparing | Customer | The order is being prepared |
| Out for delivery | Customer | The deliverer is on the way, with the drop-off point |
| Deliverer arrives | Customer | The deliverer is at the drop-off point and the grace period has started |
| 15 minutes before pickup | Deliverer | Reminder to check in |
| Batch manifest ready | Deliverer | The manifest is available |
| Batch locked | Café staff | The prep list is available |
| Deliverer no-show | Backup deliverers and admin | Urgent request to cover the orders |

## 10. Success Metrics

| KPI | What it measures | Starting target |
|---|---|---|
| Batch fill rate | Share of available batch slots filled each day | 60% or more |
| On-time delivery rate | Share of orders delivered within the promised batch window | 90% or more |
| Deliverer retention | Share of volunteers who complete 3 or more batches a week | 50% or more |
| Rollover conversion | Share of users who miss a cutoff and still finish their order in the next batch | 60% or more |

These targets are a starting point. We'll adjust them once the pilot shows us what's realistic.

We'll also keep an eye on order error rate, wait time between prep and pickup, deliverer no-show rate, cancelled batches, repeat customers and average rating.

## 11. Data Model (High Level)

| Entity | Key fields |
|---|---|
| User | id, name, role (customer, deliverer, staff, admin), phone, hostel or zone, status |
| Batch | id, date, cutoff time, pickup time, delivery window, capacity, status |
| Order | id, customer, batch, items, total, payment status, drop-off zone, PIN, status |
| OrderItem | id, order, menu item, quantity, customization |
| MenuItem | id, name, price, availability |
| DropOffZone | id, name, type (hostel, library, faculty), route group |
| DelivererShift | id, deliverer, batch, zone, check-in status, backup flag |
| Assignment | id, order, deliverer, pickup confirmed, delivered at |
| Payment | id, order, method, amount, status, reference |

## 12. Release Plan

### 12.1 Phase 1: MVP

The aim is to launch quickly and find out whether there's real demand on campus.

- **Fixed schedule:** 3 batches a day (for example 1:00 PM, 4:00 PM and 7:30 PM).
- **Zone-based drop-offs:** Central hostel gates and common areas only, not individual rooms.
- **Lightweight tools:** A mobile-first web app (PWA) with automatic WhatsApp or SMS alerts for cutoffs and arrivals.
- **Scope:** Everything marked P0 in Section 6.

### 12.2 Phase 2

- Order editing and cancellation, reorder and ratings.
- Rewards dashboard for deliverers and a backup volunteer system.
- Admin KPI dashboard and reports.

### 12.3 Phase 3

- More batches or flexible times, based on what the demand data shows.
- More vendors or campus outlets.
- Route optimization and closer drop-off options.

## 13. Assumptions, Dependencies and Risks

### 13.1 Assumptions

- The café agrees to prepare orders in batches.
- Enough students volunteer to cover at least the busiest batches.
- Students have smartphones, internet access and WhatsApp or SMS.
- Campus administration allows student deliverers at the hostels and gates.

### 13.2 Dependencies

- A payment gateway or a way to verify bank transfers.
- A WhatsApp or SMS provider.
- Hosting, and a reliable server clock for batch timing.
- Campus approval for the drop-off locations and the café partnership.

### 13.3 Risks

| Risk | Impact | What we'll do about it |
|---|---|---|
| Too few volunteers | Batches get cancelled or capped too low | Cap orders by confirmed volunteers, keep a backup list, offer good incentives |
| Volunteers drop off or burn out | Unreliable service | Clear rewards, flexible shifts, watch the retention KPI |
| Café can't keep up with batch volume | Late pickups | Cap each batch, give the café a prep list, stagger batches |
| Food safety during delivery | Health problems and lost trust | Packaging guidelines, delivery time limits, a short briefing for deliverers |
| Payment disputes or fraud | Lost money, mistrust | Verified payments, PIN-based delivery, audit logs |
| Low adoption | Not enough data to learn from | Promote in the hostels, start with the busiest batches, collect feedback |
| Privacy of student contact details | Misuse of data | Share only what's needed, role-based access |
| Weak campus network | Failed orders | Lightweight PWA, retry handling, SMS fallback |

## 14. Open Questions

1. Who pays for delivery? Is it free, a small fee or subsidized, and how are deliverer stipends funded?
2. Is pay on delivery allowed, and how do we handle cash safely?
3. What are the final batch times and cutoff gaps? Is 30 minutes before pickup the right cutoff?
4. What's the order limit per deliverer, and the minimum number of deliverers needed to run a batch?
5. What's the refund policy for undelivered orders?
6. Who approves and vets volunteers (student affairs, the student union)?
7. Can the café run the staff dashboard, for example on a tablet or a spare screen?

## 15. Glossary

- **Batch:** A scheduled delivery wave with a fixed cutoff, prep window and pickup time.
- **Cutoff:** The time a batch stops taking orders and locks.
- **Rollover:** Moving an order to the next batch when the cutoff is missed.
- **Manifest:** The list of orders assigned to a deliverer for a batch.
- **Drop-off zone:** A set campus location where orders are handed over.
- **PIN:** The 4-digit code a customer gives the deliverer to confirm delivery.
- **PWA:** Progressive Web App, a web app that works like a mobile app.
