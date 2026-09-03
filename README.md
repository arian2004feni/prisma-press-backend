# 📰 Prisma Press Backend

A production-style REST API backend for **Prisma Press**, a subscription-based publishing platform where users can create and manage posts, interact through comments, and access premium content through Stripe subscriptions.

The project is built with **Node.js, Express.js, TypeScript, PostgreSQL, Prisma ORM, JWT authentication, and Stripe**.

---

## ✨ Features

* 🔐 JWT-based authentication
* 🍪 HTTP-only cookie-based token handling
* 👥 Role-based access control
* 📝 Create, read, update, and delete posts
* 👤 User profile management
* 💬 Comment system
* 🛡️ Admin comment moderation
* 📊 Post statistics for administrators
* 💎 Premium content protection
* 💳 Stripe subscription checkout
* 🔔 Stripe webhook handling
* 🔄 Access-token refresh mechanism
* 🗄️ PostgreSQL database with Prisma ORM
* 🧩 Modular backend architecture
* ⚠️ Centralized error handling
* 🔎 Pagination/filtering support for posts
* 🌐 CORS and cookie support

---

## 🛠️ Technology Stack

| Technology        | Purpose                         |
| ----------------- | ------------------------------- |
| **Node.js**       | JavaScript runtime              |
| **Express.js**    | REST API framework              |
| **TypeScript**    | Type safety                     |
| **PostgreSQL**    | Relational database             |
| **Prisma ORM**    | Database access & migrations    |
| **JWT**           | Authentication                  |
| **bcryptjs**      | Password hashing                |
| **Stripe**        | Subscription/payment processing |
| **cookie-parser** | Cookie handling                 |
| **CORS**          | Cross-origin requests           |
| **dotenv**        | Environment configuration       |
| **tsx**           | TypeScript development server   |

The repository currently uses Express 5, Prisma 7, TypeScript 6, PostgreSQL via `pg`, and Stripe.

---

# 📋 Prerequisites

Make sure the following are installed on your machine:

* **Node.js** 18+
* **npm**
* **PostgreSQL**
* **Git**
* **Stripe CLI** — required if you want to test Stripe webhooks locally

---

# 🚀 Installation

## 1. Clone the repository

```bash
git clone https://github.com/Apollo-Level2-Web-Dev/project-prisma-press-backend.git

cd project-prisma-press-backend
```

Or clone your own fork:

```bash
git clone https://github.com/<your-username>/<your-repository>.git

cd <your-repository>
```

---

## 2. Install dependencies

```bash
npm install
```

---

## 3. Configure environment variables

Create a `.env` file in the project root:

```bash
cp .env.example .env
```

For Windows PowerShell:

```powershell
Copy-Item .env.example .env
```

The project already includes an `.env.example` containing the required database, authentication, and Stripe configuration variables.

---

# ⚙️ Configuration

Configure your `.env` file like this:

```env
# Server
PORT=5000
APP_URL=http://localhost:3000

# Database
DATABASE_URL="postgresql://USERNAME:PASSWORD@localhost:5432/prisma_press"

# Password hashing
BCRYPT_SALT_ROUNDS=10

# JWT
JWT_ACCESS_SECRET=your-access-secret
JWT_REFRESH_SECRET=your-refresh-secret

JWT_ACCESS_EXPIRES_IN=1d
JWT_REFRESH_EXPIRES_IN=7d

# Stripe
STRIPE_PRODUCT_PRICE_ID=your_stripe_price_id
STRIPE_SECRET_KEY=your_stripe_secret_key
STRIPE_WEBHOOK_SECRET=your_stripe_webhook_secret
```

### Environment variables

