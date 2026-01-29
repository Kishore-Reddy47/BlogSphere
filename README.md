# BlogSphere 📝

**A Secure Spring Boot REST API for Blogging Applications**

---

## Overview

BlogSphere is a feature-rich REST API built with Spring Boot that provides a complete backend solution for blogging applications. It includes JWT-based authentication, role-based access control, and comprehensive API documentation with Swagger UI.

---

## ✨ Key Features

✅ **JWT Authentication** - Secure token-based authentication  
✅ **Role-Based Access Control (RBAC)** - Admin and User roles with granular permissions  
✅ **Complete CRUD Operations** - Posts, comments, categories, and user management  
✅ **Image Upload Integration** - Cloudinary API integration for seamless image handling  
✅ **API Documentation** - Comprehensive Swagger UI for easy API exploration  
✅ **Error Handling** - Robust exception handling with custom error responses  
✅ **Data Validation** - Input validation using Bean Validation  
✅ **Database Persistence** - MySQL with Spring Data JPA ORM  

---

## 🛠 Tech Stack

**Backend Framework:**
- Spring Boot 2.7+ / 3.0+
- Spring Security with JWT
- Spring Data JPA

**Database:**
- MySQL 8.0+
- Hibernate ORM

**Tools & Libraries:**
- Maven for build management
- Postman for API testing
- Swagger (Springfox / Springdoc) for API documentation
- Cloudinary for image uploads
- JUnit & Mockito for unit testing

**Server:**
- Apache Tomcat (embedded)

---

## 📋 API Endpoints

### Authentication
```
POST   /api/auth/register     - Register a new user
POST   /api/auth/login        - User login with JWT token
POST   /api/auth/refresh      - Refresh JWT token
```

### Posts (CRUD)
```
GET    /api/posts             - Get all published posts
GET    /api/posts/{id}        - Get post by ID
POST   /api/posts             - Create new post (authenticated)
PUT    /api/posts/{id}        - Update post (author/admin only)
DELETE /api/posts/{id}        - Delete post (author/admin only)
```

### Comments
```
GET    /api/posts/{id}/comments       - Get comments on a post
POST   /api/posts/{id}/comments       - Add comment (authenticated)
DELETE /api/comments/{id}             - Delete comment (author/admin)
```

### Categories
```
GET    /api/categories        - Get all categories
POST   /api/categories        - Create category (admin only)
PUT    /api/categories/{id}   - Update category (admin only)
DELETE /api/categories/{id}   - Delete category (admin only)
```

### Image Upload
```
POST   /api/uploads/image     - Upload image to Cloudinary
```

---

## 🚀 Getting Started

### Prerequisites
- Java 11+ (JDK 11 or higher)
- Maven 3.6+
- MySQL 8.0+
- Cloudinary account (for image uploads)

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/Kishore-Reddy47/BlogSphere.git
   cd BlogSphere
   ```

2. **Create MySQL database**
   ```sql
   CREATE DATABASE blogsphere_db;
   ```

3. **Configure application properties**
   
   Create `src/main/resources/application.properties`:
   ```properties
   # MySQL Configuration
   spring.datasource.url=jdbc:mysql://localhost:3306/blogsphere_db
   spring.datasource.username=root
   spring.datasource.password=your_password
   spring.jpa.hibernate.ddl-auto=update
   spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect
   
   # JWT Configuration
   app.jwtSecret=your_jwt_secret_key_here
   app.jwtExpirationMs=86400000
   
   # Cloudinary Configuration
   cloudinary.name=your_cloud_name
   cloudinary.api_key=your_api_key
   cloudinary.api_secret=your_api_secret
   
   # Swagger/Springdoc
   springdoc.api-docs.path=/v3/api-docs
   springdoc.swagger-ui.path=/swagger-ui.html
   ```

4. **Build and run**
   ```bash
   mvn clean install
   mvn spring-boot:run
   ```

5. **Access API Documentation**
   - Swagger UI: `http://localhost:8080/swagger-ui.html`
   - API Docs: `http://localhost:8080/v3/api-docs`

---

## 📚 Database Schema

### Users Table
```
- id (PK)
- username (UNIQUE)
- email (UNIQUE)
- password (hashed)
- role (ADMIN, USER)
- created_at
- updated_at
```

### Posts Table
```
- id (PK)
- title
- content
- category_id (FK)
- author_id (FK -> Users)
- image_url
- published (boolean)
- created_at
- updated_at
```

### Comments Table
```
- id (PK)
- content
- post_id (FK)
- author_id (FK -> Users)
- created_at
- updated_at
```

### Categories Table
```
- id (PK)
- name (UNIQUE)
- description
- created_at
```

---

## 🔐 JWT Authentication

All protected endpoints require a JWT token in the Authorization header:

```
Authorization: Bearer {JWT_TOKEN}
```

**Token Structure:**
```json
{
  "sub": "username",
  "exp": 1234567890,
  "iat": 1234567890,
  "authorities": ["ROLE_USER"]
}
```

---

## 🧪 Testing

### Run Unit Tests
```bash
mvn test
```

### Test with Postman
1. Import the Postman collection from `/postman` folder (if available)
2. Set environment variables for base URL and JWT token
3. Execute test cases

---

## 📁 Project Structure

```
BlogSphere/
├── src/main/java/com/blogsphere/
│   ├── controller/        # REST Controllers
│   ├── service/           # Business logic
│   ├── repository/        # Data access layer
│   ├── entity/            # JPA entities
│   ├── dto/               # Data transfer objects
│   ├── security/          # JWT & authentication
│   ├── exception/         # Custom exceptions
│   └── BlogSphereApplication.java
├── src/main/resources/
│   ├── application.properties
│   └── data.sql           # Sample data
├── pom.xml                # Maven dependencies
└── README.md
```

---

## 🔧 Configuration Details

### Spring Security Configuration
- JWT token validation on every request
- Stateless authentication (no session storage)
- CORS configuration for frontend integration
- Password encryption using BCrypt

### Exception Handling
- Custom exceptions for specific error scenarios
- Centralized exception handler with @ControllerAdvice
- RESTful error response format

---

## 📝 Sample Usage

### Register User
```bash
curl -X POST http://localhost:8080/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"username":"john","email":"john@example.com","password":"pass123"}'
```

### Login
```bash
curl -X POST http://localhost:8080/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"username":"john","password":"pass123"}'
```

### Create Post
```bash
curl -X POST http://localhost:8080/api/posts \
  -H "Authorization: Bearer {JWT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{"title":"My First Post","content":"Great content here","categoryId":1}'
```

---

## 🐛 Troubleshooting

**Issue: Database connection failed**
- Ensure MySQL is running on port 3306
- Verify database credentials in application.properties

**Issue: JWT token expired**
- Call the refresh endpoint to get a new token

**Issue: Cloudinary upload fails**
- Verify Cloudinary credentials
- Check file size limits

---

## 🚀 Future Enhancements

- [ ] Add email notifications for new comments
- [ ] Implement pagination for posts/comments
- [ ] Add post search and filtering
- [ ] Social sharing features
- [ ] Analytics dashboard
- [ ] Docker containerization
- [ ] CI/CD integration

---

## 📄 License

This project is open source and available under the MIT License.

---

## 📧 Contact

**Author:** Kishore Reddy  
**Email:** kishoreankireddypalle@gmail.com  
**GitHub:** [Kishore-Reddy47](https://github.com/Kishore-Reddy47)

---

**Built with ❤️ using Spring Boot**
