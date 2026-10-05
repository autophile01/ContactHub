# ContactHub - A Smart Contact Manager

A secure, web-based contact management application built with **Java, Spring Boot, Spring Security, Thymeleaf, JPA/Hibernate and MySQL**.

Smart Contact Manager allows registered users to securely maintain their personal contacts, search and manage contact information, update their profile, and recover/change their password through OTP-based email verification.

---

## 🚀 Features

### 👤 User Authentication & Authorization
- User registration with server-side validation
- Secure login using Spring Security
- Role-based authorization
- BCrypt password hashing
- Protected user dashboard and contact management routes
- Session-based user context

### 📇 Contact Management
- Add new contacts
- View contact details
- Update existing contacts
- Delete contacts
- Upload contact profile images
- Default profile image support
- Contacts are associated with the authenticated user

### 🔎 Contact Search
- Search contacts dynamically by name
- Search results are restricted to the currently authenticated user

### 📄 Pagination
- Paginated contact listing
- Displays 5 contacts per page
- Supports navigation between contact pages

### 🔐 Password Management
- Change password from the user settings page
- BCrypt password verification
- Forgot-password workflow
- OTP generation and email verification
- Password reset after successful OTP verification

### 👤 User Profile
- View user profile
- Manage account settings
- Update password

### ✉️ Email Integration
- SMTP-based email service
- OTP delivery for password recovery
- HTML-formatted OTP emails

### 🛡️ Data Validation
- Bean Validation for user registration
- Email format validation
- Username/name length validation
- Password validation
- Server-side validation error handling

---

## 🏗️ Application Architecture

```text
                    ┌─────────────────────────┐
                    │       Web Browser       │
                    │  Thymeleaf + HTML/CSS   │
                    │       + JavaScript      │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │    Spring Boot Web      │
                    │      Controllers        │
                    └────────────┬────────────┘
                                 │
              ┌──────────────────┼──────────────────┐
              ▼                  ▼                  ▼
       ┌─────────────┐   ┌──────────────┐   ┌──────────────┐
       │   Services  │   │ Spring       │   │   Validation │
       │             │   │ Security     │   │              │
       └──────┬──────┘   └──────┬───────┘   └──────────────┘
              │                 │
              └──────────┬──────┘
                         ▼
                ┌──────────────────┐
                │ Spring Data JPA  │
                │   Hibernate ORM   │
                └────────┬─────────┘
                         │
                         ▼
                ┌──────────────────┐
                │      MySQL       │
                │  SmartContact DB │
                └──────────────────┘
```

---

## 🧩 Project Structure

```text
SmartContactManager/
│
├── src/
│   ├── main/
│   │   ├── java/com/smart/
│   │   │   ├── config/
│   │   │   │   ├── CustomerUserDetails.java
│   │   │   │   ├── MyConfig.java
│   │   │   │   └── UserDetailsServiceImpl.java
│   │   │   │
│   │   │   ├── controller/
│   │   │   │   ├── ForgotPasswordController.java
│   │   │   │   ├── HomeController.java
│   │   │   │   ├── SearchController.java
│   │   │   │   └── UserController.java
│   │   │   │
│   │   │   ├── dao/
│   │   │   │   ├── ContactRepository.java
│   │   │   │   └── UserRepository.java
│   │   │   │
│   │   │   ├── entities/
│   │   │   │   ├── Contact.java
│   │   │   │   └── User.java
│   │   │   │
│   │   │   ├── helper/
│   │   │   │   └── Message.java
│   │   │   │
│   │   │   └── services/
│   │   │       ├── EmailService.java
│   │   │       └── sessionHelper.java
│   │   │
│   │   └── resources/
│   │       ├── static/
│   │       │   ├── css/
│   │       │   ├── js/
│   │       │   └── img/
│   │       │
│   │       └── templates/
│   │           ├── login.html
│   │           ├── signup.html
│   │           ├── forgot_email_form.html
│   │           ├── verify_otp.html
│   │           ├── password_change_form.html
│   │           └── normal/
│   │               ├── user_dashboard.html
│   │               ├── add_contact_form.html
│   │               ├── show_contacts.html
│   │               ├── contact_detail.html
│   │               ├── update_form.html
│   │               ├── profile.html
│   │               └── settings.html
│   │
│   └── test/
│       └── java/
│
├── pom.xml
├── mvnw
├── mvnw.cmd
└── .gitignore
```

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Language | Java |
| Backend | Spring Boot 3.1.2 |
| Web | Spring MVC |
| Security | Spring Security |
| ORM | Hibernate / JPA |
| Database | MySQL |
| Template Engine | Thymeleaf |
| Validation | Jakarta Bean Validation |
| Email | JavaMail / SMTP |
| Frontend | HTML, CSS, JavaScript, Thymeleaf |
| Build Tool | Maven |
| Password Security | BCrypt |

---

## 🗄️ Database Model

The application uses two primary entities.

### User

```text
User
 ├── id
 ├── name
 ├── email
 ├── password
 ├── role
 ├── enabled
 ├── imageUrl
 ├── about
 └── contacts
```

### Contact

```text
Contact
 ├── cId
 ├── name
 ├── secondName
 ├── work
 ├── email
 ├── phone
 ├── image
 ├── description
 └── user
```

