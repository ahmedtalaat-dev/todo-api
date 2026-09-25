# Todo API

A simple RESTful Todo API built with **Node.js**, **Express.js**, and **MongoDB**.

The API supports user authentication with **JWT**, password hashing with **bcrypt**, and CRUD operations for tasks.

## 🚀 Features

* User registration
* User login
* Password hashing with bcrypt
* JWT authentication
* Create tasks
* Get all user's tasks
* Get a single task
* Update tasks
* Delete tasks
* Task ownership
* MongoDB database
* RESTful API structure

## 🛠️ Technologies

* Node.js
* Express.js
* MongoDB
* Mongoose
* JSON Web Token (JWT)
* bcryptjs
* dotenv
* cors

## 📁 Project Structure

```text
todo-api/
│
├── src/
│   ├── config/
│   │   └── db.js
│   │
│   ├── controllers/
│   │   ├── auth.controller.js
│   │   └── task.controller.js
│   │
│   ├── middleware/
│   │   └── auth.middleware.js
│   │
│   ├── models/
│   │   ├── User.js
│   │   └── Task.js
│   │
│   ├── routes/
│   │   ├── auth.routes.js
│   │   └── task.routes.js
│   │
│   ├── utils/
│   │   └── generateToken.js
│   │
│   └── app.js
│
├── .env
├── .gitignore
├── package.json
├── package-lock.json
└── README.md
```

## ⚙️ Installation

### 1. Clone the repository

```bash
git clone https://github.com/ahmedtalaat-dev/todo-api.git
```

### 2. Navigate to the project

```bash
cd todo-api
```

### 3. Install dependencies

```bash
npm install
```

### 4. Configure environment variables

Create a `.env` file in the root directory:

```env
PORT=5000
MONGO_URI=mongodb://127.0.0.1:27017/todo-api
JWT_SECRET=your_super_secret_jwt_key
JWT_EXPIRES_IN=7d
```

### 5. Start the development server

```bash
node app.js
```

The API will be available at:

```text
http://localhost:5000
```

## 🔐 Authentication

The API uses **JWT (JSON Web Token)** for authentication.

After registering or logging in, the API returns a token.

Use the token in the `Authorization` header when accessing protected endpoints:

```http
Authorization: Bearer YOUR_TOKEN
```

## 👤 User Model

The User model contains:

| Field      | Type   | Required |
| ---------- | ------ | -------- |
| `name`     | String | Yes      |
| `email`    | String | Yes      |
| `password` | String | Yes      |

Passwords are hashed using **bcryptjs** before being stored in MongoDB.

## 📝 Task Model

The Task model contains:

| Field         | Type     | Required |
| ------------- | -------- | -------- |
| `title`       | String   | Yes      |
| `description` | String   | No       |
| `status`      | String   | No       |
| `createdBy`   | ObjectId | Yes      |

Available task statuses:

* `pending`
* `in-progress`
* `completed`

The `createdBy` field stores the MongoDB ObjectId of the user who created the task.

## 🌐 API Endpoints

### Health Check

| Method | Endpoint | Authentication |
| ------ | -------- | -------------- |
| GET    | `/`      | No             |

### Authentication

| Method | Endpoint             | Authentication |
| ------ | -------------------- | -------------- |
| POST   | `/api/auth/register` | No             |
| POST   | `/api/auth/login`    | No             |

### Tasks

| Method | Endpoint         | Authentication |
| ------ | ---------------- | -------------- |
| POST   | `/api/tasks`     | Yes            |
| GET    | `/api/tasks`     | Yes            |
| GET    | `/api/tasks/:id` | Yes            |
| PUT    | `/api/tasks/:id` | Yes            |
| DELETE | `/api/tasks/:id` | Yes            |

## 📌 API Examples

### Register

**POST**

```text
/api/auth/register
```

Request body:

```json
{
  "name": "Ahmed",
  "email": "ahmed@example.com",
  "password": "123456"
}
```

Response:

```json
{
  "message": "User registered successfully",
  "token": "YOUR_JWT_TOKEN",
  "user": {
    "id": "USER_ID",
    "name": "Ahmed",
    "email": "ahmed@example.com"
  }
}
```

### Login

**POST**

```text
/api/auth/login
```

Request body:

```json
{
  "email": "ahmed@example.com",
  "password": "123456"
}
```

Response:

```json
{
  "message": "Login successful",
  "token": "YOUR_JWT_TOKEN",
  "user": {
    "id": "USER_ID",
    "name": "Ahmed",
    "email": "ahmed@example.com"
  }
}
```

### Create Task

**POST**

```text
/api/tasks
```

Headers:

```http
Authorization: Bearer YOUR_JWT_TOKEN
Content-Type: application/json
```

Request body:

```json
{
  "title": "Learn Express",
  "description": "Build a Todo API using Express and MongoDB",
  "status": "pending"
}
```

The `createdBy` field does not need to be sent by the client. It is automatically taken from the authenticated user's JWT.

### Get All Tasks

**GET**

```text
/api/tasks
```

Headers:

```http
Authorization: Bearer YOUR_JWT_TOKEN
```

Returns the tasks belonging to the authenticated user.

### Get One Task

**GET**

```text
/api/tasks/:id
```

Example:

```text
/api/tasks/68c123456789abcdef123456
```

Headers:

```http
Authorization: Bearer YOUR_JWT_TOKEN
```

### Update Task

**PUT**

```text
/api/tasks/:id
```

Headers:

```http
Authorization: Bearer YOUR_JWT_TOKEN
Content-Type: application/json
```

Request body:

```json
{
  "title": "Learn Express and MongoDB",
  "description": "Build a complete Todo API",
  "status": "in-progress"
}
```

You can also update individual fields:

```json
{
  "status": "completed"
}
```

### Delete Task

**DELETE**

```text
/api/tasks/:id
```

Headers:

```http
Authorization: Bearer YOUR_JWT_TOKEN
```

Response:

```json
{
  "message": "Task deleted successfully"
}
```

## 🔒 Task Ownership

Each task belongs to the user who created it.

The `createdBy` field stores the user's MongoDB ObjectId:

```json
{
  "title": "Learn Node.js",
  "createdBy": "68c123456789abcdef123456"
}
```

When retrieving, updating, or deleting a task, the API checks both:

```text
Task ID
+
Authenticated User ID
```

This prevents users from accessing or modifying tasks belonging to another user.

## 🧪 Testing with Postman

You can test the API using Postman.

Recommended testing order:

1. Register
2. Login
3. Copy the JWT token
4. Create a task
5. Get all tasks
6. Get one task
7. Update the task
8. Delete the task

For protected endpoints, add:

```http
Authorization: Bearer YOUR_JWT_TOKEN
```

## 🔑 Environment Variables

| Variable         | Description                    |
| ---------------- | ------------------------------ |
| `PORT`           | Server port                    |
| `MONGO_URI`      | MongoDB connection string      |
| `JWT_SECRET`     | Secret used to sign JWT tokens |
| `JWT_EXPIRES_IN` | JWT expiration time            |

Example:

```env
PORT=5000
MONGO_URI=mongodb://127.0.0.1:27017/todo-api
JWT_SECRET=your_super_secret_jwt_key
JWT_EXPIRES_IN=7d
```

## 📄 License

This project is available for learning and development purposes.
