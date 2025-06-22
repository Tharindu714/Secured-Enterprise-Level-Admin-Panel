# 🔐 Secured Enterprise-Level Admin Panel

> A Java EE–based web application featuring role-based access control, custom security realms, and a polished admin dashboard.

---

## 📑 Table of Contents

1. [✨ Project Overview](#-project-overview)
2. [📂 Source Structure](#-source-structure)
3. [🛡️ Security Configuration](#-security-configuration)
4. [🚀 Deployment & Realm Setup](#-deployment--realm-setup)
5. [👤 Default Credentials](#-default-credentials)
6. [🌐 URL Mapping & Access Flow](#-url-mapping--access-flow)
7. [📷 Screenshots](#-screenshots)
8. [🛠️ Customization](#-customization)
9. [🤝 Contribution](#-contribution)
10. [📜 License](#-license)

---

## ✨ Project Overview

This application demonstrates an enterprise-grade admin panel with:

* **Role-Based Access Control**: Admin vs. User pages
* **Custom Security Realms** configured in `web.xml` and application server
* **JSF/Servlet**-based UI for login, dashboard, profile, and error handling
* **Centralized Filters** for authentication and authorization

---

## 📂 Source Structure

```
security
├── servlet/        # Servlets & JSF Managed Beans
│   ├── Admin.java
│   ├── User.java
│   ├── AutoLogin.java
│   └── Logout.java
└── ejb/             
    ├── AdminSessionBean.java
    └── UserSessionBean.java
src/main/webapp/
├── WEB-INF/
│   ├── web.xml         # Security constraints & login-config
│   ├── jboss-web.xml   # Security-domain mapping (for WildFly)
│   └── admin/
│       ├── index.jsp
│    ├── index.jsp
│    ├── login.jsp
│    └── error.jsp
```
---

## 🛡️ Security Configuration

1. **web.xml** sets up security constraints and login forms:

   ```xml
   <security-constraint>
     <web-resource-collection>
       <web-resource-name>AdminArea</web-resource-name>
       <url-pattern>/admin/*</url-pattern>
     </web-resource-collection>
     <auth-constraint>
       <role-name>ADMIN</role-name>
     </auth-constraint>
   </security-constraint>

   <login-config>
     <auth-method>FORM</auth-method>
     <realm-name>AdminRealm</realm-name>
     <form-login-config>
       <form-login-page>/login.jsp</form-login-page>
       <form-error-page>/error.jsp</form-error-page>
     </form-login-config>
   </login-config>

   <security-role>
     <role-name>ADMIN</role-name>
   </security-role>
   <security-role>
     <role-name>USER</role-name>
   </security-role>
   ```
2. **jboss-web.xml** (WildFly) binds the realm:

   ```xml
   <jboss-web>
     <security-domain>java:/jaas/AdminRealm</security-domain>
   </jboss-web>
   ```
3. **AuthFilter.java** checks session user and role before protected URLs.

---

## 🚀 Deployment & Realm Setup

1. **Deploy WAR**: Copy `target/adminpanel.war` to your server’s deployment folder.
2. **Configure Realm** in WildFly (or GlassFish):

   * Define a security domain `AdminRealm` linked to a properties file or JDBC realm.
   * For WildFly, add to `standalone.xml`:

     ```xml
     <security-domain name="AdminRealm">
       <authentication>
         <login-module code="UsersRoles" flag="required">
           <module-option name="usersProperties" value="users.properties"/>
           <module-option name="rolesProperties" value="roles.properties"/>
         </login-module>
       </authentication>
     </security-domain>
     ```
3. **Reload** the server.

---

## 👤 Default Credentials

| Role  | Username | Password   |
| ----- | -------- | ---------- |
| Admin | `admin`  | `8754` |
| User  | `user`   | `1234`  |

*Update your `users.properties` and `roles.properties` files accordingly.*

---

## 🌐 URL Mapping & Access Flow

1. **Home Page**: `http://localhost:8080/SECURED-ADMIN/`
2. **Login Page**: When you try to access Admin Panel it redirect you to Login.jsp `http://localhost:8080/SECURED-ADMIN/admin/`
3. **Submit** credentials → `Admin` authenticates against realm
4. **On Success**: redirects to `admin/index.jsp` (role ADMIN)
5. **Accessing `/admin/*`** without ADMIN role → redirected to `error.jsp`
6. **Logout** via `Logout` invalidates session and returns to `login.jsp`
7. Redirect to `http://localhost:8080/SECURED-ADMIN/admin/profile` if you are not logged it brings u the login page
8. Try **Auto Login** : `http://localhost:8080/SECURED-ADMIN/autologin` - This will let you login to the admin panel without login
   
---

## 📷 Screenshots

### 1️⃣ Error Page

> *error.jsp screenshot here*
![error page](https://github.com/user-attachments/assets/cb244b77-a999-4fa4-adb6-68f2d2bf47fb)

### 2️⃣ Login Page

> *login.jsp screenshot here*
![Login](https://github.com/user-attachments/assets/cf99aada-4ef9-4257-a8b8-3ebf198a8a63)

### 3️⃣ Home Page

> *home.jsp screenshot here*
![Home](https://github.com/user-attachments/assets/d92234c1-4fba-406c-b064-379789694427)

### 4️⃣ Admin Panel

> *adminPanel.jsp screenshot here*
![admin panel](https://github.com/user-attachments/assets/3117ef47-99e9-49d7-8dda-406b0b477b0c)

### 5️⃣ Admin Profile

> *profile.jsp screenshot here*
![Admin Profile](https://github.com/user-attachments/assets/2ee45736-59fe-49f8-a129-31f7b585b4eb)

---

## 🛠️ Customization

* **Realms**: Switch to JDBC realm by adjusting login-module options to use a database.
* **Roles**: Add new roles in `web.xml` and `roles.properties`.
* **UI**: Enhance JSPs with Bootstrap or a frontend framework.

---

## 🤝 Contribution

1. Fork → branch → PR against `main`.
2. Follow Java EE conventions; include unit tests if adding business logic.

---

## 📜 License

MIT © 2025 Tharindu Chanaka

---

> Developed with 🔒 and ☕ by Tharindu Chanaka

