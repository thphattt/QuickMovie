<div align="center">

# 🎬 QuickMovie

### Modern Movie Ticket Booking Platform

[![React](https://img.shields.io/badge/React-19-61DAFB?style=for-the-badge&logo=react&logoColor=white)](https://react.dev/)
[![Vite](https://img.shields.io/badge/Vite-7-646CFF?style=for-the-badge&logo=vite&logoColor=white)](https://vite.dev/)
[![Express](https://img.shields.io/badge/Express-5-000000?style=for-the-badge&logo=express&logoColor=white)](https://expressjs.com/)
[![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)](https://www.mongodb.com/)
[![TailwindCSS](https://img.shields.io/badge/Tailwind_CSS-4-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white)](https://tailwindcss.com/)
[![Stripe](https://img.shields.io/badge/Stripe-008CDD?style=for-the-badge&logo=stripe&logoColor=white)](https://stripe.com/)
[![Clerk](https://img.shields.io/badge/Clerk-6C47FF?style=for-the-badge&logo=clerk&logoColor=white)](https://clerk.com/)
[![Vercel](https://img.shields.io/badge/Vercel-000000?style=for-the-badge&logo=vercel&logoColor=white)](https://vercel.com/)

**QuickMovie** là nền tảng đặt vé xem phim fullstack hiện đại — tích hợp thanh toán trực tuyến,
xác thực người dùng, quản lý suất chiếu, chọn ghế realtime, và thông báo email tự động.

[Tính năng](#-tính-năng) •
[Kiến trúc](#-kiến-trúc-hệ-thống) •
[Cài đặt](#-cài-đặt--chạy-local) •
[API](#-api-endpoints) •
[Triển khai](#-triển-khai)

</div>

---

## ✨ Tính năng

### 🎥 Người dùng
- **Duyệt phim** — Khám phá thư viện phim với poster, trailer, và thông tin chi tiết từ TMDB
- **Chọn ghế trực quan** — Giao diện sơ đồ ghế tương tác, hiển thị trạng thái realtime
- **Thanh toán Stripe** — Thanh toán an toàn qua Stripe Checkout, tự động huỷ vé nếu không thanh toán trong 10 phút
- **Quản lý booking** — Xem lịch sử đặt vé cá nhân
- **Yêu thích** — Lưu phim yêu thích để xem lại sau
- **Email thông báo** — Nhận xác nhận đặt vé và nhắc nhở trước giờ chiếu (8h)

### 🛠️ Quản trị viên (Admin Panel)
- **Dashboard** — Tổng quan thống kê hệ thống
- **Quản lý suất chiếu** — Thêm / xoá suất chiếu, tìm phim từ TMDB API
- **Quản lý booking** — Xem danh sách tất cả booking

### ⚡ Kỹ thuật nổi bật
- **Background Jobs** — Inngest xử lý các tác vụ nền (huỷ vé, gửi email, nhắc nhở)
- **User Sync** — Đồng bộ user tự động giữa Clerk và MongoDB qua webhook
- **Responsive Design** — Tối ưu trải nghiệm trên mọi kích thước màn hình

---

## 🏗 Kiến trúc hệ thống

```
┌─────────────────────────────────────────────────────────────────┐
│                          CLIENT                                 │
│  React 19 + Vite 7 + TailwindCSS 4 + React Router 7            │
│  ┌──────────┐ ┌───────────┐ ┌──────────┐ ┌──────────────────┐  │
│  │  Home    │ │  Movies   │ │ Booking  │ │   Admin Panel    │  │
│  │  Page    │ │  Browser  │ │ + Seats  │ │  Dashboard/CRUD  │  │
│  └──────────┘ └───────────┘ └──────────┘ └──────────────────┘  │
└────────────────────────┬────────────────────────────────────────┘
                         │ REST API (Axios)
                         ▼
┌─────────────────────────────────────────────────────────────────┐
│                          SERVER                                  │
│  Express 5 + Node.js (ES Modules)                                │
│  ┌─────────────┐  ┌─────────────┐  ┌────────────────────────┐  │
│  │  Clerk Auth │  │  Stripe     │  │  Inngest (Background)  │  │
│  │  Middleware │  │  Webhooks   │  │  • Cancel unpaid       │  │
│  └─────────────┘  └─────────────┘  │  • Email confirm       │  │
│                                     │  • Show reminders      │  │
│                                     │  • User sync           │  │
│                                     │  • New show notify     │  │
│                                     └────────────────────────┘  │
└────────────────────────┬────────────────────────────────────────┘
                         │
            ┌────────────┼────────────┐
            ▼            ▼            ▼
      ┌──────────┐ ┌──────────┐ ┌──────────┐
      │ MongoDB  │ │  TMDB    │ │  SMTP    │
      │ Atlas    │ │  API     │ │  Email   │
      └──────────┘ └──────────┘ └──────────┘
```

---

## 🛠 Tech Stack

| Layer          | Công nghệ                                                              |
| -------------- | ----------------------------------------------------------------------- |
| **Frontend**   | React 19, Vite 7, TailwindCSS 4, React Router 7, Lucide React          |
| **Backend**    | Express 5, Node.js (ESM), Mongoose 9                                    |
| **Database**   | MongoDB Atlas                                                           |
| **Auth**       | Clerk (React + Express SDK)                                             |
| **Payment**    | Stripe (Checkout + Webhooks)                                            |
| **Background** | Inngest (Event-driven functions, cron jobs)                             |
| **Email**      | Nodemailer (SMTP)                                                       |
| **Movie Data** | TMDB API                                                                |
| **Deployment** | Vercel (Frontend + Backend)                                             |

---

## 📁 Cấu trúc thư mục

```
PNmovie/
├── frontend/                   # React SPA
│   ├── src/
│   │   ├── assets/             # Icons, logos, images
│   │   ├── components/         # Reusable UI components
│   │   │   ├── Navbar.jsx
│   │   │   ├── Footer.jsx
│   │   │   ├── HeroSection.jsx
│   │   │   ├── FeaturedSection.jsx
│   │   │   ├── TrailersSection.jsx
│   │   │   ├── MovieCard.jsx
│   │   │   ├── DateSelect.jsx
│   │   │   ├── Loading.jsx
│   │   │   └── admin/          # Admin-specific components
│   │   ├── pages/              # Route pages
│   │   │   ├── Home.jsx
│   │   │   ├── Movies.jsx
│   │   │   ├── MovieDetails.jsx
│   │   │   ├── SeatLayout.jsx
│   │   │   ├── MyBookings.jsx
│   │   │   ├── Favorite.jsx
│   │   │   └── admin/          # Admin pages
│   │   │       ├── Layout.jsx
│   │   │       ├── Dashboard.jsx
│   │   │       ├── AddShows.jsx
│   │   │       ├── ListShows.jsx
│   │   │       └── ListBookings.jsx
│   │   ├── context/            # React Context (global state)
│   │   ├── lib/                # Utility functions
│   │   ├── App.jsx             # Router configuration
│   │   └── main.jsx            # Entry point
│   ├── index.html
│   ├── tailwind.config.js
│   ├── vite.config.js
│   └── vercel.json
│
├── backend/                    # Express REST API
│   ├── configs/                # DB connection, Nodemailer, etc.
│   ├── controllers/            # Route handlers
│   │   ├── showController.js
│   │   ├── bookingController.js
│   │   ├── adminController.js
│   │   ├── userController.js
│   │   └── stripeWebhooks.js
│   ├── middleware/              # Auth & validation middleware
│   ├── models/                  # Mongoose schemas
│   │   ├── User.js
│   │   ├── Movie.js
│   │   ├── Show.js
│   │   └── Booking.js
│   ├── routes/                  # Express routers
│   ├── inngest/                 # Background job functions
│   │   └── index.js
│   ├── server.js                # App entry point
│   └── vercel.json
│
└── .gitignore
```

---

## 🚀 Cài đặt & Chạy Local

### Yêu cầu

- **Node.js** ≥ 18
- **npm** ≥ 9
- Tài khoản [MongoDB Atlas](https://www.mongodb.com/atlas), [Clerk](https://clerk.com/), [Stripe](https://stripe.com/), [TMDB](https://developer.themoviedb.org/), và SMTP email

### 1. Clone repository

```bash
git clone https://github.com/thphattt/QuickMovie.git
cd QuickMovie
```

### 2. Cài đặt dependencies

```bash
# Backend
cd backend
npm install

# Frontend
cd ../frontend
npm install
```

### 3. Cấu hình biến môi trường

Tạo file `.env` trong cả thư mục `backend/` và `frontend/` dựa trên file `.env.example`:

<details>
<summary><strong>📄 Backend <code>.env</code></strong></summary>

```env
# Database
MONGODB_URI=your_mongodb_connection_string

# Authentication (Clerk)
CLERK_PUBLISHABLE_KEY=your_clerk_publishable_key
CLERK_SECRET_KEY=your_clerk_secret_key

# Inngest
INNGEST_EVENT_KEY=your_inngest_event_key
INNGEST_SIGNING_KEY=your_inngest_signing_key

# TMDB API
TMDB_API_KEY=your_tmdb_api_key

# Stripe
STRIPE_PUBLISHABLE_KEY=your_stripe_publishable_key
STRIPE_SECRET_KEY=your_stripe_secret_key
STRIPE_WEBHOOK_SECRET=your_stripe_webhook_secret

# SMTP (Email)
SENDER_EMAIL=your_sender_email
SMTP_USER=your_smtp_user
SMTP_PASS=your_smtp_pass
```

</details>

<details>
<summary><strong>📄 Frontend <code>.env</code></strong></summary>

```env
VITE_CURRENCY=$
VITE_CLERK_PUBLISHABLE_KEY=your_clerk_publishable_key
VITE_BASE_URL=http://localhost:3000
VITE_TMDB_IMAGE_BASE_URL=https://image.tmdb.org/t/p/original
```

</details>

### 4. Chạy ứng dụng

```bash
# Terminal 1 — Backend (port 3000)
cd backend
npm run dev

# Terminal 2 — Frontend (port 5173)
cd frontend
npm run dev
```

Mở trình duyệt tại **http://localhost:5173**

---

## 📡 API Endpoints

### Shows

| Method | Endpoint                   | Mô tả                               | Auth  |
| ------ | -------------------------- | ------------------------------------ | ----- |
| GET    | `/api/show/shows`          | Lấy danh sách tất cả suất chiếu     | No    |
| GET    | `/api/show/shows/:id`      | Chi tiết suất chiếu theo ID          | No    |
| GET    | `/api/show/movie/:movieId` | Lấy suất chiếu theo phim             | No    |

### Bookings

| Method | Endpoint                  | Mô tả                               | Auth  |
| ------ | ------------------------- | ------------------------------------ | ----- |
| POST   | `/api/booking/create`     | Tạo booking + Stripe payment link    | ✅    |
| GET    | `/api/booking/my-bookings`| Lấy danh sách booking của user       | ✅    |

### Admin

| Method | Endpoint                  | Mô tả                               | Auth  |
| ------ | ------------------------- | ------------------------------------ | ----- |
| POST   | `/api/admin/add-show`     | Thêm suất chiếu mới                 | Admin |
| DELETE | `/api/admin/delete-show`  | Xoá suất chiếu                       | Admin |
| GET    | `/api/admin/bookings`     | Lấy tất cả bookings                 | Admin |

### User

| Method | Endpoint                  | Mô tả                               | Auth  |
| ------ | ------------------------- | ------------------------------------ | ----- |
| GET    | `/api/user/data`          | Lấy thông tin user hiện tại          | ✅    |
| POST   | `/api/user/favorite`      | Thêm/xoá phim yêu thích             | ✅    |

### Webhooks

| Method | Endpoint                  | Mô tả                               |
| ------ | ------------------------- | ------------------------------------ |
| POST   | `/api/stripe`             | Stripe payment webhooks              |
| POST   | `/api/inngest`            | Inngest event handling               |

---

## ☁️ Triển khai

Dự án được cấu hình sẵn để triển khai trên **Vercel** (cả frontend và backend).

### Frontend

```bash
cd frontend
npx vercel --prod
```

### Backend

```bash
cd backend
npx vercel --prod
```

> **Lưu ý:** Cần cấu hình biến môi trường trên Vercel Dashboard trước khi deploy.

---

## 📋 Background Jobs (Inngest)

| Function                          | Trigger                      | Mô tả                                      |
| --------------------------------- | ---------------------------- | ------------------------------------------- |
| `sync-user-from-clerk`            | `clerk/user.created`         | Đồng bộ user mới từ Clerk → MongoDB        |
| `delete-user-with-clerk`          | `clerk/user.deleted`         | Xoá user khi bị xoá trên Clerk             |
| `update-user-from-clerk`          | `clerk/user.updated`         | Cập nhật thông tin user                     |
| `release-seats-delete-booking`    | `app/checkpayment`           | Huỷ booking & trả ghế nếu chưa thanh toán sau 10p |
| `send-booking-confirmation-email` | `app/show.booked`            | Gửi email xác nhận booking                  |
| `send-show-reminders`             | Cron `0 */8 * * *`           | Nhắc nhở trước suất chiếu 8 giờ             |
| `send-new-show-notifications`     | `app/show.added`             | Thông báo cho tất cả user khi có suất mới   |

---

## 🤝 Đóng góp

Mọi đóng góp đều được chào đón! Hãy tạo **Issue** hoặc **Pull Request** nếu bạn muốn cải thiện dự án.

1. Fork repository
2. Tạo branch (`git checkout -b feature/amazing-feature`)
3. Commit thay đổi (`git commit -m 'Add amazing feature'`)
4. Push lên branch (`git push origin feature/amazing-feature`)
5. Tạo Pull Request

---

## 📄 License

Dự án này được phân phối dưới giấy phép [MIT](LICENSE).

---

<div align="center">

**Được xây dựng với ❤️ bởi [thphattt](https://github.com/thphattt)**

</div>
