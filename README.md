# PC Component Price Monitor

A Spring Boot application that automatically monitors the prices of PC components across multiple online stores and notifies users when a product reaches or falls below a target price.

Although the example focuses on **computer hardware**, the application is generic and can monitor the price of virtually any product by configuring its URL and CSS selectors.

---

# Features

## Automatic Monitoring

* Periodically checks product prices (default: every **1 minute**)
* Monitoring interval can be configured through Spring Scheduling

---

## Multi-Store Support

The system can monitor any website, provided the correct CSS selectors are supplied.

Example stores include:

* Amazon
* eBay
* CompraGamer
* PCComponentes
* Newegg
* Best Buy

---

## Price Alerts

When the current price is **equal to or lower** than the configured target price, the application generates a notification.

(Current implementation prints alerts to the console.)

---

## Price History

Every detected price is stored, allowing historical price tracking over time.

---

## Web Scraping

Uses **Selenium** together with configurable CSS selectors to extract product prices from web pages.

---

# Technology Stack

* **Java**
* **Spring Boot**
* **Spring Data JPA**
* **Hibernate**
* **MySQL**
* **Selenium**
* **Spring Scheduling**

---

# System Architecture

```text id="3x3rdb"
Scheduler
     │
     ▼
Monitoring Service
     │
     ▼
 Selenium WebDriver
     │
     ▼
Online Store
     │
     ▼
Price Extractor
     │
     ▼
 MySQL Database
     │
     ▼
 Alert System
```

---

# REST API

## Add a Component

```http id="a9vs5w"
POST /api/componentes/addComponente
```

---

## Get Component by ID

```http id="9yvhzi"
GET /api/componentes/{id}
```

---

## Delete Component

```http id="w4gvcz"
DELETE /api/componentes/{id}
```

---

## Run Monitoring Manually

```http id="3n9zqx"
GET /api/componentes/ejecutarMonitoreo/{id}
```

---

# Automatic Monitoring

The application automatically executes the monitoring task every **minute** using **Spring Scheduling**.

The interval can easily be modified by changing the value in the `@Scheduled` annotation.

Whenever a target price is reached, an alert is displayed.

---

# Data Model

## Component

Represents a monitored product.

| Field         | Description                           |
| ------------- | ------------------------------------- |
| ID            | Unique identifier                     |
| Name          | Product name                          |
| Store         | Online store                          |
| URL           | Product page                          |
| CSS Selectors | Candidate selectors used for scraping |
| Target Price  | Desired purchase price                |

---

## Price Record

Represents one detected price.

| Field     | Description             |
| --------- | ----------------------- |
| ID        | Unique identifier       |
| Price     | Detected price          |
| Timestamp | Detection date and time |
| Component | Associated product      |

---

## Price History

Each component maintains a collection of historical price records, allowing trend analysis over time.

---

# Example JSON Configuration

The application requires product definitions containing the product URL and CSS selectors used to locate the price on each website.

Example:

```json id="v3b4gv"
[
  {
    "nombre": "NVIDIA GeForce RTX 5060 Ti",
    "tienda": "Amazon",
    "precioObjetivo": 450000.0,
    "url": "https://www.amazon.com/s?k=RTX+5060+Ti",
    "selectores": [
      ".a-price-whole",
      ".a-price .a-offscreen",
      ".a-price.a-text-price.a-size-medium.apexPriceToPay",
      ".tw-text-price",
      "span[class*='tw:text-price']"
    ]
  },
  {
    "nombre": "Corsair Vengeance DDR5 32GB",
    "tienda": "Amazon",
    "precioObjetivo": 110000.0,
    "url": "...",
    "selectores": [
      ".a-price-whole",
      ".a-price .a-offscreen"
    ]
  }
]
```

Each product can define multiple CSS selectors, improving resilience when websites change their HTML structure.

---

# Workflow

1. Scheduler triggers the monitoring task.
2. Selenium opens the product page.
3. CSS selectors are evaluated until a valid price is found.
4. The detected price is stored in the database.
5. The application compares the current price with the target price.
6. If the price meets the condition, an alert is generated.

---

# Future Improvements

* Email notifications
* Telegram and Discord alerts
* Browser notifications
* Support for Playwright
* Docker deployment
* Price trend charts
* User authentication
* Multiple monitored products per user
* REST API documentation with Swagger
* Scheduled reports

---

# Author

**Juan**

## Version

**1.0.0**

