---
Creation Time: Saturday, February 15th 2025
Modified Time: Friday, May 9th 2025
---
Problem statement:

1. Too many 3rd party calls in parallel and in sequence.
2. Heigh concurrent  request on the service
3. Large payload transformation based on third party responses delays

Capacity estimates: 

Build integration service in reactive programming using java reactive programming along with Apache airflow for managing workflows..

[[Reactive programming]]

## Kotlin vrs JAVA

>Eliminates boilerplate (constructors, getters, setters).
>Kotlin has **built-in null safety**, preventing many runtime crashes due to `NullPointerException`.
>Coroutines for Asynchronous Programming (Better than Java’s Threads)**
>Java uses **Threads** or **CompletableFutures** for concurrency, which can be **complex**. Kotlin’s **Coroutines** provide an **easier, lightweight, and non-blocking** way to handle async tasks.
>Improves **code readability and maintainability**.
Kotlin **automatically detects types**, reducing redundant code.
>Kotlin supports **higher-order functions, lambdas, and collection operators** natively, making functional programming **easier** than in Java.
>**Spring Boot supports Kotlin natively**, leading to **faster development** and **concise APIs** in backend systems.
>Interoperability with Java (100% Compatible)
>You can **mix Kotlin and Java code in the same project**.


### **When is Java Still Better?**

✔ **Larger Ecosystem & Legacy Code:** Java has been around since 1995, meaning **more libraries, frameworks, and legacy systems** are built in Java.  
✔ **Performance for Large Enterprise Systems:** Java can be **slightly faster** for certain high-performance applications due to mature JIT optimizations.  
✔ **Team Experience:** If your team is deeply experienced in Java and there’s no need for Kotlin’s advantages, **sticking to Java may be more practical**.



**Project: Axis Bank Module: VIKI**

- Worked on redesign of the existing monolithic PHP web application to an microservice architecture.
- Design and developed gateway to connect Axis internal module with External Bureaus.
- Integrated with bureaus for fetching User Score details from Cibil and caching it for further use
    cases.
- Integrated with several other internal and external API to serve different purpose.
- Worked on performance evaluation and improvement as external bureaus API response average time was 35 Sec
    
**Project: Titan Leader board**

- Designed and developed Leader board for sports and events scheduled for a group.
- Worked on event scheduling, result evaluation, event notification, real time evaluation of user
    performance etc.
- Implemented notification system for sending notification to for sport events and achievements.


    **Project: Manipal**  
    **Module: Manipal Microsoft assessment server:**
    

- This was a web-based exam portal built using Django and python to conduct Microsoft assessments.
- Worked end to end backend implementation.
- Implemented integration with AWS service S3 to store exam events log and candidate results.
- Implemented Lamda function to parse proctor and candidates' logs uploaded in S3 to generate
real time exam status dashboard.



Paytm, Noida [ SEPT 2016 – JUNE 2019 ] Software Engineer

1. Payment-Gateway Automation & automatic build deployment:
    - Developed Automation suite for UI testing of all Payment Gateway flows.
    - Automated all payment flows (Wallet, Auth, PG) and Admin panel APIs
    - Worked on setting and executing a suite on selenium Grid.
    - Coordination with a Functional testing team for new features and functionalities.
    - Mentoring and training new team members.
    - Automation build a deployment process for all QA environment using Jenkins, Shell script and Java.
    - Worked on setting up docker for build deployment on QA environment.
2. Mock service:
    
• This is REST API service using a Spring boot framework to mock response from external modules such as external Banks (HDFC, ICICI), Paytm bank, Paytm postpaid, Merchant
3. Server and Environment setup.
- Setup and managed HA-Proxy services for load balance
- Setup and monitored Zabbix for QA environment.
- Setup Kibana for easy logging on Automation Environment.
- Creating and Maintaining a developer and QA infrastructure which is hosted on Open
    

stack platform.


Module: Manipal Proctor Portal

• Implemented one to one and one to many real time communications between proctor and candidates to conduct an online tutorial using WebRTC and Kurento Media Server in JAVA.