| Variable                  | Description                   | Example                 |
| ------------------------- | ----------------------------- | ----------------------- |
| `PORT`                    | Backend server port           | `5000`                  |
| `APP_URL`                 | Frontend URL allowed by CORS  | `http://localhost:3000` |
| `DATABASE_URL`            | PostgreSQL connection string  | `postgresql://...`      |
| `BCRYPT_SALT_ROUNDS`      | bcrypt hashing rounds         | `10`                    |
| `JWT_ACCESS_SECRET`       | Access-token signing secret   | `your-secret`           |
| `JWT_REFRESH_SECRET`      | Refresh-token signing secret  | `your-secret`           |
| `JWT_ACCESS_EXPIRES_IN`   | Access-token lifetime         | `1d`                    |
| `JWT_REFRESH_EXPIRES_IN`  | Refresh-token lifetime        | `7d`                    |
| `STRIPE_PRODUCT_PRICE_ID` | Stripe subscription Price ID  | `price_xxx`             |
| `STRIPE_SECRET_KEY`       | Stripe secret key             | `sk_test_xxx`           |
| `STRIPE_WEBHOOK_SECRET`   | Stripe webhook signing secret | `whsec_xxx`             |

> **Important:** Never commit your `.env` file or expose JWT/Stripe secrets publicly.

The application configuration reads `DATABASE_URL`, JWT variables, `APP_URL`, and `STRIPE_PRODUCT_PRICE_ID` from the environment.

---

# 🗄️ Database Setup

This project uses **PostgreSQL + Prisma**.

The Prisma configuration points to:

```text
prisma/schema
```

and migrations are stored in:

```text
prisma/migrations
```

The project uses multiple Prisma schema files instead of putting every model into one large schema file.

### Generate Prisma Client

```bash
npx prisma generate
```

### Run database migrations

For an existing project with committed migrations:

```bash
npx prisma migrate deploy
```

For local development when creating a new migration:

```bash
npx prisma migrate dev
```

### Open Prisma Studio

```bash
npx prisma studio
```

Prisma Studio allows you to inspect and manage your database through a graphical interface.

---

# ▶️ Running the Application

## Development

```bash
npm run dev
```

The development server uses `tsx watch` and starts from:

```text
src/server.ts
```

The API will normally be available at:

```text
http://localhost:5000
```

---

## Production Build

Compile TypeScript:

```bash
npm run build
```

Start the compiled application:

```bash
npm start
```

The project currently defines:

```json
{
  "dev": "tsx watch src/server.ts",
  "build": "tsc",
  "start": "node dist/server.js"
}
```

---

# 🏗️ How Is It Structured?

The project follows a **modular architecture**.

Instead of putting every controller, service, and route into global folders, each major business domain has its own module.

```text
project-prisma-press-backend/
│
├── prisma/
│   ├── migrations/
│   └── schema/
│       ├── comment.prisma
│       ├── enums.prisma
│       ├── post.prisma
│       ├── profile.prisma
│       ├── schema.prisma
│       ├── subscription.prisma
│       └── user.prisma
│
├── src/
│   ├── config/
│   │   └── index.ts
│   │
│   ├── lib/
│   │   └── prisma.ts
│   │
│   ├── middlewares/
│   │   ├── auth.ts
│   │   ├── globalErrorHandler.ts
│   │   ├── notFound.ts
│   │   └── premiumGuard.ts
│   │
│   ├── modules/
│   │   ├── auth/
│   │   │   ├── auth.controller.ts
│   │   │   ├── auth.interface.ts
│   │   │   ├── auth.routes.ts
│   │   │   └── auth.service.ts
│   │   │
│   │   ├── comment/
│   │   │   ├── comment.controller.ts
│   │   │   ├── comment.interface.ts
│   │   │   ├── comment.route.ts
│   │   │   └── comment.service.ts
│   │   │
│   │   ├── post/
│   │   │   ├── post.controller.ts
│   │   │   ├── post.interface.ts
│   │   │   ├── post.route.ts
│   │   │   └── post.service.ts
│   │   │
│   │   ├── premium/
│   │   │   ├── premium.controller.ts
│   │   │   ├── premium.route.ts
│   │   │   └── premium.service.ts
│   │   │
│   │   ├── subscription/
│   │   │   ├── subscription.controller.ts
│   │   │   ├── subscription.route.ts
│   │   │   ├── subscription.service.ts
│   │   │   └── subscription.utils.ts
│   │   │
│   │   └── user/
│   │       ├── user.controller.ts
│   │       ├── user.interface.ts
│   │       ├── user.route.ts
│   │       └── user.service.ts
│   │
│   ├── utils/
│   │   ├── catchAsync.ts
│   │   └── sendResponse.ts
│   │
│   ├── app.ts
│   └── server.ts
│
├── .env.example
├── prisma.config.ts
├── package.json
├── tsconfig.json
└── README.md
```

