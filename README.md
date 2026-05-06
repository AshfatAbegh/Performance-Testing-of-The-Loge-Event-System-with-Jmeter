# 🚀 Load Testing with Apache JMeter — Event System 

This repository documents a **real-world load testing strategy** implemented using Apache JMeter for an **Event Management System**.

---

## 📌 Overview

The system under test includes the following APIs:

- 🔐 User Registration API  
- 🔑 User Login API  
- 🗓️ Event Creation API  
- 🎟️ Event Registration API  

---

## 🎯 Objectives

- Simulate up to **1000 users**
- Validate **system performance under normal load**
- Identify issues under **high concurrency (same event registration)**
- Detect:
  - Race conditions  
  - Duplicate registrations  
  - Capacity overflow  
  - Performance degradation  

---

## 🏗️ Architecture Overview

Users (JMeter) | ↓ Backend API Server | ↓ Application Layer (Event Service) | ↓ Database (Users, Events, Event Registrations)


---

## 🔄 End-to-End Test Flow

User Registration ↓
User Login (Token Extraction) ↓
Event Creation (Setup) ↓
Extract eventId ↓
Event Registration (Load Test Target)


---

## ⚙️ Test Plan Structure

Test Plan
│
├── Setup Thread Group
│ ├── User Registration
│ ├── User Login (extract token)
│ ├── Create Events
│ └── Extract eventId
│
├── Thread Group 1 → Concurrent Registration
│
├── Thread Group 2 → Normal Registration
│
└── Listeners (Reports)


---

## 🧪 Setup Thread Group

**Purpose:** Prepare test data

### Configuration:
- Threads: 100  
- Ramp-up: 60 sec  
- Loop Count: 10  

### Tasks:
- Register users  
- Login and extract tokens  
- Create events  
- Store `eventId` for later use  

---

## 📂 Test Data Strategy

### users.csv

user1
user2
user3
...


### eventIds.csv

#### 🔥 Concurrency Test

101 101 101 101


#### 🌤️ Normal Load Test

101
102
103
104


---

## 🔥 Concurrent Registration Test

### 🎯 Goal:
Simulate **multiple users registering for the same event simultaneously**

User1 ─┐
User2 ─┤
User3 ─┤
... ├──→ Register API → Event 101
User200┘


### Configuration:
- Users: 600  
- Ramp-up: 20 sec
- Ramp-up Steps Count: 5
- Hold Target Rate Time(sec): 500

### Validations:
- No duplicate registrations  
- No overselling beyond capacity  
- Acceptable error handling (e.g., event full)  

---

## 🌤️ Normal Load Test

### 🎯 Goal:
Simulate **real-world gradual traffic**

User1 → Event 101
User2 → Event 102
User3 → Event 103
User4 → Event 104


### Configuration:
- Users: 400  
- Ramp-up: 120 sec  
- Loop Count: 1  

### Validations:
- Stable response time  
- Low error rate  
- Consistent throughput  

---

## ⚖️ Concurrency vs Normal Load

| Feature        | Concurrency Test | Normal Load Test |
|---------------|------------------|------------------|
| Ramp-up       | Fast (5–10 sec)  | Slow (120–180 sec) |
| Event IDs     | Same             | Multiple          |
| Purpose       | Bug detection    | Performance check |
| Behavior      | Spike            | Gradual           |

---

## 📊 Key Metrics Monitored

- ⏱ Response Time (avg, p90, p95)
- 📈 Throughput (requests/sec)
- ❌ Error Rate (%)
- 👥 Active Threads

---

## 🔍 JMeter Listeners Used

- Summary Report  
- Aggregate Report  
- Graph Results  
- View Results Tree (for debugging only)  

---

## 🧠 Important Concepts

### 🔹 Correlation
Extract dynamic values:
- `eventId`
- `auth token`

---

### 🔹 Test Data Design
- Unique users  
- Controlled event distribution  

---

### 🔹 Realistic Behavior
- Think time (optional)  
- Gradual ramp-up  

---

## ⚠️ Common Mistakes

- ❌ Skipping login/authentication  
- ❌ Using same user repeatedly  
- ❌ Not handling dynamic data  
- ❌ Ignoring failed responses  
- ❌ Jumping to high load without validation  

---

## 🧪 Execution Strategy

Smoke Test (1 user)
Small Load (10–50 users)
Normal Load (200 users)
Concurrency Test (200–600 users)
Stress Test (600+ users)

---

## 📊 Advanced Scenario
Total Users = 1000
600 → Hot Event (Concurrency) 400 → Normal Events


👉 Simulates real production traffic distribution

---

## 🧠 Final Thoughts

Load testing is not just about executing requests.

It’s about:
- Designing realistic scenarios  
- Understanding system behavior  
- Identifying breaking points before users do  

---

## 🛠️ Tools Used

- Apache JMeter  
- CSV Data Set Config  
- JSON Extractor  






