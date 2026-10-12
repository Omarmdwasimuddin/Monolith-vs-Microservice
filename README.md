## Monolith vs Microservices Architecture

> **মূল কথা:** Monolith হলো একটি deployable application-এর মধ্যে একাধিক module; Microservices হলো একাধিক স্বাধীন service মিলে তৈরি একটি system।

## ১. Monolith Architecture কী?

Monolith-এ application-এর প্রধান feature-গুলো সাধারণত একই project এবং deployable unit-এর মধ্যে থাকে। উদাহরণস্বরূপ, E-commerce application-এর Auth, Product, Order এবং Payment module একই NestJS application-এ থাকতে পারে।

>#### Monolithic Architecture Diagram
<img width="646" height="632" alt="image" src="https://github.com/user-attachments/assets/0019b70e-3531-4d16-8cb9-9d986578a5ce" />


### সুবিধা
- তৈরি করা ও বোঝা তুলনামূলক সহজ।
- Development, testing এবং debugging সাধারণত সহজ।
- Deployment ও infrastructure-এর খরচ তুলনামূলক কম।
- ছোট ও মাঝারি application-এর জন্য ভালো পছন্দ হতে পারে।

### অসুবিধা
- কোনো module-এ পরিবর্তনের জন্য পুরো application redeploy করতে হতে পারে।
- একটি অংশের resource usage বেড়ে গেলে পুরো application-এর ওপর প্রভাব পড়তে পারে।
- বড় codebase-এ module-গুলোর dependency জটিল হতে পারে।

## ২. Microservices Architecture কী?

Microservices-এ application-কে একাধিক ছোট, স্বতন্ত্র service-এ ভাগ করা হয়। প্রতিটি service নির্দিষ্ট business responsibility পালন করে এবং আলাদাভাবে run, deploy ও scale করা যায়।

>#### Microservice Architecture Diagram
<img width="1109" height="680" alt="image" src="https://github.com/user-attachments/assets/6eab69b7-9a1c-403d-ac9c-8870658642b5" />


Service-গুলোর মধ্যে HTTP/REST, TCP, gRPC অথবা message broker-এর মাধ্যমে communication হতে পারে। প্রতিটি service-এর নিজস্ব database থাকতে পারে; তবে database design business requirement অনুযায়ী ঠিক করতে হয়।

### সুবিধা
- Service আলাদাভাবে deploy করা যায়।
- প্রয়োজন অনুযায়ী নির্দিষ্ট service scale করা যায়।
- বড় team আলাদা service নিয়ে স্বাধীনভাবে কাজ করতে পারে।
- বিভিন্ন service-এ প্রয়োজন অনুযায়ী ভিন্ন technology ব্যবহার করা সম্ভব।

### অসুবিধা
- Setup, deployment ও monitoring জটিল।
- Network failure এবং timeout সামলাতে হয়।
- Distributed logging, tracing ও debugging কঠিন হতে পারে।
- Data consistency, security এবং service-to-service communication-এর জন্য বেশি পরিকল্পনা প্রয়োজন।

## ৩. Monolith vs Microservices

| বিষয় | Monolith | Microservices |
|---|---|---|
| Architecture | একটি application | একাধিক স্বাধীন service |
| Deployment | সাধারণত একসঙ্গে | আলাদাভাবে |
| Communication | Function/method call | HTTP, TCP, gRPC, messaging ইত্যাদি |
| Scaling | সাধারণত পুরো application | প্রয়োজনীয় service |
| Database | Shared database প্রচলিত | Service অনুযায়ী আলাদা database প্রচলিত |
| Debugging | তুলনামূলক সহজ | Distributed tracing প্রয়োজন হতে পারে |
| Complexity | তুলনামূলক কম | তুলনামূলক বেশি |
| উপযোগী | ছোট/মাঝারি বা সুসংগঠিত application | জটিল, বড় এবং আলাদা করে scale করা প্রয়োজন এমন system |

> **নোট:** এগুলো সাধারণ প্রবণতা, কঠোর নিয়ম নয়। Monolith-ও ভালোভাবে scale করা যায়, আর Microservices হলেই যে application দ্রুত বা নির্ভরযোগ্য হবে, তা নয়।

## ৪. NestJS Project Structure-এর উদাহরণ

### Monolith — একটি NestJS project

```text
src/
├── auth/
├── users/
├── products/
├── orders/
├── payments/
├── app.module.ts
└── main.ts
```

একটি NestJS application চালু করেই সব module ব্যবহার করা যায়। Module-গুলো পরিষ্কারভাবে আলাদা রাখলে একে **Modular Monolith** বলা হয়।

### Microservices — একাধিক NestJS application

```text
apps/
├── api-gateway/
├── auth-service/
├── product-service/
├── order-service/
└── payment-service/
```

প্রতিটি service আলাদাভাবে চালানো ও deploy করা সম্ভব। তবে service-গুলো কীভাবে communicate করবে, authentication কীভাবে হবে এবং data ownership কার থাকবে—এসব স্পষ্টভাবে নির্ধারণ করতে হয়।

## ৫. API Gateway ও Load Balancing

![API Gateway and Load Balancing Diagram](images/api-gateway-load-balancing.svg)

### API Gateway কী?

API Gateway হলো client এবং backend service-গুলোর মাঝখানের একটি entry point। এটি request সঠিক service-এ পাঠাতে পারে। Configuration অনুযায়ী authentication, rate limiting, request routing এবং অন্যান্য cross-cutting কাজও করতে পারে।

### Load Balancer কী?

Load Balancer একাধিক service instance-এর মধ্যে incoming request ভাগ করে দেয়। যেমন, Order Service-এর তিনটি instance থাকলে request-গুলো উপযুক্ত instance-এ পাঠানো যায়।

**Kong** একটি API Gateway, যা উপযুক্ত configuration ও deployment-এ routing, load balancing এবং plugin-ভিত্তিক বিভিন্ন কাজ করতে পারে।

## ৬. কোনটি দিয়ে শুরু করা উচিত?

NestJS developer হিসেবে এই ক্রমে শেখা কার্যকর:

1. **Modular Monolith:** Modules, Dependency Injection, database transactions, validation, authentication এবং testing।
2. **Docker ও Deployment:** Application containerize করা, environment variables, logs ও health checks।
3. **Microservices Fundamentals:** TCP/HTTP communication, message broker, timeout, retry এবং idempotency।
4. **Distributed Systems:** Observability, distributed tracing, data consistency, service security ও failure handling।
5. **API Gateway ও Load Balancing:** Kong-এর মতো gateway, routing এবং একাধিক service instance পরিচালনা।

ছোট বা মাঝারি project-এ Microservices-এর জটিলতা না বাড়িয়ে Modular Monolith ব্যবহার করাই অনেক সময় ভালো সিদ্ধান্ত। প্রকৃত প্রয়োজন—যেমন স্বাধীন deployment, আলাদা scaling বা team ownership—থাকলে Microservices বিবেচনা করো।

## দ্রুত মনে রাখার কৌশল

- **Monolith:** একটি application, অনেক module।
- **Microservices:** অনেক স্বাধীন service, মিলে একটি system।
- **API Gateway:** Client-এর request গ্রহণ করে উপযুক্ত service-এ পাঠায়।
- **Load Balancer:** একাধিক instance-এর মধ্যে traffic ভাগ করে।

### আরও পড়ার জন্য
- NestJS Documentation: https://docs.nestjs.com/
- NestJS Microservices: https://docs.nestjs.com/microservices/basics
- Kong Documentation: https://docs.konghq.com/
