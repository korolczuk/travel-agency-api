# Travel Agency REST API

A robust, domain-driven RESTful Web API designed to manage operations, client registrations, and business rules for a travel agency.

**🎓 Academic Context:** This repository contains an implementation focused on Enterprise Application Development concepts at the Polish-Japanese Academy of Information Technology. The primary educational and technical value of this project lies in database interaction. It intentionally bypasses modern ORMs (like Entity Framework) in favor of raw **ADO.NET**. This approach demonstrates a deep understanding of underlying SQL execution, manual connection management, and query optimization, avoiding the "magic" of automated object-relational mappers.

---

## 🛠️ Tech Stack

| Category | Technology |
| :--- | :--- |
| **Language** | C# 12 |
| **Framework** | ASP.NET Core 8 (`net8.0` Web API) |
| **Database Access** | ADO.NET (`Microsoft.Data.SqlClient`) |
| **API Documentation** | Swagger / OpenAPI |
| **Architecture** | N-Tier, Dependency Injection, DTO Pattern |

---

## 🧠 Architectural Decisions & Patterns

To maintain a clean codebase and ensure scalability, the project strictly adheres to modern software engineering principles:

* **N-Tier Architecture:** Separation of concerns is strictly enforced. Controllers (`ClientsController`, `TripsController`) remain incredibly "thin," responsible solely for HTTP request/response handling. All heavy lifting and database interactions are delegated to dedicated Service classes.
* **Data Transfer Objects (DTO):** The API never exposes raw database entities. By implementing a strict DTO pattern (e.g., `ClientCreateDTO`, `TripDTO`), the system prevents over-posting attacks, decouples the database schema from the API contract, and formats JSON responses precisely to client needs.
* **Inversion of Control (IoC):** The system relies heavily on Dependency Injection. Services are abstracted behind interfaces (e.g., `iTripsService`) and injected into controllers via the native ASP.NET Core DI container, ensuring the code is modular and fully testable.
* **Raw SQL via ADO.NET:** Database commands are written as raw SQL queries within the service layer. This ensures maximum execution speed and granular control over the data retrieval process.

---

## 🏗️ Domain Model & Core Features

The API manages two primary resources—**Clients** and **Trips**—and handles complex business logic regarding their relationships:

* **Trip Management:**
  * Retrieve a comprehensive list of all available trips.
  * Fetch specific geographical data (e.g., countries associated with a specific trip).
* **Client Management:**
  * Register new clients into the system.
  * Retrieve detailed client profiles, including a history of all trips they are currently registered for.
* **Complex Business Logic (Trip Registration):**
  The endpoint for assigning a client to a trip (`RegisterClientForTrip`) is not a simple database `INSERT`. It implements a strict validation pipeline:
  1. **Existence Check:** Validates if the client and the requested trip exist in the database.
  2. **Capacity Validation:** Ensures the trip has not exceeded its maximum participant limit.
  3. **Duplicate Prevention:** Checks if the client is already registered for the specified trip to prevent redundant entries.
  * *The API also supports safe removal/unregistration of clients from their assigned trips (`RemoveClientFromTrip`).*

---

## ⚙️ How to Run

1. Clone the repository to your local machine:
    ```bash
    git clone [https://github.com/korolczuk/travel-agency-api.git](https://github.com/korolczuk/travel-agency-api.git)
    ```

2. Navigate to the project directory:
    ```bash
    cd travel-agency-api
    ```

3. Restore dependencies and run the application using the .NET CLI:
    ```bash
    dotnet restore
    dotnet run
    ```

4. **Explore the API:** Once the application is running, open your browser and navigate to the automatically generated Swagger UI (typically `https://localhost:<port>/swagger`). This interface allows you to easily test all endpoints, view required JSON payloads, and interact with the database.