The actual repository follows this module structure, with separate modules for authentication, comments, posts, premium content, subscriptions, and users.

---

# 🧩 Architecture

The general request flow is:

```text
Client
  │
  ▼
Express Route
  │
  ▼
Middleware
  │
  ├── Authentication
  ├── Role Authorization
  └── Premium Guard
  │
  ▼
Controller
  │
  ▼
Service
  │
  ▼
Prisma Client
  │
  ▼
PostgreSQL
```

### Route

Responsible for defining the API endpoint and applying required middleware.

### Middleware

Responsible for cross-cutting concerns such as:

* Authentication
* Authorization
* Premium subscription checks
* 404 handling
* Global error handling

### Controller

Responsible for:

* Receiving the HTTP request
* Extracting parameters/body/query
* Calling the service
* Returning the HTTP response

### Service

Contains the actual business logic and database operations.

### Prisma

Provides the database abstraction layer between the application and PostgreSQL.

---

# 🔐 Authentication & Authorization

The application uses JWT-based authentication.

Users receive:

* Access Token
* Refresh Token

The tokens are also stored in HTTP-only cookies.

The login controller sets:

```text
accessToken
refreshToken
```

with different expiration periods.

---

## 👥 Roles

The API currently works with these roles:

| Role     | Description                      |
| -------- | -------------------------------- |
| `USER`   | Normal platform user             |
| `AUTHOR` | User who can create/manage posts |
| `ADMIN`  | Platform administrator           |

Authorization is implemented through the `auth()` middleware.

For example:

```ts
auth(Role.USER, Role.ADMIN, Role.AUTHOR)
```

allows all three roles to access the protected route.

Admin-only operations use:

```ts
auth(Role.ADMIN)
```

---

# 📡 API Endpoints

Base URL:

```text
http://localhost:5000/api
```

---

## 🔑 Authentication

### Login

```http
POST /api/auth/login
```

Authenticate a user and issue access/refresh tokens.

**Authentication:** Public

---

### Refresh Access Token

```http
POST /api/auth/refresh-token
```

Generates a new access token using the refresh token.

**Authentication:** Refresh token required

The authentication module exposes these two endpoints.

---

# 👤 Users

### Register

```http
POST /api/users/register
```

Create a new user account.

**Authentication:** Public

---

### Get My Profile

```http
GET /api/users/me
```

Returns the authenticated user's profile.

**Roles:**

```text
USER
AUTHOR
ADMIN
```

---

### Update My Profile

```http
PUT /api/users/my-profile
```

Updates the authenticated user's profile.

**Roles:**

```text
USER
AUTHOR
ADMIN
```

The user routes currently expose registration, authenticated profile retrieval, and profile updating.

---

# 📝 Posts

### Create Post

```http
POST /api/posts
```

Create a new post.

**Roles:**

```text
USER
AUTHOR
ADMIN
```

---

### Get All Posts

```http
GET /api/posts
```

Retrieve published posts.

**Authentication:** Public

Supports query parameters for retrieving/filtering posts.

Example:

```http
GET /api/posts?page=1&limit=10
```

---

### Get Post by ID

