# Codebase structure

The **AMRIT Platform** is a comprehensive digital healthcare solution, composed of modular, domain-specific repositories. This document provides an overview of the project’s structure, its components, and how they interconnect to deliver a scalable and efficient platform.

***

### Overview

The AMRIT Platform codebase is organized into **three primary categories** of repositories:

1. **UI Repositories**: These repositories host the Angular-based frontend modules for various healthcare services.
2. **API Repositories**: These repositories provide Spring Boot-based microservices exposing REST APIs to power the UI modules and mobile applications.
3. **Mobile Application Repositories**: Android apps built with Kotlin, designed for front-line workers (FLWs) and health professionals.

Each repository focuses on a specific domain (e.g., Telemedicine, Inventory, Health and Wellness Centers) and follows **microservices architecture** principles, ensuring modularity, scalability, and ease of maintenance.

***

### Codebase Structure

Below is the general structure for each type of repository:

#### 1. **UI Repositories**

UI repositories are Angular projects. Each repository includes reusable components, services, and routing configurations tailored to its domain.\
**Key folders and files**:

* **`src/app/`**: Contains all Angular components, services, and modules.
  * **`components/`**: Modular UI components.
  * **`services/`**: Business logic and API integration.
  * **`pages/`**: Views corresponding to the application’s routes.
* **`src/assets/`**: Static assets like images, icons, and translations.
* **`angular.json`**: Configuration file for Angular CLI.
* **`package.json`**: Contains dependencies and build scripts.

**Example**:

```
Inventory-UI/
├── src/
│   ├── app/
│   │   ├── components/
│   │   ├── services/
│   │   ├── pages/
│   │   └── app.module.ts
│   ├── assets/
│   └── environments/
├── angular.json
├── package.json
└── README.md

```

#### 2. **API Repositories**

API repositories are Spring Boot microservices. They provide RESTful endpoints for their respective UI or mobile applications.\
**Key folders and files**:

* **`src/main/java/`**: Application source code.
  * **`controllers/`**: REST controllers to handle incoming API requests.
  * **`services/`**: Business logic implementation.
  * **`repositories/`**: Data access layer using JPA or custom queries.
  * **`models/`**: Domain models and entities.
* **`src/main/resources/`**: Configuration files, including `application.yml`.
* **`pom.xml`**: Maven configuration for dependencies and plugins.

**Example**:

<pre><code><strong>Inventory-API/
</strong>├── src/
│   ├── main/
│   │   ├── java/
│   │   │   ├── controllers/
│   │   │   ├── services/
│   │   │   ├── repositories/
│   │   │   └── models/
│   │   └── resources/
│   │       └── application.yml
├── pom.xml
└── README.md
</code></pre>

#### 3. **Mobile Application Repositories**

Mobile repositories are Kotlin-based Android projects. They are designed to integrate seamlessly with the AMRIT APIs.\
**Key folders and files**:

* **`app/src/main/java/`**: Application source code.
  * **`activities/`**: Screens and UI logic.
  * **`fragments/`**: Reusable UI components.
  * **`viewmodels/`**: Data-binding and state management.
  * **`repositories/`**: API calls and data management.
* **`res/`**: UI resources such as layouts, drawables, and strings.
* **`build.gradle`**: Build configurations and dependencies.

**Example**:

```
FLW-Mobile-App/
├── app/
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/
│   │   │   │   ├── activities/
│   │   │   │   ├── fragments/
│   │   │   │   ├── viewmodels/
│   │   │   │   └── repositories/
│   │   │   └── res/
│   ├── build.gradle
├── gradle.properties
└── README.md
```

### Interdependencies

AMRIT follows a **loosely coupled architecture**, where each module functions independently but integrates via APIs to create a unified experience.

* **UI to API Communication**: Angular applications interact with backend services via REST APIs, using `HttpClient` for communication.
* **Mobile App to API Communication**: Android apps consume the same APIs used by the UI modules, enabling uniform behavior across platforms.
* **Shared Components**: Common-UI and Common-API are reusable components shared across multiple modules, ensuring consistency.

***

### Best Practices

1. **Consistent Naming**: Follow the naming conventions used across repositories to maintain uniformity.
2. **Modular Design**: Ensure each repository serves a single responsibility for better maintainability.
3. **Environment-Specific Configurations**: Use environment variables (`application.yml` or `.env`) to manage configurations for staging, production, and local development.

### Next Steps

* Explore the [**Developer Guide**](development-environment-setup/) for setup instructions.
* Dive into the [**API Documentation**](../architecture/api-guide.md) for endpoint details.

Happy Coding! 🎉
