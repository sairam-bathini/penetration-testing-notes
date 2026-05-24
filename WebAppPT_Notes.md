# Web Application Penetration Testing - Industry Approach with IDOR Demonstration

## Overview

In this session, we will learn how professional penetration testers approach a web application security assessment.

Rather than immediately searching for vulnerabilities, we will first:

* Understand the application
* Identify user roles
* Map application functionality
* Create a testing strategy
* Perform a practical IDOR (Insecure Direct Object Reference) assessment using Burp Suite and OWASP Juice Shop

The goal is to demonstrate how penetration testing is performed in real-world environments rather than simply showcasing a vulnerability.

---

# Agenda

1. What is Penetration Testing?
2. Why Organizations Perform Penetration Testing
3. Real-World Penetration Testing Lifecycle
4. Introduction to OWASP Juice Shop
5. Application Mapping
6. HTTP Fundamentals
7. API Identification and Analysis
8. Creating a Test Plan
9. Understanding IDOR
10. Practical IDOR Demonstration
11. Reporting the Finding
12. Tips for Aspiring Penetration Testers

---

# Prerequisites

Participants should have a basic understanding of:

* Client-Server Architecture
* HTTP Requests and Responses
* Web Browsers
* Basic Cybersecurity Terminology

---

# Lab Environment

## Client

* Google Chrome Browser

## Proxy

* Burp Suite Community/Professional

## Server

* AWS EC2 Instance

## Vulnerable Application

* OWASP Juice Shop
* Hosted using Docker

### Request Flow

```text
Browser
   ↓
Burp Suite Proxy
   ↓
OWASP Juice Shop (AWS EC2)
   ↓
Response
   ↓
Browser
```

---

# What is Penetration Testing?

Penetration Testing is the process of simulating real-world attacks against an application to identify security weaknesses before attackers can exploit them.

### Objective

The objective is NOT to break the application.

The objective is to:

* Identify security risks
* Assess business impact
* Provide remediation guidance
* Improve overall security posture

---

# Why Organizations Perform Penetration Testing

Organizations conduct penetration testing to:

* Identify vulnerabilities before attackers
* Meet compliance requirements
* Protect customer data
* Protect business reputation
* Improve overall security posture

---

# Real-World Penetration Testing Lifecycle

## Step 1: Understand the Business

Before testing begins, understand:

* What does the application do?
* Who are the users?
* What are the critical business workflows?
* What data is considered sensitive?

---

## Step 2: Define Scope

### In Scope

* Application URLs
* APIs
* Mobile APIs (if applicable)

### Out of Scope

* Production Databases
* Third-Party Integrations
* Support Portals
* External Systems

---

## Step 3: Obtain Test Accounts

Example Roles:

* Customer User
* Support User
* Administrator

---

## Step 4: Create a Role Matrix

| Function      | Support | Customer | Admin |
| ------------- | ------- | -------- | ----- |
| Login         | Yes     | Yes      | Yes   |
| View Orders   | No      | Yes      | Yes   |
| Delete Orders | No      | No       | Yes   |

This matrix becomes the foundation for Authorization Testing.

---

# Introduction to OWASP Juice Shop

OWASP Juice Shop is an intentionally vulnerable e-commerce application designed for security training and penetration testing practice.

### Demonstration Areas

* User Registration
* User Login
* Product Browsing
* Shopping Basket
* Order History
* User Profile

---

# Application Mapping

## Configure Burp Suite

1. Configure Browser Proxy
2. Intercept Requests
3. Browse the Application
4. Analyze Traffic

### Key Areas to Observe

* Site Map
* HTTP History
* Endpoints
* APIs
* Authentication Mechanisms

### Important Mindset

At this stage, we are NOT looking for vulnerabilities.

We are trying to understand how the application works.

---

# HTTP Fundamentals

## Common HTTP Methods

| Method | Purpose               |
| ------ | --------------------- |
| GET    | Retrieve Data         |
| POST   | Create Data           |
| PUT    | Update Data           |
| PATCH  | Partially Update Data |
| DELETE | Delete Data           |

