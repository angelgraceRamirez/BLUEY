# HydroFlow - Complete VS Code Setup Guide

> Last Updated: 2026-04-22

---

## TABLE OF CONTENTS
1. [Environment Variables Setup](#1-environment-variables-setup)
2. [Supabase RLS Policies](#2-supabase-rls-policies)
3. [Project Dependencies](#3-project-dependencies)
4. [File Structure & Code](#4-file-structure--code)

---

## 1. ENVIRONMENT VARIABLES SETUP

### Where to Put Them: `.env.local` File

In your VS Code project root, create a file named `.env.local`:

```
.env.local
```

### Copy-Paste These Variables

Replace the values with your own:

```
NEXT_PUBLIC_SUPABASE_URL=https://cuparnvpiaxcuswbstla.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImN1cGFybnZwaWF4Y3Vzd2JzdGxhIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NzY3OTI5MTksImV4cCI6MjA5MjM2ODkxOX0.4BrRAVK2XLprpYm20DBNoys9y4wFD39vP7Ol96n5GNE
SUPABASE_SERVICE_ROLE_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImN1cGFybnZwaWF4Y3Vzd2JzdGxhIiwicm9sZSI6InNlcnZpY2Vfcm9sZSIsImlhdCI6MTc3Njc5MjkxOSwiZXhwIjoyMDkyMzY4OTE5fQ.9pPAo1xgMBIVTdeV2Ixu9n6PhJeU2jccV31r1wySGlI
```

### What Each API Key Does

| Variable | Purpose | Where Used |
|----------|---------|-----------|
| `NEXT_PUBLIC_SUPABASE_URL` | Your Supabase project URL | Frontend & Backend |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | Anonymous key for browser access | Frontend (Login/Signup) |
| `SUPABASE_SERVICE_ROLE_KEY` | Secret key for server-side operations | Backend only (API routes) |

**IMPORTANT:**
- `NEXT_PUBLIC_*` = Safe to expose in browser (frontend)
- No prefix = Keep secret (backend only, never share)
- `.env.local` is ignored by git (it's in `.gitignore`)

---

## 2. SUPABASE RLS POLICIES

### What is RLS?

RLS (Row Level Security) = Only logged-in users can see/edit THEIR OWN data.

### How to Set Up RLS Policies

**Go to Supabase Dashboard:**
1. Click **Database** → **Tables**
2. Select a table (e.g., `profiles`)
3. Click **Auth** tab (top right)
4. Toggle **Enable RLS** to ON
5. Click **New policy** → **Create policy**

---

### POLICY 1: `profiles` Table

**For Reading (SELECT):**

1. Click **New policy**
2. Click **Create policy from template**
3. Select **"Enable read access for users based on their UID"**
4. Keep defaults, click **Review** → **Save**

**For Writing (UPDATE/DELETE):**

1. Click **New policy** again
2. Select **"Enable update for users based on their UID"**
3. Keep defaults, click **Review** → **Save**

**Manual SQL (If you prefer to paste):**

```sql
-- Allow users to read their own profile
CREATE POLICY "Users can read own profile"
ON profiles FOR SELECT
USING (auth.uid() = id);

-- Allow users to update their own profile
CREATE POLICY "Users can update own profile"
ON profiles FOR UPDATE
USING (auth.uid() = id)
WITH CHECK (auth.uid() = id);

-- Allow users to insert their own profile
CREATE POLICY "Users can insert own profile"
ON profiles FOR INSERT
WITH CHECK (auth.uid() = id);

-- Allow users to delete their own profile
CREATE POLICY "Users can delete own profile"
ON profiles FOR DELETE
USING (auth.uid() = id);
```

---

### POLICY 2: `sensor_readings` Table

**Using Templates (Recommended):**

1. Click **New policy**
2. Select **"Enable read access for users based on their UID"** (choose `user_id` field)
3. Select **"Enable insert for authenticated users"**
4. Select **"Enable update for users based on their UID"** (choose `user_id` field)

**Or use SQL:**

```sql
-- Allow users to read their own sensor readings
CREATE POLICY "Users can read own sensor readings"
ON sensor_readings FOR SELECT
USING (auth.uid() = user_id);

-- Allow users to insert sensor readings
CREATE POLICY "Users can insert sensor readings"
ON sensor_readings FOR INSERT
WITH CHECK (auth.uid() = user_id);

-- Allow users to update their own sensor readings
CREATE POLICY "Users can update own sensor readings"
ON sensor_readings FOR UPDATE
USING (auth.uid() = user_id)
WITH CHECK (auth.uid() = user_id);

-- Allow users to delete their own sensor readings
CREATE POLICY "Users can delete own sensor readings"
ON sensor_readings FOR DELETE
USING (auth.uid() = user_id);
```

---

### POLICY 3: `activity_logs` Table

**Using Templates:**

1. Click **New policy**
2. Select **"Enable read access for users based on their UID"** (choose `user_id` field)
3. Select **"Enable insert for authenticated users"**
4. Select **"Enable update for users based on their UID"** (choose `user_id` field)

**Or use SQL:**

```sql
-- Allow users to read their own activity logs
CREATE POLICY "Users can read own activity logs"
ON activity_logs FOR SELECT
USING (auth.uid() = user_id);

-- Allow users to insert activity logs
CREATE POLICY "Users can insert activity logs"
ON activity_logs FOR INSERT
WITH CHECK (auth.uid() = user_id);

-- Allow users to update their own activity logs
CREATE POLICY "Users can update own activity logs"
ON activity_logs FOR UPDATE
USING (auth.uid() = user_id)
WITH CHECK (auth.uid() = user_id);

-- Allow users to delete their own activity logs
CREATE POLICY "Users can delete own activity logs"
ON activity_logs FOR DELETE
USING (auth.uid() = user_id);
```

---

### How to Apply SQL Policies

1. Go to **SQL Editor** in Supabase
2. Click **New query**
3. Paste the SQL code above
4. Click **Run**
5. Done! Policies are applied.

---

## 3. PROJECT DEPENDENCIES

### Install Everything:

```bash
npm install
```

### package.json Dependencies

```json
{
  "dependencies": {
    "@supabase/ssr": "^0.10.2",
    "@supabase/supabase-js": "^2.39.0",
    "@tailwindcss/postcss": "^4.2.3",
    "@types/node": "^25.6.0",
    "@types/react": "^19.2.14",
    "autoprefixer": "^10.5.0",
    "next": "^16.2.4",
    "postcss": "^8.5.10",
    "react": "^19.2.5",
    "react-dom": "^19.2.5",
    "tailwindcss": "^4.2.3",
    "typescript": "^6.0.3"
  }
}
```

### Run the Project:

```bash
npm run dev
```

Then go to: `http://localhost:3000`

---

## 4. FILE STRUCTURE & CODE

### Project Structure

```
hydroflow/
├── app/
│   ├── page.tsx (Home page)
│   ├── layout.tsx (Root layout)
│   ├── globals.css (Styling)
│   ├── dashboard/
│   │   └── page.tsx (Dashboard page)
│   ├── auth/
│   │   ├── login/page.tsx
│   │   ├── sign-up/page.tsx
│   │   └── callback/route.ts
│   └── api/
│       ├── sensors/route.ts
│       ├── profile/route.ts
│       ├── activity/route.ts
│       └── arduino/route.ts
├── components/
│   ├── dashboard-client.tsx
│   ├── sensor-card.tsx
│   ├── hydration-card.tsx
│   ├── activity-panel.tsx
│   ├── profile-panel.tsx
│   ├── header.tsx
│   └── connection-status.tsx
├── lib/
│   └── supabase/
│       ├── client.ts (Browser client)
│       └── server.ts (Server client)
├── .env.local (Your API keys - DO NOT SHARE)
├── package.json
├── tsconfig.json
└── next.config.js
```

### Key Files to Copy

#### 1. `.env.local` (Environment Variables)
```
NEXT_PUBLIC_SUPABASE_URL=https://cuparnvpiaxcuswbstla.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImN1cGFybnZwaWF4Y3Vzd2JzdGxhIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NzY3OTI5MTksImV4cCI6MjA5MjM2ODkxOX0.4BrRAVK2XLprpYm20DBNoys9y4wFD39vP7Ol96n5GNE
SUPABASE_SERVICE_ROLE_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImN1cGFybnZwaWF4Y3Vzd2JzdGxhIiwicm9sZSI6InNlcnZpY2Vfcm9sZSIsImlhdCI6MTc3Njc5MjkxOSwiZXhwIjoyMDkyMzY4OTE5fQ.9pPAo1xgMBIVTdeV2Ixu9n6PhJeU2jccV31r1wySGlI
```

#### 2. `lib/supabase/client.ts` (Browser Client)
```typescript
import { createBrowserClient } from '@supabase/ssr'

export function createClient() {
  return createBrowserClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!
  )
}
```

#### 3. `lib/supabase/server.ts` (Server Client)
```typescript
import { createServerClient } from '@supabase/ssr'
import { cookies } from 'next/headers'

export async function createClient() {
  const cookieStore = await cookies()

  return createServerClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
    {
      cookies: {
        getAll() {
          return cookieStore.getAll()
        },
        setAll(cookiesToSet) {
          try {
            cookiesToSet.forEach(({ name, value, options }) =>
              cookieStore.set(name, value, options)
            )
          } catch {
            // Handle cookie setting errors
          }
        },
      },
    }
  )
}
```

---

## TROUBLESHOOTING

### Issue: "Environment variables not set"
**Solution:** Make sure `.env.local` exists in project root with correct keys

### Issue: "RLS policy denied"
**Solution:** Ensure RLS policies are enabled on the table and policies are correctly created

### Issue: "Cannot read property 'user' of null"
**Solution:** User is not authenticated - redirect to login page needed

### Issue: "Supabase connection failed"
**Solution:** Check if `NEXT_PUBLIC_SUPABASE_URL` is correct (should be `https://cuparnvpiaxcuswbstla.supabase.co`)

---

## SUMMARY

✅ **Done:**
1. Created `.env.local` with API keys
2. Set up RLS policies for all 3 tables
3. All dependencies installed
4. Project ready to run with `npm run dev`

You're all set! Your HydroFlow app is ready to use with your own Supabase project.
