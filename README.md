# AssignFlow 🎓

> **Modern Assignment Management System for College Students**  
> Built with Next.js App Router, TypeScript, Tailwind CSS, and Supabase PostgreSQL with Row Level Security (RLS).

---

## 🌟 Key Features

### 1. 🔐 Robust Authentication with `@supabase/ssr`
- **Email & Password Authentication**: Full signup, login, and secure sign-out.
- **Session Cookie Persistence**: Secure cookie handling via `@supabase/ssr` with middleware session refresh and route protection.
- **Protected Routes**: `/dashboard` and `/dashboard/*` are strictly protected; unauthenticated students are redirected to `/login`.

### 2. 📊 Academic Productivity Dashboard
- **Real-time Statistics**: Calculated directly from actual database records:
  - Total assignments
  - Pending assignments
  - In-Progress assignments
  - Completed assignments
  - Overdue assignments (accurately determined by `due_date < now && status != 'Completed'`)
  - Upcoming deadlines within the next 7 days
- **Academic Term Progress**: Visual progress bar tracking completion rate across the semester.
- **Deadline Radar**: Highlights urgent deadlines with dynamic urgency badges (*Overdue*, *Due today*, *Due soon*).

### 3. 📝 Complete Assignment Management (Full CRUD)
- **Create Assignment**: Title, course/subject, due date & time picker, priority level, initial status, and rich markdown/checklist notes.
- **View Assignment Details**: Dedicated modal displaying full description, course tags, formatted timestamps, and time remaining until deadline.
- **Quick Status Toggle**: 1-click toggle to mark completed or reopen directly from cards or table rows.
- **Edit Assignment**: Update any assignment detail with prefilled values.
- **Delete Confirmation**: Safe deletion with explicit modal confirmation to prevent accidental loss.

### 4. 🔍 Advanced Search, Filtering, and Sorting
- **Instant Search**: Search across assignment titles, subjects, and descriptions simultaneously.
- **Status Filter**: Filter by *All*, *Pending*, *In Progress*, *Completed*, or *Overdue Only*.
- **Priority Filter**: Filter by *High*, *Medium*, or *Low*.
- **Subject Filter**: Dynamic filtering by academic subjects (*Computer Science*, *Mathematics*, *Physics*, etc.).
- **Flexible Sorting**: Sort by deadline (earliest/latest), creation date (newest/oldest), priority level, or title (A–Z).
- **View Switcher**: Seamlessly toggle between modern **Grid Cards** and compact **Table Rows**.

### 5. 🛡️ Database & Security Architecture
- **Supabase PostgreSQL**: Schema with UUID primary keys and foreign key constraints on `auth.users(id)`.
- **Row Level Security (RLS)**: Fine-grained PostgreSQL policies ensure each student can strictly access only their own assignments.
- **Automatic Timestamp Triggers**: Automated trigger updates `updated_at` whenever a record changes.

---

## 🚀 Tech Stack

| Layer | Technology |
|---|---|
| **Framework** | Next.js (App Router, Turbopack) |
| **Language** | TypeScript |
| **Styling** | Tailwind CSS v4 |
| **Database & Auth** | Supabase (PostgreSQL + Auth + RLS) |
| **Auth Adapter** | `@supabase/ssr` |
| **Icons** | Lucide React |
| **Deployment** | Vercel Ready |

---

## 🛠️ Getting Started

### 1. Prerequisites
- **Node.js** v18+ (tested on Node.js v24)
- **npm**, **pnpm**, or **yarn**
- A free **[Supabase](https://supabase.com)** project

### 2. Installation

```bash
# Clone the repository
git clone <your-repo-url>
cd port

# Install dependencies
npm install
```

### 3. Configure Environment Variables

Create `.env.local` based on `.env.example`:

```bash
cp .env.example .env.local
```

Fill in your actual Supabase credentials:

```env
NEXT_PUBLIC_SUPABASE_URL=https://your-project-id.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_publishable_or_anon_key
```

> **Note:** The variable names `NEXT_PUBLIC_SUPABASE_URL` and `NEXT_PUBLIC_SUPABASE_ANON_KEY` are strictly required throughout the application.

### 4. Initialize Database Schema & RLS

1. Go to your **[Supabase Dashboard](https://app.supabase.com)**.
2. Navigate to **SQL Editor** -> **New query**.
3. Copy the entire contents of [`supabase/schema.sql`](supabase/schema.sql) and click **Run**.

This script will:
- Create the `public.assignments` table.
- Create performance indexes on `user_id`, `due_date`, `status`, and `priority`.
- Create the automated `updated_at` trigger.
- Enable Row Level Security (RLS).
- Add SELECT, INSERT, UPDATE, and DELETE policies scoped to `auth.uid() = user_id`.

### 5. Run Development Server

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

---

## 🧪 Production Build & Testing

```bash
# Build the production bundle
npm run build

# Start production server
npm run start
```

---

## 🚢 Deployment to Vercel

1. Push your repository to **GitHub**.
2. Go to **[Vercel](https://vercel.com)** and click **Add New Project**.
3. Import your GitHub repository.
4. Under **Environment Variables**, add:
   - `NEXT_PUBLIC_SUPABASE_URL`
   - `NEXT_PUBLIC_SUPABASE_ANON_KEY`
5. Click **Deploy**. Vercel will build and launch AssignFlow globally!

---

## 📁 Project Structure

```
├── public/
├── src/
│   ├── app/
│   │   ├── (auth)/
│   │   │   ├── login/page.tsx         # Student login
│   │   │   └── signup/page.tsx        # Student registration
│   │   ├── auth/
│   │   │   ├── callback/route.ts      # Auth token exchange
│   │   │   └── signout/route.ts       # Secure logout
│   │   ├── dashboard/
│   │   │   ├── layout.tsx             # Server auth layout
│   │   │   ├── page.tsx               # Main stats & overview dashboard
│   │   │   └── assignments/page.tsx   # Dedicated assignment manager
│   │   ├── layout.tsx                 # Root layout & typography
│   │   └── page.tsx                   # Landing page
│   ├── components/
│   │   ├── assignments/               # Card, Row, Filters, Stats, Modals
│   │   ├── dashboard/                 # Header, Sidebar, UpcomingDeadlines
│   │   └── ui/                        # Badge, Button, Modal, Toast
│   ├── lib/
│   │   ├── supabase/                  # client.ts, server.ts, middleware.ts
│   │   ├── types.ts                   # TypeScript interfaces & types
│   │   ├── utils.ts                   # Date math, stats calculation, styling
│   │   └── demoData.ts                # Initial preview semester data
│   └── middleware.ts                  # Next.js session refresh & protection
└── supabase/
    └── schema.sql                     # Full PostgreSQL table & RLS policies
```

---

## 📄 License
MIT © AssignFlow