---

## Common HTTP Status Codes

| Code | Meaning                          |
| ---- | -------------------------------- |
| 200  | Success                          |
| 201  | Resource Created                 |
| 401  | Authentication Required          |
| 403  | Authenticated but Not Authorized |
| 404  | Resource Not Found               |
| 500  | Internal Server Error            |

### Authentication vs Authorization

#### 401 Unauthorized

The application does not know who you are.

Example:

```http
GET /api/orders/1001
```

Without a valid session or token:

```http
HTTP/1.1 401 Unauthorized
```

---

#### 403 Forbidden

The application knows who you are but does not allow access.

Example:

User A attempts to access User B's order.

```http
HTTP/1.1 403 Forbidden
```

---

#### 404 Not Found

The requested resource does not exist or is intentionally hidden.

```http
HTTP/1.1 404 Not Found
```

---

# API Identification

## Common API Indicators

```text
/api/
/v1/
/v2/
/graphql
/rest
```

Examples:

```text
/api/users
/api/orders
/api/products
/api/basket
```

---

# Understanding HTTP Requests

Whenever you intercept a request, analyze:

### 1. HTTP Method

```http
GET
POST
PUT
PATCH
DELETE
```

### 2. URL Path

Example:

```http
/api/orders/1001
```

### 3. Query Parameters

Example:

```http
/api/orders?id=1001
```

Questions:

* What does the ID represent?
* Can it be modified?
* Does it reference another user?

### 4. Headers

Examples:

```http
Authorization:
Cookie:
Content-Type:
Origin:
```

### 5. Request Body

Example:

```json
{
  "userId": 123
}
```

---

# Creating a Test Plan

## Possible Testing Categories

* Authentication Testing
* Authorization Testing
* Session Management Testing
* Business Logic Testing
* API Security Testing
* Input Validation Testing

For this session, we will focus on Authorization Testing.

---

# Understanding IDOR

## What is IDOR?

IDOR stands for Insecure Direct Object Reference.

It occurs when an application exposes references to internal objects without properly validating authorization.

---

## Example

User A accesses:

```text
/orders/1001
```

User A modifies the identifier:

```text
/orders/1002
```

If the application returns another user's data, an IDOR vulnerability exists.

---

## Impact

* Data Exposure
* Privacy Violations
* Unauthorized Access
* Regulatory Compliance Issues

---

# Practical Demonstration

## Step 1

Create User A

## Step 2

Create User B

## Step 3

Login as User A

## Step 4

Intercept traffic using Burp Suite

## Step 5

Identify user-specific requests

Examples:

* User ID
* Basket ID
* Order ID
* Address ID

## Step 6

Send the request to Repeater

## Step 7

Modify the identifier

## Step 8

Observe the response

## Questions to Ask

* Can User A access User B's data?
* Can User A modify User B's data?
* Can User A delete User B's data?

---

# Reporting the Finding

## Title

Insecure Direct Object Reference (IDOR)

## Severity

High

## Description

The application fails to validate whether the authenticated user is authorized to access the requested resource.

## Impact

An attacker may gain unauthorized access to sensitive information belonging to other users.

## Recommendation

Implement server-side authorization checks for every object access request.

## Evidence

Include:

* Request
* Response
* Screenshots
* Reproduction Steps

---

# Tips for Aspiring Penetration Testers

* Never start testing before understanding the application.
* Spend time learning how the application works.
* Create a Role Matrix before performing authorization testing.
* Understand APIs because modern applications are API-driven.
* Focus on business workflows, not just vulnerabilities.
* Use tools to assist your work, not replace your thinking.
* Always validate findings before reporting.
* Think like a legitimate user first, then think like an attacker.

---

# Key Takeaway

A professional penetration tester does not start by looking for vulnerabilities.

A professional penetration tester starts by understanding:

* The business
* The users
* The workflows
* The application architecture

Only then do they begin identifying security weaknesses.
