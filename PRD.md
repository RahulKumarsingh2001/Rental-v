# 📄 Product Requirements Document (PRD)

## 📌 Product Name

**RideShareX**
*A Production-Ready Peer-to-Peer Long-Term Vehicle Rental Platform*

---

# 1. 🧠 Problem Statement

Urban vehicle usage inefficiency leads to:

* Idle vehicles (weeks/months unused)
* High rental costs for temporary users
* Lack of structured and trusted P2P vehicle sharing

### Key Gaps:

* No **long-term (7–60 days) rental platform**
* No **trust layer (KYC, accountability)**
* No **structured booking + approval system**

👉 Opportunity: Build a **trusted, structured, long-term rental marketplace**

---

# 2. 🎯 Product Goal

Deliver a **secure, scalable, and deployable platform** where:

* Owners monetize idle vehicles
* Renters access affordable mobility
* Platform ensures **trust, tracking, and accountability**

---

# 3. 👥 User Roles

### 3.1 Renter

* Searches & books vehicles
* Pays cash at pickup
* Responsible for vehicle usage

### 3.2 Owner

* Lists vehicles
* Approves bookings
* Handles handover & return

### 3.3 Admin

* Verifies users & vehicles
* Handles disputes
* Controls platform integrity

---

# 4. 🧩 Core Modules

---

## 4.1 👤 Authentication & KYC

### Features:

* Email + OTP login
* JWT-based session
* Mandatory KYC:

  * Aadhaar
  * Driving License

### States:

* `PENDING`
* `VERIFIED`
* `REJECTED`
* `BLOCKED`

### Edge Cases:

* Duplicate accounts (same DL/Aadhaar)
* Fake document upload
* OTP abuse → rate limiting
* Session expiry / token misuse

---

## 4.2 🚗 Vehicle Listing Module

### Owner Inputs:

* Vehicle details (brand, model, fuel)
* Registration number (unique)
* Documents:

  * RC
  * Insurance
* Pricing (daily/weekly)
* Availability calendar
* Pickup location (lat/lng optional)

### States:

* `DRAFT`
* `UNDER_REVIEW`
* `ACTIVE`
* `REJECTED`
* `BLOCKED`

### Validations:

* Duplicate vehicle registration
* Expired insurance
* Missing documents

### Edge Cases:

* Owner edits listing after booking exists
* Vehicle becomes unavailable mid-request
* Multiple listings for same vehicle

---

## 4.3 🔍 Search & Discovery

### Features:

* Location-based search
* Date range filtering
* Filters:

  * Price
  * Vehicle type
* Sorting:

  * Price
  * Rating

### Edge Cases:

* No vehicles available
* Invalid date range
* Overlapping bookings
* Stale cached results

---

## 4.4 📅 Booking System (Critical Module)

### Flow:

1. Select vehicle + dates
2. Check availability
3. Calculate price
4. Create booking request

### Booking States:

* `PENDING`
* `ACCEPTED`
* `REJECTED`
* `CANCELLED`
* `EXPIRED`
* `ONGOING`
* `COMPLETED`

### Rules:

* Prevent overlapping bookings
* Lock dates during `PENDING` (optional)
* Auto-expire request after 24 hrs

### Edge Cases:

* Double booking race condition
* Owner accepts after expiry
* Renter cancels after acceptance
* Booking during blocked user state

---

## 4.5 🤝 Approval System

### Owner Actions:

* Accept
* Reject

### System Behavior:

* Notify renter (email/notification)
* Auto-expire if no response

### Edge Cases:

* Owner inactive
* Spam booking requests
* Multiple pending requests for same dates

---

## 4.6 💵 Payment System (Offline Only)

### Design:

* No online gateway
* Payment handled physically

### Components:

* Rental amount
* Security deposit

### System Tracking:

* `paymentStatus`:

  * `PENDING`
  * `CONFIRMED`

### Edge Cases:

* Owner claims payment not received
* Renter claims overcharge
* Deposit disputes
* Partial payments

👉 Solution:

* Manual confirmation + optional photo proof

---

## 4.7 📦 Handover & Return System

### Handover Checklist:

* Identity verification
* Payment confirmation
* Vehicle condition photos

### Return Checklist:

* Damage inspection
* Fuel level check
* Deposit settlement

### Edge Cases:

* Vehicle damaged
* Late return
* Missing accessories
* Dispute over condition

---

## 4.8 ⭐ Review & Rating System

### Rules:

* Only after completed booking
* Both sides rate each other

### Edge Cases:

* Fake reviews
* Abuse/spam reviews
* One-sided reviews

---

## 4.9 👨‍💼 Admin Panel

### Capabilities:

* Verify KYC
* Approve/reject vehicles
* Block users
* Handle disputes
* Monitor bookings
* Audit logs

### Edge Cases:

* Fraud detection
* Mass spam users
* Legal complaints

---

# 5. 🗄️ MongoDB Data Models

## User

* _id
* name
* email
* role
* kycStatus
* isBlocked
* createdAt

## Vehicle

* _id
* ownerId
* registrationNumber (unique)
* details
* price
* availability
* status

## Booking

* _id
* vehicleId
* renterId
* startDate
* endDate
* status
* paymentStatus
* totalAmount

## Review

* _id
* fromUser
* toUser
* rating
* comment

## AuditLog (IMPORTANT)

* userId
* action
* timestamp
* metadata

---

# 6. 🔐 Security & Abuse Prevention

* Rate limiting (OTP, login)
* Input validation (backend)
* JWT expiration & refresh
* Role-based access control
* Activity logging
* Blacklist users

---

# 7. ⚙️ System Workflows

---

## Renter Flow:

1. Register → KYC
2. Search vehicle
3. Send request
4. Wait approval
5. Meet owner
6. Pay cash
7. Use vehicle
8. Return
9. Review

---

## Owner Flow:

1. Register → KYC
2. Add vehicle
3. Accept booking
4. Meet renter
5. Collect cash
6. Handover
7. Inspect return
8. Review

---

# 8. 📊 KPIs

* Active users
* Listings count
* Booking success rate
* Retention rate
* Avg booking duration

---

# 9. ⚠️ Production-Level Risks

| Risk             | Solution                   |
| ---------------- | -------------------------- |
| Fraud users      | Strong KYC + admin checks  |
| Payment disputes | Manual confirmation + logs |
| Vehicle damage   | Deposit + checklist        |
| Low supply       | Target niche onboarding    |
| Legal issues     | Start compliant            |

---

# 10. 🚀 Deployment Considerations

* Environment configs (.env)
* Secure API routes
* Cloud storage setup
* Database indexing:

  * vehicleId
  * dates
* Backup strategy
* Error logging (Winston / Log service)

---

# 11. 🔮 Future Scope

* Online payments
* Mobile app
* GPS tracking
* Insurance integration
* AI pricing

---

# 12. 🧠 USP

* Long-term rental focus
* Trust-driven system
* Affordable alternative to rentals

---

# 13. 🏁 Final Conclusion

RideShareX is a **scalable, deployable marketplace system** designed with:

* Real-world edge cases
* Strong backend structure
* Trust & safety mechanisms

👉 Strategy:
**Build MVP → Deploy → Learn → Scale**
