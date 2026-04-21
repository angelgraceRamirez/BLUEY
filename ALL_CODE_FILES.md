# HYDROFLOW - ALL CODE FILES FOR VS CODE

Copy and paste each file into the corresponding location in your VS Code project.

---

## 📂 FILE STRUCTURE
```
hydroflow/
├── .env.local (CREATE THIS - see below)
├── lib/
│   └── supabase/
│       ├── client.ts (CREATE THIS)
│       └── server.ts (CREATE THIS)
├── package.json (ALREADY EXISTS - no changes needed)
├── app/
│   ├── page.tsx (ALREADY EXISTS)
│   ├── layout.tsx (ALREADY EXISTS)
│   ├── globals.css (ALREADY EXISTS)
│   └── ... (other files exist)
└── components/
    └── ... (all components exist)
```

---

## 🔑 FILE 1: .env.local
**Location:** `.env.local` (in project root, same level as package.json)

**IMPORTANT:** This file is NOT in the repo - YOU MUST CREATE IT

```
NEXT_PUBLIC_SUPABASE_URL=https://cuparnvpiaxcuswbstla.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImN1cGFybnZwaWF4Y3Vzd2JzdGxhIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NzY3OTI5MTksImV4cCI6MjA5MjM2ODkxOX0.4BrRAVK2XLprpYm20DBNoys9y4wFD39vP7Ol96n5GNE
SUPABASE_SERVICE_ROLE_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImN1cGFybnZwaWF4Y3Vzd2JzdGxhIiwicm9sZSI6InNlcnZpY2Vfcm9sZSIsImlhdCI6MTc3Njc5MjkxOSwiZXhwIjoyMDkyMzY4OTE5fQ.9pPAo1xgMBIVTdeV2Ixu9n6PhJeU2jccV31r1wySGlI
```

---

## 🔌 FILE 2: lib/supabase/client.ts
**Location:** `lib/supabase/client.ts`

```typescript
import { createBrowserClient } from '@supabase/ssr'

export function createClient() {
  return createBrowserClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!
  )
}
```

---

## 🔌 FILE 3: lib/supabase/server.ts
**Location:** `lib/supabase/server.ts`

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

## ✅ ALL OTHER FILES ALREADY EXIST

The following files are already in the project and do NOT need to be created:

### App Files:
- `app/page.tsx` ✅
- `app/layout.tsx` ✅
- `app/globals.css` ✅
- `app/dashboard/page.tsx` ✅
- `app/auth/login/page.tsx` ✅
- `app/auth/sign-up/page.tsx` ✅
- `app/auth/callback/route.ts` ✅
- `app/auth/error/page.tsx` ✅
- `app/auth/sign-up-success/page.tsx` ✅

### API Routes:
- `app/api/sensors/route.ts` ✅
- `app/api/profile/route.ts` ✅
- `app/api/activity/route.ts` ✅
- `app/api/arduino/route.ts` ✅

### Components:
- `components/dashboard-client.tsx` ✅
- `components/sensor-card.tsx` ✅
- `components/hydration-card.tsx` ✅
- `components/activity-panel.tsx` ✅
- `components/profile-panel.tsx` ✅
- `components/header.tsx` ✅
- `components/connection-status.tsx` ✅

### Config Files:
- `package.json` ✅
- `tsconfig.json` ✅
- `next.config.js` ✅

---

## 🚀 QUICK SETUP STEPS

1. **Create `.env.local`** with the API keys above
2. **Create `lib/supabase/client.ts`** with the code above
3. **Create `lib/supabase/server.ts`** with the code above
4. Run: `npm install`
5. Run: `npm run dev`
6. Go to: `http://localhost:3000`

---

## 📋 CHECKLIST

- [ ] Created `.env.local` file
- [ ] Created `lib/supabase/client.ts`
- [ ] Created `lib/supabase/server.ts`
- [ ] Verified all files are in correct locations
- [ ] Ran `npm install`
- [ ] Ran `npm run dev`
- [ ] Opened `http://localhost:3000` in browser
- [ ] Tested Sign Up
- [ ] Tested Login
- [ ] Tested Dashboard

---

## 🆘 PROBLEMS?

### "Cannot find module '@supabase/ssr'"
**Solution:** Run `npm install` to install all dependencies

### ".env.local not found"
**Solution:** Create `.env.local` in the project root (same folder as package.json)

### "Connection refused"
**Solution:** Check if `npm run dev` is running in terminal

### "RLS policy denied"
**Solution:** Set up RLS policies on Supabase (see RLS_POLICIES_COMPLETE.md)

---

## 📚 LEARN MORE

- **Quick Start:** Read `QUICK_START.md`
- **Full RLS Guide:** Read `RLS_POLICIES_COMPLETE.md`
- **VS Code Setup:** Read `VSCODE_SETUP_GUIDE.md`

---

That's it! You're all set to run HydroFlow locally in VS Code. 🎉