```http
GET /api/posts/:postId
```

Retrieve a specific post.

**Authentication:** Public

---

### Get My Posts

```http
GET /api/posts/my-posts
```

Retrieve posts created by the authenticated user.

**Roles:**

```text
USER
AUTHOR
ADMIN
```

---

### Get Post Statistics

```http
GET /api/posts/stats
```

Retrieve post statistics.

**Role:**

```text
ADMIN
```

---

### Update Post

```http
PATCH /api/posts/:postId
```

Update an existing post.

**Roles:**

```text
USER
AUTHOR
ADMIN
```

Authors can manage their own posts, while administrators can manage posts with elevated privileges.

---

### Delete Post

```http
DELETE /api/posts/:postId
```

Delete a post.

**Roles:**

```text
USER
AUTHOR
ADMIN
```

The post module currently defines all seven of these operations.

---

# 💬 Comments

### Create Comment

```http
POST /api/comments
```

Create a comment on a post.

**Roles:**

```text
USER
AUTHOR
ADMIN
```

---

### Get Comments by Post

```http
GET /api/comments/:postId
```

Retrieve comments belonging to a post.

**Authentication:** Public

---

### Get Comments by Author

```http
GET /api/comments/author/:authorId
```

Retrieve comments created by a specific author.

**Authentication:** Public

---

### Update Comment

```http
PATCH /api/comments/:commentId
```

Update a comment.

**Roles:**

```text
USER
AUTHOR
ADMIN
```

---

### Delete Comment

```http
DELETE /api/comments/:commentId
```

Delete a comment.

**Roles:**

```text
USER
AUTHOR
ADMIN
```

---

### Moderate Comment

```http
PUT /api/comments/:commentId/moderate
```

Moderate a comment.

**Role:**

```text
ADMIN
```

The comment router applies admin-only authorization to moderation while allowing authenticated users to manage their comments.

---

# 💳 Subscriptions

The project integrates Stripe for premium subscriptions.

---

### Create Checkout Session

```http
POST /api/subscription/checkout
```

Creates a Stripe Checkout session for subscribing to the premium plan.

**Roles:**

```text
USER
AUTHOR
ADMIN
```

---

### Stripe Webhook

```http
POST /api/subscription/webhook
```

Stripe sends subscription/payment events to this endpoint.

**Authentication:** Stripe webhook signature

The application configures this endpoint to receive raw JSON because Stripe webhook signature verification requires the original request body.

---

### Get Subscription Status

```http
GET /api/subscription/status
```

Returns the current subscription status of the authenticated user.

**Roles:**

```text
USER
AUTHOR
ADMIN
```

---

# 💎 Premium Content

### Get Premium Content

```http
GET /api/premium
```

Returns premium content to users with an active subscription.

**Roles:**

```text
USER
AUTHOR
ADMIN
```

This endpoint uses two layers of protection:

```text
Authentication
      ↓
Subscription Guard
      ↓
Premium Content
```

The route explicitly applies both the authentication middleware and `subscriptionGuard()`.

---

# 📊 API Summary