### Relationship

```text
User 1 ──────────────── * Contact
```

A single user can maintain multiple contacts, while every contact belongs to a specific user.

---

## 🔐 Security

Spring Security protects application routes based on roles.

```text
/user/**  → ROLE_USER
/admin/** → ROLE_ADMIN
/**       → Public
```

Passwords are encoded using BCrypt before being stored in the database.

The application also uses a custom `UserDetailsService` and `DaoAuthenticationProvider` for authentication.

---

## 🔎 Search Flow

A logged-in user can search their contacts using:

```text
GET /search/{query}
```

The application identifies the authenticated user and returns only contacts belonging to that user.

This prevents one user from retrieving another user's contacts through the search functionality.

---

## 📄 Pagination

Contact listings use Spring Data's `Pageable` abstraction.

Current configuration:

```text
5 contacts per page
```

Example route:

```text
/user/show-contacts/{page}
```

This allows the application to handle larger contact lists without loading every contact onto a single page.

---

## 🔑 Forgot Password Flow

```text
User enters email
       │
       ▼
Generate OTP
       │
       ▼
Send OTP through SMTP
       │
       ▼
User enters OTP
       │
       ▼
Validate OTP
       │
       ▼
Verify user exists
       │
       ▼
Set new BCrypt password
       │
       ▼
Redirect to Login
```

---

## 📧 Email Configuration

The application uses Gmail SMTP for sending OTP emails.

**Do not commit real email credentials or application passwords to GitHub.**

Configure your SMTP credentials through environment variables or another secret-management mechanism before running the application.

For example:

```text
MAIL_USERNAME=your-email@example.com
MAIL_PASSWORD=your-app-password
```

---

## ⚙️ Prerequisites

Before running the project, install:

- Java 20 or a compatible Java version
- Maven (optional because Maven Wrapper is included)
- MySQL 8+
- Git
- A Gmail account/app password if using the OTP email feature

Verify Java:

```bash
java -version
```

---

## 🗄️ MySQL Setup

Create the database:

```sql
CREATE DATABASE smartContact;
```

Then configure the application's database connection.

Example:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/smartContact
spring.datasource.username=YOUR_USERNAME
spring.datasource.password=YOUR_PASSWORD
```

> Never commit your actual database password to a public repository.

Hibernate is configured to automatically update the schema:

```properties
spring.jpa.hibernate.ddl-auto=update
```

---

## ▶️ Running the Application

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/smart-contact-manager.git
cd smart-contact-manager
```

### 2. Configure MySQL

Create the `smartContact` database and update the database credentials in your local configuration.

### 3. Configure email

Set your SMTP credentials locally if you want to use the forgot-password OTP functionality.

### 4. Run with Maven Wrapper

#### Windows

```bash
mvnw.cmd spring-boot:run
```

#### macOS/Linux

```bash
./mvnw spring-boot:run
```

Or with Maven:

```bash
mvn spring-boot:run
```

### 5. Open the application

```text
http://localhost:8080
```

---

## 🧪 Testing

The project includes Spring Boot test configuration under:

```text
src/test/
```

Run tests with:

```bash
mvn test
```

or:

```bash
mvnw.cmd test
```

on Windows.

---

## 🔗 Important Routes

| Route | Purpose |
|---|---|
| `/` | Home page |
| `/about` | About page |
| `/signup` | User registration |
| `/signin` | Login page |
| `/forgot` | Forgot password |
| `/user/index` | User dashboard |
| `/user/add-contact` | Add contact |
| `/user/show-contacts/{page}` | View contacts |
| `/user/contact/{id}` | Contact details |
| `/user/delete/{id}` | Delete contact |
| `/user/profile` | User profile |
| `/user/settings` | Account settings |
| `/search/{query}` | Search contacts |

---

## 💡 Key Engineering Concepts Demonstrated

This project demonstrates practical backend concepts including:

- MVC architecture
- REST endpoint for contact search
- Spring Security authentication
- Role-based authorization
- Custom `UserDetailsService`
- BCrypt password hashing
- Spring Data JPA
- Hibernate ORM
- Entity relationships
- Repository pattern
- Pagination using `Pageable`
- Bean Validation
- Multipart file upload
- Session management
- SMTP email integration
- OTP-based password recovery
- Exception handling
- Server-side form validation

---

## 📌 Future Improvements

Potential improvements for a production-ready version:

- Replace session-stored OTP with a short-lived database/cache-backed OTP
- Add OTP expiration and rate limiting
- Move all secrets to environment variables or a secret manager
- Add CSRF protection instead of disabling it
- Add stronger password policies
- Add comprehensive unit and integration tests
- Add Docker support
- Add centralized exception handling with `@ControllerAdvice`
- Add API documentation using OpenAPI/Swagger
- Add contact sorting and advanced filtering
- Add audit logging
- Add profile/contact image storage using object storage rather than the application classpath

---

## 👨‍💻 Project Highlights

**Smart Contact Manager** demonstrates the development of a secure server-side web application using the Spring ecosystem, with particular focus on authentication, authorization, relational data modeling, contact CRUD operations, pagination, validation, file uploads, and OTP-based password recovery.

---

## 📄 License

This project is intended for educational and portfolio purposes.
