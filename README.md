# Airbnb Clone Project (StayHub)

## Project Overview

This is a robust, backend-only Airbnb clone designed to simulate the core functionalities of a vacation rental platform. Built with Python, Django, and Django REST Framework (DRF), this API enables users to seamlessly list, discover, book, and review properties.

## Project Goals

The primary objectives of the StayHub project are to:

- Develop a comprehensive RESTful API for a property rental platform
- Implement real-world backend concepts, including secure authentication, role-based access control, CRUD operations, and efficient database design
- Leverage Django and Django REST Framework for streamlined API management, data serialization, and validation
- Utilize JSON Web Tokens (JWT) for secure and stateless authentication
- Prepare the backend for deployment to a production-like hosting environment

## Technology Stack

The StayHub API is built using the following technologies:

### Backend Framework
- **Python**: The primary programming language used for backend development, chosen for its simplicity, extensive libraries, and strong community support in web development
- **Django**: A high-level Python web framework that encourages rapid development and clean, pragmatic design. It handles URL routing, database interactions, and provides built-in security features
- **Django REST Framework (DRF)**: A powerful toolkit for building Web APIs in Django, providing serialization, authentication, permissions, and browsable API interface

### Database
- **PostgreSQL**: Production database system chosen for its reliability, ACID compliance, and advanced features like JSON support and full-text search capabilities
- **SQLite**: Development database used for local testing due to its simplicity and zero-configuration setup

### Authentication & Security
- **JWT (JSON Web Tokens)**: Implemented using `djangorestframework-simplejwt` for secure, stateless authentication that scales well across distributed systems
- **Django Authentication System**: Built-in user management and permission system for role-based access control

### Development & Testing Tools
- **Postman**: API testing and debugging tool used to validate endpoint functionality and test various request scenarios
- **Git & GitHub**: Version control system for tracking code changes and enabling collaborative development

### Deployment Platforms
- **Render/Railway**: Target cloud platforms for production deployment, chosen for their ease of use, automatic deployments, and PostgreSQL integration

## Team Roles

### Backend Developer
**Main Responsibility:** Develops the server-side logic, APIs, and core functionalities of the application using Django and DRF.

**Key Tasks:**
- Build and document RESTful APIs for listings, bookings, authentication, etc.
- Integrate JWT-based authentication and user roles
- Handle data serialization and validation
- Ensure API security, performance, and scalability
- Write unit tests for endpoints

### Database Administrator (DBA)
**Main Responsibility:** Designs, manages, and optimizes the project's database structure and performance.

**Key Tasks:**
- Design the database schema (e.g., users, listings, bookings, reviews)
- Set up and manage PostgreSQL or SQLite
- Define data relationships and constraints
- Monitor data integrity, backups, and indexing
- Optimize queries for performance

### DevOps Engineer
**Main Responsibility:** Handles the setup, deployment, and infrastructure management of the application.

**Key Tasks:**
- Configure CI/CD pipelines for auto-deployment (e.g., GitHub + Render/Railway)
- Set up production environment variables and secrets
- Monitor server health and logs
- Containerize the app with Docker (if used)
- Ensure rollback and recovery processes

## Database Design

### User
Represents both guests and hosts in the platform. Each user has a designated role that determines their access permissions and available functionalities.

**Important Fields:**
- `id`: Unique identifier (UUID or AutoField)
- `username`: User's display name
- `email`: Unique email address for authentication
- `password`: Hashed password for security
- `role`: Either "guest" or "host" to determine user permissions

**Relationships:**
- A User can own multiple Properties (if host)
- A User can make multiple Bookings (if guest)
- A User can leave multiple Reviews (as a guest)

### Property (Listing)
Represents a rental property listed by a host on the platform.

**Important Fields:**
- `id`: Unique property identifier
- `host`: ForeignKey to User (host)
- `title`: Name/title of the property
- `description`: Detailed description of the property
- `price_per_night`: Cost per night for booking

**Relationships:**
- A Property is owned by one User (host)
- A Property can have many Bookings
- A Property can have many Reviews

### Booking
Represents a reservation made by a guest for a specific property and time period.

**Important Fields:**
- `id`: Unique booking identifier
- `guest`: ForeignKey to User (guest)
- `property`: ForeignKey to Property
- `check_in`: Start date of stay
- `check_out`: End date of stay

**Relationships:**
- A Booking belongs to one Property
- A Booking is made by one User (guest)
- A Booking can have one Payment

### Review
Represents feedback and ratings left by guests after completing their stays.

**Important Fields:**
- `id`: Unique review identifier
- `author`: ForeignKey to User (guest)
- `property`: ForeignKey to Property
- `rating`: Integer rating (1-5 scale)
- `comment`: Text review content

**Relationships:**
- A Review is made by a User (guest)
- A Review is tied to a specific Property

