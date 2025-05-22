# PingTag

[![MIT License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Build Status](https://img.shields.io/badge/build-passing-brightgreen.svg)]()

A social media platform driven by **interests, not people**. Users discover and engage with posts through tags. Built with React (Vite + TypeScript), Supabase (PostgreSQL, Auth, Storage), and Prisma ORM for a modern, scalable stack.

---

## 🚀 Tech Stack

| Feature     | Use                                             |
|-------------|-------------------------------------------------|
| Frontend    | React + Vite + TypeScript                       |
| Backend     | Fastify (optional) or Supabase Edge Functions   |
| Auth        | Supabase Auth (JWT + OAuth)                     |
| Media       | Supabase Storage (S3-compatible)                |
| Database    | Supabase PostgreSQL                             |
| ORM         | Prisma                                          |
| Search      | Elasticsearch (optional)                        |
| Hosting     | Vercel / Netlify (frontend), Supabase (backend) |

---

## 📦 Features

- 🔖 Tag-based feeds and posting
- 🗂️ Interest profiles (follow tags)
- 💬 Chatrooms tied to tags
- 📢 Notifications for tag activity
- 🧠 Smart tags (AI-suggested)
- 🕶️ Anonymous posting mode
- 🖼️ Media uploads (images, video) via Supabase Storage
- 🔐 Auth via Supabase (JWT + OAuth)

## 🗂 State Management

- **React Query** is used for server state (data-fetching, caching, mutations).
- For local/global UI state, use:
  - React Context (for simple, app-wide state)
  - [Zustand](https://zustand-demo.pmnd.rs/), [Jotai](https://jotai.org/), or [Redux Toolkit](https://redux-toolkit.js.org/) for more complex state needs.
- Place custom hooks for state logic in `/src/hooks/`.
- Use `/src/context/` for React Context providers.

---

## 📁 Project Structure

```
/frontend
  ├── src/
  │   ├── components/      # Reusable UI components
  │   ├── pages/           # Page-level components (routes)
  │   ├── hooks/           # Custom React hooks (including state logic)
  │   ├── services/        # API calls and data fetching logic
  │   ├── types/           # TypeScript types/interfaces
  │   ├── utils/           # Utility/helper functions
  │   ├── constants/       # Global constants (colors, text, etc.)
  │   ├── context/         # React context/providers for global state
  │   ├── assets/          # Static assets (images, fonts, etc.)
  │   └── config/          # App-wide config (theme, env, etc.)
  └── public/              # Public static files

/backend
  ├── src/
  │   ├── routes/          # Fastify route files
  │   ├── services/        # Business logic
  │   ├── models/          # Prisma models
  │   ├── plugins/         # Fastify plugins (auth, DB, logger)
  │   ├── schemas/         # Zod or JSON schemas
  │   ├── utils/           # Utility/helper functions
  │   ├── constants/       # Global constants (status codes, etc.)
  │   └── config/          # Server config (env, logger, etc.)
  └── prisma/
      ├── schema.prisma
      └── migrations/

/shared                  # Shared code (types, constants, utils) for FE/BE
  ├── types/
  ├── constants/
  └── utils/
```

---

## 🎨 Global Constants & Theming

- **Frontend:**
  - Place color palettes, typography, spacing, and other design tokens in `/src/constants/` or `/src/config/`.
  - Use TypeScript files (e.g., `colors.ts`, `typography.ts`) for JS/TS constants.
  - For CSS variables, use a global CSS file or Tailwind config (e.g., `tailwind.config.js`).
  - Example:
    ```ts
    // src/constants/colors.ts
    export const COLORS = {
      primary: '#2563eb',
      secondary: '#f59e42',
      background: '#f9fafb',
      text: '#22223b',
      // ...
    };
    ```
    ```js
    // tailwind.config.js
    module.exports = {
      theme: {
        extend: {
          colors: {
            primary: '#2563eb',
            secondary: '#f59e42',
            // ...
          },
        },
      },
    };
    ```
  - For global text (e.g., app name, slogans), use `/src/constants/text.ts`.

- **Backend:**
  - Place status codes, error messages, and other constants in `/src/constants/`.
  - Use `/src/config/` for environment and server config.

- **Shared:**
  - Use `/shared/constants/` for values used by both frontend and backend (e.g., tag names, roles, enums).

---

## 🛠️ Installation & Setup

### Prerequisites
- Node.js (v18+ recommended)
- npm or yarn
- Supabase project (see [Supabase docs](https://supabase.com/docs/guides/getting-started))
- (Optional) Elasticsearch for search features

### Clone the repository

```bash
# Clone the repo
git clone https://github.com/bantoinese83/pingtag.git
cd pingtag
```

### Install Dependencies

#### Frontend
```bash
cd frontend
npm install
```

#### Backend
```bash
cd ../backend
npm install
```

#### Prisma Setup (Backend)
```bash
# In /backend directory
npx prisma generate
npx prisma migrate dev --name init
```

#### Supabase Setup
- Create a project at [Supabase](https://app.supabase.com/)
- Set up your environment variables in `/backend/.env`:

```env
# Supabase
SUPABASE_URL=your-supabase-url
SUPABASE_ANON_KEY=your-supabase-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-supabase-service-role-key
```

---

## ⚙️ Environment Variables

Create a `.env` file in the `/backend` directory with the following:

```env
# Supabase
SUPABASE_URL=your-supabase-url
SUPABASE_ANON_KEY=your-supabase-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-supabase-service-role-key
```

---

## 🚦 Usage

### Run Dev Servers

#### Frontend
```bash
cd frontend
npm run dev
```

#### Backend
```bash
cd backend
npm run dev
```

---

## 🧪 Testing

#### Frontend
```bash
npm run test
```

#### Backend
```bash
npm run test
```

---

## 🧩 Design Document: PingTag

### 1. Overview

PingTag connects users based on **shared tags** rather than traditional follower graphs. Each tag acts as a mini-community. The app promotes discovery and serendipity in social interactions.

### 2. Architecture Overview

- **Frontend (React/Vite)** communicates with **Supabase API**
- Supabase handles: Auth, Storage, Database (PostgreSQL)
- Optional: Fastify for custom backend logic or Supabase Edge Functions
- Optional: Elasticsearch for tag/post search
- Services are stateless, following a **microservices-inspired monolith** approach

### 3. Core Modules

#### Tags
- Users follow tags to personalize their feed
- Tags contain metadata, popularity score, and relationships

#### Posts
- Every post must have at least one tag
- Supports text, image, video (uploads via Supabase Storage)

#### Chatrooms
- Chat per tag (public + anonymous mode)
- WebSocket or polling fallback

#### Notifications
- Tag activity, comment replies, mentions

#### Auth
- Supabase Auth for sessions (JWT + OAuth)

---

## 🗄️ Database Models (Prisma Schema)

```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id             String     @id @default(uuid())
  username       String     @unique
  email          String     @unique
  passwordHash   String
  bio            String?
  profilePicture String?
  posts          Post[]
  messages       ChatMessage[]
  tagFollows     TagFollow[]
  createdAt      DateTime   @default(now())
  updatedAt      DateTime   @updatedAt
}

model Post {
  id         String    @id @default(uuid())
  content    String
  mediaUrl   String?
  author     User      @relation(fields: [authorId], references: [id])
  authorId   String
  tags       Tag[]     @relation("PostTags", references: [id])
  createdAt  DateTime  @default(now())
  updatedAt  DateTime  @updatedAt

  @@index([authorId])
}

model Tag {
  id          String     @id @default(uuid())
  name        String     @unique
  description String?
  followers   TagFollow[]
  posts       Post[]     @relation("PostTags")
  messages    ChatMessage[]
  createdAt   DateTime   @default(now())
  updatedAt   DateTime   @updatedAt
}

model TagFollow {
  id        String   @id @default(uuid())
  user      User     @relation(fields: [userId], references: [id])
  userId    String
  tag       Tag      @relation(fields: [tagId], references: [id])
  tagId     String
  followedAt DateTime @default(now())

  @@unique([userId, tagId])
}

model ChatMessage {
  id        String   @id @default(uuid())
  tag       Tag      @relation(fields: [tagId], references: [id])
  tagId     String
  sender    User?    @relation(fields: [senderId], references: [id])
  senderId  String?
  message   String
  createdAt DateTime @default(now())

  @@index([tagId])
}

model Notification {
  id          String   @id @default(uuid())
  recipient   User     @relation(fields: [recipientId], references: [id])
  recipientId String
  type        String   // e.g., 'tag_activity', 'mention', 'reply'
  content     String
  read        Boolean  @default(false)
  createdAt   DateTime @default(now())
}
```

---

## 🗄️ Database Models (schema.sql)

```sql
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

CREATE TABLE "User" (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  username TEXT UNIQUE NOT NULL,
  email TEXT UNIQUE NOT NULL,
  passwordHash TEXT NOT NULL,
  bio TEXT,
  profilePicture TEXT,
  createdAt TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updatedAt TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

CREATE TABLE "Tag" (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  name TEXT UNIQUE NOT NULL,
  description TEXT,
  createdAt TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updatedAt TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

CREATE TABLE "Post" (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  content TEXT NOT NULL,
  mediaUrl TEXT,
  authorId UUID NOT NULL REFERENCES "User"(id),
  createdAt TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updatedAt TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
CREATE INDEX idx_post_authorId ON "Post"(authorId);

CREATE TABLE "TagFollow" (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  userId UUID NOT NULL REFERENCES "User"(id),
  tagId UUID NOT NULL REFERENCES "Tag"(id),
  followedAt TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  UNIQUE(userId, tagId)
);

CREATE TABLE "ChatMessage" (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  tagId UUID NOT NULL REFERENCES "Tag"(id),
  senderId UUID REFERENCES "User"(id),
  message TEXT NOT NULL,
  createdAt TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
CREATE INDEX idx_chatmessage_tagId ON "ChatMessage"(tagId);

CREATE TABLE "Notification" (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  recipientId UUID NOT NULL REFERENCES "User"(id),
  type TEXT NOT NULL,
  content TEXT NOT NULL,
  read BOOLEAN DEFAULT FALSE,
  createdAt TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

---

## ☁️ Supabase Storage Setup

- Create a storage bucket in your Supabase project for media uploads (images, videos, etc.).
- Set the bucket name and credentials in your `.env` file as shown above.
- Ensure your Supabase service role key has the correct permissions for storage operations.

---

## API Endpoints

### Auth
- `POST   /auth/register` — Register a new user
- `POST   /auth/login` — Login and receive a token
- `POST   /auth/logout` — Logout (invalidate token)
- `GET    /auth/me` — Get current authenticated user
- `POST   /auth/refresh` — Refresh JWT token
- `POST   /auth/password/forgot` — Request password reset
- `POST   /auth/password/reset` — Reset password

### Users
- `GET    /users/:id` — Get user profile by ID
- `GET    /users/username/:username` — Get user by username
- `PATCH  /users/:id` — Update user profile (bio, picture, etc.)
- `DELETE /users/:id` — Delete user account
- `GET    /users/:id/posts` — Get posts by user
- `GET    /users/:id/followed-tags` — Get tags followed by user
- `GET    /users/:id/notifications` — Get notifications for user

### Tags
- `GET    /tags` — List all tags (with search/filter)
- `POST   /tags` — Create a new tag (admin/mod only)
- `GET    /tags/:id` — Get tag details
- `PATCH  /tags/:id` — Update tag (admin/mod only)
- `DELETE /tags/:id` — Delete tag (admin/mod only)
- `POST   /tags/:id/follow` — Follow a tag
- `DELETE /tags/:id/follow` — Unfollow a tag
- `GET    /tags/:id/followers` — List users following a tag
- `GET    /tags/:id/posts` — List posts for a tag
- `GET    /tags/:id/chat` — Get chat messages for a tag

### Posts
- `GET    /posts` — List all posts (with search/filter)
- `POST   /posts` — Create a new post
- `GET    /posts/:id` — Get post by ID
- `PATCH  /posts/:id` — Edit post
- `DELETE /posts/:id` — Delete post
- `POST   /posts/:id/like` — Like a post
- `DELETE /posts/:id/like` — Unlike a post
- `POST   /posts/:id/report` — Report a post

### Chat (per Tag)
- `GET    /chat/:tagId` — Get messages for a tag chatroom
- `POST   /chat/:tagId` — Send a message to tag chatroom
- `GET    /chat/:tagId/messages/:messageId` — Get a specific message
- `DELETE /chat/:tagId/messages/:messageId` — Delete a message (self or mod)
- `POST   /chat/:tagId/messages/:messageId/report` — Report a message

### Notifications
- `GET    /notifications` — List notifications for current user
- `PATCH  /notifications/:id/read` — Mark notification as read
- `PATCH  /notifications/read-all` — Mark all as read

### Media (Supabase Storage)
- `POST   /media/upload` — Upload media (image/video)
- `GET    /media/:id` — Get media by ID or URL
- `DELETE /media/:id` — Delete media (owner or admin)

### Search (Optional/Advanced)
- `GET    /search/tags?q=...` — Search tags
- `GET    /search/posts?q=...` — Search posts
- `GET    /search/users?q=...` — Search users

### Admin/Moderation (Optional)
- `GET    /admin/reports` — List reported posts/messages
- `PATCH  /admin/posts/:id/moderate` — Moderate a post
- `PATCH  /admin/messages/:id/moderate` — Moderate a chat message
- `GET    /admin/users` — List all users

---

## 🤝 Contributing

Contributions, issues and feature requests are welcome! Feel free to check the [issues page](https://github.com/bantoinese83/pingtag/issues) or submit a pull request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/YourFeature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin feature/YourFeature`)
5. Create a new Pull Request

---

## 📜 License

This project is [MIT](LICENSE) licensed.

---



