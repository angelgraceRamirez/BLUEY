# HYDROFLOW - QUICK START FOR VS CODE

**TL;DR - Do this in order:**

---

## 1️⃣ SETUP SUPABASE (5 mins)

### Create Tables - Go to SQL Editor in Supabase Dashboard:

```sql
-- Create profiles table
CREATE TABLE profiles (
  id uuid PRIMARY KEY REFERENCES auth.users(id) ON DELETE CASCADE,
  first_name text,
  last_name text,
  email text,
  age integer,
  weight decimal,
  height decimal,
  sport_type text,
  team_name text,
  avatar_url text,
  updated_at timestamp DEFAULT now()
);

-- Create sensor_readings table
CREATE TABLE sensor_readings (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id uuid NOT NULL REFERENCES auth.users(id) ON DELETE CASCADE,
  heart_rate integer,
  body_temp decimal,
  flow_rate decimal,
  water_consumed integer DEFAULT 0,
  device_connected boolean DEFAULT false,
  recorded_at timestamp DEFAULT now()
);

-- Create activity_logs table
CREATE TABLE activity_logs (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id uuid NOT NULL REFERENCES auth.users(id) ON DELETE CASCADE,
  activity_type text NOT NULL,
  flow_rate_avg decimal,
  duration_minutes integer,
  total_water_ml integer,
  notes text,
  created_at timestamp DEFAULT now()
);

-- Create indexes
CREATE INDEX idx_sensor_readings_user_id ON sensor_readings(user_id);
CREATE INDEX idx_sensor_readings_recorded_at ON sensor_readings(recorded_at);
CREATE INDEX idx_activity_logs_user_id ON activity_logs(user_id);
CREATE INDEX idx_activity_logs_created_at ON activity_logs(created_at);
```

**Run this in SQL Editor, then come back here.**

---

### Enable RLS (Row Level Security) - Go to Database → Tables:

**For EACH table (profiles, sensor_readings, activity_logs):**

1. Click table name
2. Click **Auth** tab
3. Toggle RLS **ON**
4. Click **New policy**
5. Select: **"Enable read access for users based on their UID"** → Save
6. Click **New policy**
7. Select: **"Enable insert for authenticated users"** → Save
8. Click **New policy**
9. Select: **"Enable update for users based on their UID"** → Save
10. Click **New policy**
11. Select: **"Enable delete for users based on their UID"** → Save

**For sensor_readings and activity_logs, use `user_id` as the column.**
**For profiles, use `id` as the column.**

Done! Now to VS Code.

---

## 2️⃣ SETUP VS CODE (5 mins)

### Step 1: Create `.env.local` file in project root

**File location:** `.env.local` (at the very top level, same folder as `package.json`)

**Copy & paste this exactly:**

```
NEXT_PUBLIC_SUPABASE_URL=https://cuparnvpiaxcuswbstla.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImN1cGFybnZwaWF4Y3Vzd2JzdGxhIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NzY3OTI5MTksImV4cCI6MjA5MjM2ODkxOX0.4BrRAVK2XLprpYm20DBNoys9y4wFD39vP7Ol96n5GNE
SUPABASE_SERVICE_ROLE_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImN1cGFybnZwaWF4Y3Vzd2JzdGxhIiwicm9sZSI6InNlcnZpY2Vfcm9sZSIsImlhdCI6MTc3Njc5MjkxOSwiZXhwIjoyMDkyMzY4OTE5fQ.9pPAo1xgMBIVTdeV2Ixu9n6PhJeU2jccV31r1wySGlI
```

**SAVE THIS FILE!**

### Step 2: Create `lib/supabase/client.ts`

**File location:** `lib/supabase/client.ts`

```typescript
import { createBrowserClient } from '@supabase/ssr'

export function createClient() {
  return createBrowserClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!
  )
}
```

### Step 3: Create `lib/supabase/server.ts`

**File location:** `lib/supabase/server.ts`

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
            // Handle errors
          }
        },
      },
    }
  )
}
```

---

## 3️⃣ INSTALL & RUN (2 mins)

### Open Terminal in VS Code:

```bash
npm install
```

Wait for installation to complete, then:

```bash
npm run dev
```

You'll see:
```
▲ Next.js 16.2.4
- Local: http://localhost:3000
```

---

## 4️⃣ TEST IT

Open browser: **http://localhost:3000**

You should see:
- ✅ HydroFlow homepage
- ✅ "Sign In" button works
- ✅ "Get Started" button works
- ✅ Can create account
- ✅ Can see dashboard

---

## 📝 API KEYS EXPLAINED

| Variable | What It Is | Safe to Share? | Used By |
|----------|-----------|----------------|---------|
| `NEXT_PUBLIC_SUPABASE_URL` | Your project URL | ✅ YES | Frontend & Backend |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | Anonymous key for login | ✅ YES | Frontend (Login/Signup) |
| `SUPABASE_SERVICE_ROLE_KEY` | Secret admin key | ❌ NO | Backend only (API routes) |

**IMPORTANT:**
- Never share `SUPABASE_SERVICE_ROLE_KEY` with anyone
- Never push `.env.local` to GitHub (it's in `.gitignore`)
- The `NEXT_PUBLIC_*` keys are visible in browser (that's OK, they're limited)

---

## 🚨 TROUBLESHOOTING

### "Environment variables not set"
**Fix:** Check `.env.local` exists in project root with correct keys

### "RLS policy denied"
**Fix:** Go to Supabase dashboard → Database → Tables → Check RLS is enabled

### "Cannot read property of null"
**Fix:** You're not logged in - try signing up first

### "Port 3000 is already in use"
**Fix:** Run this in terminal:
```bash
npm run dev -- -p 3001
```

---

## 📚 FULL GUIDES

- **RLS Setup Guide:** Read `RLS_POLICIES_COMPLETE.md`
- **VS Code Setup:** Read `VSCODE_SETUP_GUIDE.md`
- **Code Export:** Read `COMPLETE_CODE_EXPORT.txt`

---

## ✅ YOU'RE DONE!

Your HydroFlow app is now:
- ✅ Connected to your Supabase database
- ✅ Secured with RLS policies
- ✅ Ready to use locally
- ✅ Ready to deploy to production

Start the app with: `npm run dev`

Enjoy! 🚀
