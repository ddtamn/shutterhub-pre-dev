# ShutterHub Pre-Development Documentation

## Table of Contents

1. [Project Overview](#1-project-overview)
2. [Features](#2-features)
   - [Core Features](#core-features)
   - [Additional Features](#additional-features)
3. [Tech Stack](#3-tech-stack)
4. [Workflow](#4-workflow)
5. [Database Schema (Prisma)](#5-database-schema-prisma)
6. [Project Roadmap](#6-project-roadmap)
7. [Milestones](#7-milestones)


---

## 1. **Project Overview**

ShutterHub is a platform designed for photographers and event organizers to manage, showcase, and monetize their content. The platform includes user registration, content uploads, search functionality, and payment processing.

### Goals:

- Simplify photo sharing and selling.
- Provide a seamless user experience for both creators and buyers.
- Integrate with scalable third-party services like Vercel, Supabase, AWS, Cloudflare, and Midtrans.

### Target Audience:

- Event organizers (e.g., fun runs, weddings).
- Professional photographers and content creators.
- Individuals looking to purchase high-quality images.

---

## 2. **Features**

### **Core Features**

1. **User Authentication:**

   - Authentication system uses Better-Auth for enhanced security and simplicity.
   - Email-based registration and login.
   - Social login options (Google, Facebook, etc.).
   - Password reset with email verification.

2. **Profile Management:**

   - Editable user profiles with unique URLs (e.g., `sh.id/<username>`).
   - Profile picture uploads.

3. **Content Management:**

   - Image uploads with metadata (tags, descriptions, etc.).
   - Folder and subfolder organization.
   - Image watermarking using Cloudflare Workers.

4. **Search Functionality:**

   - Advanced search with filters (e.g., tags, prices, dates).
   - Face Based Search

5. **E-Commerce:**

   - Shopping cart for users to add and purchase content.
   - Secure payment processing via Midtrans.
   - Order history for users.

6. **Balance Management:**

   - Top-up functionality integrated with Midtrans.
   - Transaction records for transparency.

7. **Dashboard:**

   - Metrics for creators (e.g., sales, views).
   - Admin dashboard for managing users and content.

### **Additional Features**

- Notifications via email (using Resend).
- Analytics tracking for user engagement.
- SEO optimization with meta tags and clean URLs.

---

## 3. **Tech Stack**

### **Frontend:**

- Framework: SvelteKit
- Styling: TailwindCSS
- State Management: Svelte Store

### **Backend:**

- Database: Supabase
- ORM: Prisma
- Server: Node.js (with SvelteKit server-side rendering)

### **Third-Party Services:**

- File Storage: Cloudflare R2.
- Payments: Midtrans.
- Email: Resend.
- Image Optimization: Cloudflare Workers (for resizing & watermarking).
- Image Search by face: AWS Recognition

### **DevOps:**

- Hosting: Vercel
- DNS: Cloudflare
- CI/CD: GitHub Actions

---

## 4. **Workflow**

### **User Registration Flow:**

1. User signs up via email or social login.
2. A confirmation email is sent to activate the account.
3. Users complete their profile (add profile picture, set username, etc.).

### **Content Upload Flow:**

1. Creator uploads images to Cloudflare R2.
2. Metadata (e.g., tags, price, and location) is added during the upload process.
3. Watermarked versions are generated via Cloudflare Workers.
4. Content is organized into Space.

### **Search Flow:**

1. User enters search query.
2. AWS Recognition returns relevant results based on metadata and similarity.
3. Results are displayed with filters and sorting options.

### **Payment Flow:**

1. User adds content to the cart.
2. Midtrans processes the payment.
3. Order details are stored in the database.
4. Users can download purchased content.

---

## 5. ** Sample Database Schema (Prisma)**

### **User Table**

```prisma
model User {
  id           String        @id @default(uuid())
  username     String        @unique
  email        String        @unique
  passwordHash String
  createdAt    DateTime      @default(now())
  updatedAt    DateTime      @updatedAt
  roles        UserRole[]
  profile      Profile?
  sessions     Session[]
  socialLogins SocialLogin[]
  credits      Balance?
  transactions Transaction[]
  orders       Order[]
  Space        Space[]
}
```

### **Content Table**

```prisma
model Content {
  id               String          @id @default(uuid())
  src              String
  fileName         String
  originalFileName String
  fileId           String
  size             Int
  height           Int
  width            Int
  price            Decimal
  tags             Json?
  faceDetection    FaceDetection[]
  createdAt        DateTime        @default(now())
  updatedAt        DateTime        @updatedAt
  deletedAt        DateTime?
  space            Space?          @relation(fields: [spaceId], references: [id])
  spaceId          String?
  Cart             Cart[]
}
```

### Additional Tables:

- **Order:** Tracks user purchases.
- **Balance:** Manages user top-ups and withdrawals.
- **Transaction:** Logs payment activities.

---

## 6. **Project Roadmap**

### **Phase 1: Core Features**

- User authentication and profiles.
- Content upload and organization.
- Basic search functionality.
- Payment integration.

### **Phase 2: Advanced Features**

- Watermarking and image optimization.
- Dashboard for metrics and analytics.
- Advanced search with AWS Recognition.
- SEO improvements.

### **Phase 3: Scaling and Optimization**

- Improved caching and performance.
- Multi-language support.
- Mobile app integration.

---

## 7. **Milestones**

### **Week 1-2:**

- Finalize database schema and initial setup.
- Implement user authentication.

### **Week 3-4:**

- Develop content upload and organization features.
- Integrate Cloudflare and Midtrans Payment.

### **Week 5-6:**

- Implement basic search functionality.
- Integrate AWS Recognition.
- Deploy MVP version on Vercel.

---