| Method   | Endpoint                            | Auth        | Role              |
| -------- | ----------------------------------- | ----------- | ----------------- |
| `POST`   | `/api/auth/login`                   | ❌           | Public            |
| `POST`   | `/api/auth/refresh-token`           | 🔑          | Authenticated     |
| `POST`   | `/api/users/register`               | ❌           | Public            |
| `GET`    | `/api/users/me`                     | ✅           | User/Author/Admin |
| `PUT`    | `/api/users/my-profile`             | ✅           | User/Author/Admin |
| `POST`   | `/api/posts`                        | ✅           | User/Author/Admin |
| `GET`    | `/api/posts`                        | ❌           | Public            |
| `GET`    | `/api/posts/stats`                  | ✅           | Admin             |
| `GET`    | `/api/posts/my-posts`               | ✅           | User/Author/Admin |
| `GET`    | `/api/posts/:postId`                | ❌           | Public            |
| `PATCH`  | `/api/posts/:postId`                | ✅           | User/Author/Admin |
| `DELETE` | `/api/posts/:postId`                | ✅           | User/Author/Admin |
| `POST`   | `/api/comments`                     | ✅           | User/Author/Admin |
| `GET`    | `/api/comments/author/:authorId`    | ❌           | Public            |
| `GET`    | `/api/comments/:postId`             | ❌           | Public            |
| `PATCH`  | `/api/comments/:commentId`          | ✅           | User/Author/Admin |
| `DELETE` | `/api/comments/:commentId`          | ✅           | User/Author/Admin |
| `PUT`    | `/api/comments/:commentId/moderate` | ✅           | Admin             |
| `POST`   | `/api/subscription/checkout`        | ✅           | User/Author/Admin |
| `POST`   | `/api/subscription/webhook`         | ❌*          | Stripe            |
| `GET`    | `/api/subscription/status`          | ✅           | User/Author/Admin |
| `GET`    | `/api/premium`                      | ✅ + Premium | User/Author/Admin |

> `*` The webhook is not protected by application JWT authentication; it is intended to be called by Stripe and should be secured through Stripe webhook signature verification.

---

# 💰 Stripe Local Development

If you want to test Stripe webhooks locally, install the **Stripe CLI** and authenticate it with your Stripe account.

Then run:

```bash
stripe login
```

Start the webhook listener:

```bash
npm run stripe:webhook
```

The project's script forwards Stripe events to:

```text
localhost:5000/api/subscription/webhook
```

This command is already defined in `package.json`.

Stripe will provide a webhook signing secret. Add it to:

```env
STRIPE_WEBHOOK_SECRET=whsec_xxxxxxxxx
```

---

# 🧪 Testing the API

A Postman collection is included in the repository:

```text
Prisma Press Backend.postman_collection.json
```

You can import this file into **Postman** and test the available endpoints.

Recommended testing flow:

```text
1. Register
      ↓
2. Login
      ↓
3. Create Post
      ↓
4. Get Posts
      ↓
5. Get Post Details
      ↓
6. Create Comment
      ↓
7. Update/Delete Comment
      ↓
8. Subscribe through Stripe
      ↓
9. Verify Subscription
      ↓
10. Access Premium Content
```

The repository includes both the Postman collection and the project requirements documentation.

---

# 🔄 Authentication Flow

```text
             ┌───────────────┐
             │     Login     │
             └───────┬───────┘
                     │
                     ▼
             ┌───────────────┐
             │ Verify User   │
             │ & Password    │
             └───────┬───────┘
                     │
                     ▼
          ┌──────────────────────┐
          │ Generate JWT Tokens  │
          └──────────┬───────────┘
                     │
             ┌───────┴────────┐
             ▼                ▼
       Access Token      Refresh Token
             │                │
             ▼                ▼
        API Access       Token Refresh
```

The login endpoint returns both tokens and sets them as cookies.

---

# 💎 Subscription Flow

```text
User
 │
 ▼
POST /api/subscription/checkout
 │
 ▼
Backend
 │
 ▼
Stripe Checkout
 │
 ▼
User completes payment
 │
 ▼
Stripe
 │
 ▼
POST /api/subscription/webhook
 │
 ▼
Backend verifies event
 │
 ▼
Subscription updated
 │
 ▼
User can access
Premium Content
```

---

# 🛡️ Error Handling

The project uses centralized error handling.

The middleware layer contains:

```text
globalErrorHandler.ts
notFound.ts
```

Async controller operations are wrapped through utility helpers such as:

```text
catchAsync.ts
sendResponse.ts
```

This keeps controllers relatively clean and provides a consistent response pattern.

---

# 📦 NPM Scripts

