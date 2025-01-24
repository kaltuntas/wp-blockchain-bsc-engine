# ING Credit Module

## Overview
The ING Credit Module is a software solution designed to manage customer loans. It allows users to:

- Create loans for customers
- List existing loans
- Process loan payments

This project is built with Java and Spring Boot and uses Maven for dependency management and builds.

## Features

- **Loan Creation**: Easily create loans for customers with necessary details.
- **Loan Listing**: Retrieve a list of all loans in the system.
- **Loan Payments**: Process payments for existing loans.
- **API Documentation**: Integrated Swagger UI for exploring and testing the API.
- **Authentication**: Secured with basic authentication (username and password).
- **In-Memory Database**: Uses H2 database; tables and dummy data are automatically created on application startup.

---

## Installation

To set up the ING Credit Module on your local machine, follow these steps:

### Prerequisites
- Java Development Kit (JDK) 17 or later
- Maven 3.6 or later

### Steps
1. Clone the repository:
   ```bash
   git clone <repository-url>
   cd ing-credit-module
   ```

2. Build the project using Maven:
   ```bash
   mvn clean install
   ```

3. Run the application:
   ```bash
   mvn spring-boot:run
   ```

The application should now be running locally on `http://localhost:8080`.

---

## Usage

The ING Credit Module exposes RESTful APIs for interacting with the system. Below are some examples of how to use these APIs with `curl`.

### Authentication
The APIs are secured using basic authentication. Use the `--user` flag in `curl` to pass the username and password. Replace `username` and `password` with `user` and `12345`.

### 1. Create a Loan
```bash
curl -X POST http://localhost:8080/api/loans \
--user username:password \
-H "Content-Type: application/json" \
-d '{
  "customerId": 123,
  "amount": 5000,
  "term": 12
}'
```

### 2. List Loans
```bash
curl -X GET http://localhost:8080/api/loans \
--user username:password
```

### 3. Make a Payment
```bash
curl -X POST http://localhost:8080/api/loans/123/payments \
--user username:password \
-H "Content-Type: application/json" \
-d '{
  "paymentAmount": 500
}'
```

---

## API Endpoints

### Loan Management
- `GET /api/v1/loans/` - List loans of a customer
- `GET /api/v1/loans/{loanId}/installments` - List loan installments of a loan
- `POST /api/v1/loans` - Create a new loan for a customer
- `POST /api/v1/loans/pay` - Pay a loan

---

## API Documentation

The ING Credit Module includes Swagger UI for exploring and testing the API. Once the application is running, navigate to:

[http://localhost:8080/swagger-ui.html](http://localhost:8080/swagger-ui.html)

Here, you can view detailed API documentation and interact with the endpoints directly.

---

## Database Schema

The ING Credit Module uses an H2 in-memory database. Upon application startup, the following tables are created along with some dummy data:

### `customer` Table
```sql
CREATE TABLE IF NOT EXISTS customer
(
    id                BIGINT         NOT NULL AUTO_INCREMENT,
    idate             TIMESTAMP      NOT NULL,
    udate             TIMESTAMP DEFAULT NULL,
    name              VARCHAR(50)    NOT NULL,
    surname           VARCHAR(50)    NOT NULL,
    credit_limit      DECIMAL(15, 2) NOT NULL,
    used_credit_limit DECIMAL(15, 2) NOT NULL,
    PRIMARY KEY (id)
);
```

### `loan` Table
```sql
CREATE TABLE IF NOT EXISTS loan
(
    id                    BIGINT         NOT NULL AUTO_INCREMENT,
    idate                 TIMESTAMP      NOT NULL,
    udate                 TIMESTAMP DEFAULT NULL,
    customer_id           BIGINT         NOT NULL,
    loan_amount           DECIMAL(15, 2) NOT NULL,
    number_of_installment SMALLINT       NOT NULL,
    is_paid               BOOLEAN        NOT NULL,
    interest_rate         DECIMAL(15, 2) NOT NULL,
    PRIMARY KEY (id),
    FOREIGN KEY (customer_id) REFERENCES customer (id)
);
```

### `loan_installment` Table
```sql
CREATE TABLE IF NOT EXISTS loan_installment
(
    id           BIGINT         NOT NULL AUTO_INCREMENT,
    idate        TIMESTAMP      NOT NULL,
    udate        TIMESTAMP DEFAULT NULL,
    loan_id      BIGINT         NOT NULL,
    amount       DECIMAL(15, 2) NOT NULL,
    paid_amount  DECIMAL(15, 2) NOT NULL,
    due_date     DATE           NOT NULL,
    payment_date TIMESTAMP DEFAULT NULL,
    is_paid      BOOLEAN        NOT NULL,
    PRIMARY KEY (id),
    FOREIGN KEY (loan_id) REFERENCES loan (id)
);
```

---

## Project Structure

```plaintext
src/main/java
  |-- com.ing.creditmodule
       |-- controllers  # API controllers
       |-- services     # Business logic
       |-- repositories # Data access
       |-- models       # Entity and DTO classes
```

---
