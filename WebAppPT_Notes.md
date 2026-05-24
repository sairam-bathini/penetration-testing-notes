**Web Application Penetration Testing:**



In this session, we will learn how professional penetration testers approach a web application assessment. Rather than immediately searching for vulnerabilities, we will first understand the application, identify user roles, map functionality, create a testing strategy, and then perform a practical IDOR (Insecure Direct Object Reference) assessment using Burp Suite and OWASP Juice Shop.



**Agenda**



1\. What is Penetration Testing?

2\. Why Organizations Perform Penetration Testing

3\. Real-World Penetration Testing Lifecycle

4\. Understanding OWASP Juice Shop

5\. Application Mapping

6\. IDOR Vulnerability Theory

7\. IDOR Practical Demonstration using Burp Suite

8\. Reporting the Finding

\---------------------------------------------------------------------------------------------------------------

**Pre-requisites:** Client - Server model, Basic Terminologies



Client (From where you are accessing the application): Chrome Browser

Request (to)

+Burpsuite (proxy)

Server: AWS EC2 instance

Response (from)



**OWASP Juice Shop:** 



**E-commerce application:** OWASP Juice Shop (Vulnerable) Open-source (Hosted on AWS using Docker Image of Juice Shop)

**Tools Used:** Burp Suite (Proxy Tool)

\---------------------------------------------------------------------------------------------------------------

**What is Penetration Testing? (Public Facing: Crown Jewels)**



Penetration Testing is the process of simulating real-world attacks against an application to identify security weaknesses before attackers can exploit them.



The objective is **not to break the application**.



The objective is to **identify security risks** and provide remediation guidance.

\---------------------------------------------------------------------------------------------------------------

**Why Organizations Perform Penetration Testing?**



To:



Identify vulnerabilities before attackers

Meet compliance requirements

Protect customer data

Protect business reputation

Improve overall security posture

\---------------------------------------------------------------------------------------------------------------

**How to do Penetration Testing?**



Before testing, professional penetration testers perform several activities.



**Step 1: Understand the Business**



Questions:



What does the application do?

Who are the users?

What are the critical workflows?

What data is sensitive?



**Step 2: Define Scope**



Examples:



In Scope:

\* Application URL

\* APIs



Out of Scope:

\* Production databases

\* Third-party integrations

\* Support pages



**Step 3: Obtain Test Accounts**



Examples:

Customer User

Admin User

Support User



**Step 4: Create a Role Matrix**



| Function      | Suppt | Customer | Admin |

| ------------- | ----- | -------- | ----- |

| Login         | Yes   | Yes      | Yes   |

| View Orders   | No    | Yes      | Yes   |

| Delete Orders | No    | No       | Yes   |



This matrix serves as the foundation for authorisation testing.

\---------------------------------------------------------------------------------------------------------------

**Start with Demo: Introduction to OWASP Juice Shop**



Demonstrate:



\* Registration

\* Login

\* Product browsing

\* Order history

\---------------------------------------------------------------------------------------------------------------

**Application Mapping**



Open Burp Suite.

Configure Proxy.

Browse the application.



Explain:

Site Map

HTTP History

Endpoints

API Calls



**Common API Indicators**



When you see:



/api/

/v1/

/v2/

/graphql

/rest



It is usually an API endpoint.



Examples:



/api/users

/api/orders

/api/products

/api/basket



**HTTP Methods:**

Method	Purpose

GET	Read Data

POST	Create Data

PUT	Update Data

PATCH	Partial Update

DELETE	Delete Data



**Status Codes:**

Code	Meaning

200	Success

201	Created

401	Unauthorized (Authentication Required)

403	Forbidden (Authenticated but Not Authorized)

404	Not Found (The requested resource does not exist)

500	Server Error



As a penetration tester, we are not looking for vulnerabilities yet.



We are trying to understand how the application works.

\---------------------------------------------------------------------------------------------------------------

**Creating a Test Plan**



**Possible Test Categories:**



Authentication Testing

Authorization Testing

Session Management Testing

Business Logic Testing

API Security Testing

Input Validation Testing



For this session, we will focus on Authorisation Testing.

\---------------------------------------------------------------------------------------------------------------

**Understanding IDOR**



IDOR stands for Insecure Direct Object Reference.



It occurs when an application exposes references to internal objects without properly validating user authorization.



**Example:**



User A can access:



/orders/1001



If User A changes the identifier to:



/orders/1002



and gains access to another user's data, an IDOR vulnerability exists.



Impact:



\* Data Exposure

\* Privacy Violations

\* Unauthorized Access

\* Compliance Issues

\---------------------------------------------------------------------------------------------------------------

Practical Demonstration



Create User A.



Create User B.



Login as User A.



Intercept traffic using Burp Suite.



Navigate to a feature that references user-specific data.



Send the request to Repeater.



Can User A access User B's data?



**Identify:**



\* User ID

\* Basket ID

\* Order ID

\* Resource Identifier



**Modify the identifier.**



Observe the response.

\---------------------------------------------------------------------------------------------------------------

**Reporting the Finding**



Title:

Insecure Direct Object Reference (IDOR)



Severity:

High



Description:

The application fails to verify whether the authenticated user is authorised to access the requested resource.



Impact:

An attacker may access data belonging to other users.



Recommendation:

Implement server-side authorisation validation for every object access request.



Evidence:

Include request, response, screenshots, and reproduction steps.



\---------------------------------------------------------------------------------------------------------------

**Important Tips for Aspiring Penetration Testers**



Never start testing before understanding the application.

Create a role matrix before authorization testing.

Understand APIs because modern applications are API-driven.

Use tools to assist your work, not to replace your thinking.

Always validate findings before reporting.

Think like a legitimate user first, then think like an attacker.