### Payment
Tracks payment information and transaction status for bookings.

**Important Fields:**
- `id`: Unique payment identifier
- `booking`: OneToOneField to Booking
- `amount`: Total payment amount
- `status`: Payment status (completed, failed, pending)
- `payment_method`: Method used (credit_card, paypal, stripe)

**Relationships:**
- A Payment is linked to one Booking
- A Booking has one Payment (when paid)

## Feature Breakdown

### User Management
This feature handles all aspects of user interaction with the platform, from secure registration and login using JWT to managing user profiles and roles. It ensures that users can be authenticated as either guests or hosts, providing tailored access and functionalities based on their designated role.

### Property Listings
This allows hosts to effectively manage their rental properties on the platform. Hosts can create new listings with detailed information (title, description, price, location, amenities, photos), as well as update or remove existing ones, enabling them to control their offerings and maintain accurate property information.

### Booking System
The booking system facilitates the core transaction of the platform, enabling guests to reserve available properties for specific dates and guest counts. It ensures accurate total price calculation and empowers hosts to efficiently manage and track reservations for their listings.

### Reviews & Ratings
This feature fosters trust and transparency within the community by allowing guests to provide feedback on their completed stays. It helps future guests make informed decisions by providing average ratings for listings, contributing to the overall quality and reliability of the platform.

### Search & Filters
This functionality enhances user experience by allowing guests to easily discover properties that meet their specific criteria. Users can efficiently search by location and refine their results using various filters such as price range, dates, and amenities, making property discovery intuitive and tailored.

### Payments (Optional/Future Enhancement)
Integration with a payment processor like Stripe allows guests to securely pay for their bookings online. This feature automates financial transactions and provides a seamless booking experience while ensuring secure handling of sensitive payment information.

### Notifications (Optional/Future Enhancement)
The system can be extended to send email notifications for booking confirmations, cancellations, or reminders. This keeps both hosts and guests informed in real-time and improves overall user engagement and communication.

## API Security

Securing the backend APIs is paramount for protecting sensitive data, maintaining user trust, and ensuring the integrity of the platform. This project implements several key security measures:

### Authentication
This verifies the identity of users attempting to access the API. For StayHub, JWT (JSON Web Tokens) are used for secure and stateless authentication, ensuring that only legitimate and recognized users can interact with restricted endpoints. This is crucial for protecting user accounts and their associated personal and booking data.

### Authorization
Once authenticated, authorization determines what specific resources a user is permitted to access or actions they can perform based on their role (Guest or Host). This measure prevents unauthorized access to sensitive operations, such as a Guest attempting to manage a Host's property listings or a Host accessing another Host's bookings, thereby safeguarding both user data and property management integrity.

### Rate Limiting
This mechanism controls the number of API requests a user or IP address can make within a given timeframe. Implementing rate limiting helps prevent abuse, such as brute-force attacks on login endpoints, denial-of-service (DoS) attacks, and excessive data scraping, ensuring the stability and availability of the API for all legitimate users while protecting overall system performance.

## CI/CD Pipeline

### What is CI/CD?

CI/CD (Continuous Integration and Continuous Deployment) is a development practice that automates the process of testing, building, and deploying code.

**Continuous Integration (CI)** ensures that every code change is automatically tested and merged into the main branch without breaking the application. This practice helps catch bugs early, maintains code quality, and enables faster development cycles.

**Continuous Deployment (CD)** automatically deploys these changes to the production or staging environment after successful tests. This reduces manual deployment errors and ensures consistent, reliable releases.

### Benefits for This Project

Implementing a CI/CD pipeline in StayHub provides several key advantages:

- **Faster Development Cycles**: Automated testing and deployment eliminate manual bottlenecks, allowing developers to focus on feature development rather than deployment processes
- **Higher Code Quality**: Consistent integration and validation through automated testing ensures that code meets quality standards before reaching production
- **Reduced Human Error**: Automation eliminates manual deployment steps that are prone to mistakes, resulting in more reliable deployments
- **Early Bug Detection**: Automated testing catches issues and security vulnerabilities before they reach production, reducing the cost and impact of fixes
- **Deployment Confidence**: Knowing that code has been thoroughly tested through automated pipelines gives teams confidence in their production releases

### Tools & Technologies

The following tools are utilized to implement CI/CD for this project:

- **GitHub Actions**: Automates testing and deployment workflows directly from GitHub repositories, providing seamless integration with the existing version control system
- **Docker**: Ensures consistent environments across development, testing, and production by containerizing the application and its dependencies
- **Render/Railway**: Cloud platforms that enable automated backend deployment triggered by code pushes to the main branch
- **PostgreSQL**: Integrated in both development and CI environments for comprehensive database functionality testing
- **Coverage.py/Pytest**: Python testing frameworks used for automated unit testing and coverage reporting to ensure code quality standards