| Command                  | Description                     |
| ------------------------ | ------------------------------- |
| `npm run dev`            | Start development server        |
| `npm run build`          | Compile TypeScript              |
| `npm start`              | Run production build            |
| `npm run stripe:webhook` | Forward Stripe webhooks locally |

These scripts are defined in the current `package.json`.

---

# 🔒 Security Considerations

For production deployments:

* Use strong random JWT secrets.
* Never commit `.env`.
* Use HTTPS.
* Set secure cookie options.
* Configure CORS to allow only trusted frontend origins.
* Verify Stripe webhook signatures.
* Never expose Stripe secret keys.
* Use environment variables for all secrets.
* Validate incoming request data.
* Apply proper authorization checks to sensitive resources.
* Use production database credentials instead of local development credentials.

---

# 🚀 Production Deployment

Before deploying:

### 1. Configure production environment variables

Set all required variables in your hosting provider.

### 2. Build the project

```bash
npm run build
```

### 3. Run migrations

```bash
npx prisma migrate deploy
```

### 4. Start the application

```bash
npm start
```

### 5. Configure Stripe

Update the Stripe webhook endpoint to your deployed API:

```text
https://your-domain.com/api/subscription/webhook
```

---

# 🗂️ Important Project Files

| File/Directory                          | Responsibility                    |
| --------------------------------------- | --------------------------------- |
| `src/server.ts`                         | Application entry point           |
| `src/app.ts`                            | Express application configuration |
| `src/config/`                           | Environment/configuration         |
| `src/lib/prisma.ts`                     | Prisma client                     |
| `src/middlewares/auth.ts`               | Authentication & authorization    |
| `src/middlewares/premiumGuard.ts`       | Premium subscription protection   |
| `src/middlewares/globalErrorHandler.ts` | Global error handling             |
| `src/modules/auth/`                     | Authentication                    |
| `src/modules/user/`                     | User management                   |
| `src/modules/post/`                     | Post management                   |
| `src/modules/comment/`                  | Comment management                |
| `src/modules/subscription/`             | Stripe subscriptions              |
| `src/modules/premium/`                  | Premium content                   |
| `prisma/schema/`                        | Database schema                   |
| `prisma/migrations/`                    | Database migrations               |
| `prisma.config.ts`                      | Prisma configuration              |

The server establishes a Prisma database connection before starting the Express server.

---

# 🧠 Architectural Principles

This project follows several backend development principles:

### Separation of Concerns

Each layer has a specific responsibility:

```text
Route
 ↓
Controller
 ↓
Service
 ↓
Database
```

### Modularization

Business domains are isolated:

```text
Auth
User
Post
Comment
Subscription
Premium
```

### Reusable Middleware

Authentication, authorization, premium checks, 404 handling, and error handling are implemented as reusable middleware.

### Database Abstraction

Application code interacts with PostgreSQL through Prisma rather than directly writing database queries throughout the business logic.

---

# 🔗 Related Resources

* **Backend Repository:**
  [Prisma Press Backend](https://github.com/Apollo-Level2-Web-Dev/project-prisma-press-backend?utm_source=chatgpt.com)

* **Frontend Repository:**
  [Prisma Press Frontend](https://github.com/Apollo-Level2-Web-Dev/project-nextjs-press-frontend?utm_source=chatgpt.com)

* **Prisma Documentation:**
  [Prisma Docs](https://www.prisma.io/docs?utm_source=chatgpt.com)

* **Stripe Documentation:**
  [Stripe Docs](https://docs.stripe.com/?utm_source=chatgpt.com)

---

# 👨‍💻 Author

**Arian**

Full-Stack Web Developer focused on building modern, scalable web applications with TypeScript, React, Next.js, Node.js, Express, PostgreSQL, Prisma, and modern backend architecture.

---

## ⭐ If You Find This Project Useful

Give the repository a ⭐ on GitHub and feel free to explore, fork, and improve it.

---

## 📄 License

This project is intended for educational and development purposes.
