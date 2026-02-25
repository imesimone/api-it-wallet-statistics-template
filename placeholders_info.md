# IT-Wallet Statistics API Template Guide

This repository contains an OpenAPI specification (`template.yaml`) that serves as a **standard template** for implementing monitoring and statistics services for IT-Wallet authentications.

## Template Purpose
This template defines a standard interface for service providers (or technical aggregators such as Regional Hubs) to expose aggregated data regarding IT-Wallet usage. It is designed for scenarios where a single access point (e.g., a Region) manages data for multiple federated entities.

---

## Placeholder Table
The placeholder values must be replaced with the actual data of the implementing entity.

| Placeholder Value | YAML File Path | Meaning and Usage |
| :--- | :--- | :--- |
| `region.example.it` | `info.termsOfService`, `info.contact.url` | Official domain of the aggregating entity. |
| `legal-notes` | `info.termsOfService` | URL path for legal documentation and terms of service. |
| `CREDENTIAL_SERVICE_NAME` | `info.contact.name` | Specific name of the service (e.g., `Driving License`). |
| `ISSUING_ENTITY` | `info.contact.name` | Name of the issuing organization (e.g., `Ministry of Infrastructure`). |
| `technical.support@region.example.it` | `info.contact.email` | Technical support contact email. |
| `00000000-0000-0000-0000-000000000000` | `info.x-api-id` | API ID on PDND (UUID format). |
| `api.production.region.example.it` | `servers[0].url` | Official production environment endpoint. |
| `api.test.example.it` | `servers[1].url` | Test/sandbox environment endpoint. |
| `r_piemon` | `components.schemas.AdministrationEntry.properties.ipa.example` | Example IPA code for an entity. |
| `Piedmont Region` | `components.schemas.AdministrationEntry.properties.name.example` | Example full name of the administration. |

---

## Key Features for Regional Aggregators

The template includes advanced mechanisms for robust authentication monitoring across federated entities:

### 1. Flexible Query Interface (POST Method)
The `/auth-stats` endpoint utilizes the `POST` method to handle structured query payloads (`StatisticsQuery`). This approach ensures better extensibility and avoids URL length limitations for complex filters.

### 2. Date Range Support (`period.from` / `period.to`)
Instead of fixed time units (day/month/year), the API uses a start (`from`) and end (`to`) date in ISO 8601 format.
- **Versatility**: Supports querying a single day (e.g., `from: 2025-03-01, to: 2025-03-01`), a specific week, or an entire month.
- **Reporting**: The response `period` confirms the exact timeframe analyzed by the system.

### 3. Hierarchical Reporting (Summary vs. Breakdown)
The report (`AuthenticationReport`) provides:
- **Summary**: Global counts (successes, technical failures, business failures) for the entire requested period.
- **Breakdown Array**: A granular list that adapts to the query. If a multi-day range is requested, it provides daily metrics. If no specific entity is filtered, it provides metrics for each managed administration.

### 4. Categorized Failure Analysis
Failures are distinguished into:
- **Technical**: Infrastructure, server-side, or connectivity issues (Hub responsibility).
- **Business**: Token expiration, unauthorized access, or user data issues (Client/Service responsibility).

---

## HTTP Header Definitions

To ensure high-quality interoperability and compliance with international standards, the template utilizes the following HTTP headers:

### **Cache-Control**
To avoid data leaks or the usage of stale data, the API implements a strict caching policy documented using the following directives:
- **`no-store`** and **`no-cache`**: Prevents the storage of statistical data in intermediate caches or local storage, ensuring that the information retrieved is always current.
- **`private`**: Restricts the response to be intended for a single user, preventing shared caches from storing the response.
- **`max-age=0`**: Forces caches to revalidate the response with the origin server before using a cached copy.
- **`no-transform`**: Prevents transforming proxies (e.g., those that compress images or modify headers) from modifying the content, ensuring data integrity across network nodes.

### **Rate-Limit Headers** (`RateLimit-Limit`, `RateLimit-Remaining`, `RateLimit-Reset`)
- **Purpose**: Communicates the maximum number of requests allowed within a specific time frame and the remaining quota.
- **Utility**: Allows clients to self-regulate their request frequency, preventing service interruptions and ensuring fair resource allocation.

### **Retry-After**
- **Purpose**: Indicates the duration (in seconds) the client should wait before attempting a new request following a `429` or `503` error.
- **Utility**: Enhances system stability by managing traffic peaks and avoiding unnecessary retry attempts during maintenance or congestion.

### **WWW-Authenticate**
- **Purpose**: A mandatory requirement under **RFC 7235** for `401 Unauthorized` responses. In this context, it aligns with **RFC 6750** for Bearer Tokens.
- **Utility**: Provides the client with critical diagnostic information regarding authentication failures (e.g., expired tokens), enabling efficient automated error resolution without compromising security protocols.
