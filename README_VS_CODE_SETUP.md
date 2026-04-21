# 🚀 HYDROFLOW - COMPLETE VS CODE SETUP GUIDE

**Everything you need to run HydroFlow in VS Code with your own Supabase project**

---

## 📖 START HERE - CHOOSE YOUR GUIDE

### ⚡ **Option 1: I'm in a hurry** (5 minutes)
👉 Read: **`QUICK_START.md`**
- Copy-paste SQL for Supabase
- Copy-paste environment variables
- Run 2 commands
- Done!

### 🎯 **Option 2: I want everything explained** (15 minutes)
👉 Read: **`VSCODE_SETUP_GUIDE.md`**
- Complete step-by-step instructions
- What each API key does
- How RLS policies work
- Troubleshooting section

### 🔐 **Option 3: I need RLS policy details** (10 minutes)
👉 Read: **`RLS_POLICIES_COMPLETE.md`**
- How to set up RLS in Supabase
- Both GUI and SQL methods
- Why RLS is important
- Common mistakes to avoid

### 💻 **Option 4: Show me all the code**
👉 Read: **`ALL_CODE_FILES.md`**
- Every file you need to create
- Exact code to copy-paste
- File locations
- Checklist

---

## 🔑 YOUR API KEYS (SAVE THESE!)

**Project ID:** `cuparnvpiaxcuswbstla`

| Key | Value |
|-----|-------|
| `NEXT_PUBLIC_SUPABASE_URL` | `https://cuparnvpiaxcuswbstla.supabase.co` |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImN1cGFybnZwaWF4Y3Vzd2JzdGxhIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NzY3OTI5MTksImV4cCI6MjA5MjM2ODkxOX0.4BrRAVK2XLprpYm20DBNoys9y4wFD39vP7Ol96n5GNE` |
| `SUPABASE_SERVICE_ROLE_KEY` | `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImN1cGFybnZwaWF4Y3Vzd2JzdGxhIiwicm9sZSI6InNlcnZpY2Vfcm9sZSIsImlhdCI6MTc3Njc5MjkxOSwiZXhwIjoyMDkyMzY4OTE5fQ.9pPAo1xgMBIVTdeV2Ixu9n6PhJeU2jccV31r1wySGlI` |

---

## 📋 3-STEP SETUP SUMMARY

### Step 1: Setup Supabase Database (5 min)
1. Go to https://app.supabase.com → Select your project
2. Go to **SQL Editor**
3. Run the SQL script from `QUICK_START.md`
4. Then go to **Database** → **Tables**
5. Enable RLS on each table (see `RLS_POLICIES_COMPLETE.md`)

### Step 2: Create Environment File (2 min)
1. In VS Code, create file: `.env.local`
2. Copy the API keys from section above
3. Save the file

### Step 3: Install & Run (2 min)
```bash
npm install
npm run dev
```

Go to: http://localhost:3000

---

## 📚 FULL DOCUMENTATION

| Document | What It Covers | Read Time |
|----------|---------------|-----------|
| **QUICK_START.md** | Fast setup with copy-paste code | 5 min |
| **VSCODE_SETUP_GUIDE.md** | Complete step-by-step guide | 15 min |
| **RLS_POLICIES_COMPLETE.md** | How to set up Row Level Security | 10 min |
| **ALL_CODE_FILES.md** | All code files with locations | 10 min |
| **COMPLETE_CODE_EXPORT.txt** | Code export reference | 5 min |

---

## 🎯 WHAT YOU'LL NEED TO CREATE

### Files to CREATE:
1. `.env.local` - Your API keys
2. `lib/supabase/client.ts` - Browser client
3. `lib/supabase/server.ts` - Server client

### Everything Else:
✅ All other files already exist in the project!

---

## 🆘 QUICK TROUBLESHOOTING

| Problem | Solution |
|---------|----------|
| "Environment variables not set" | Create `.env.local` file with API keys |
| "Cannot find module '@supabase/ssr'" | Run `npm install` |
| "RLS policy denied" | Set up RLS policies (see RLS_POLICIES_COMPLETE.md) |
| "Connection refused" | Check if `npm run dev` is running |
| "Port 3000 in use" | Run `npm run dev -- -p 3001` |

---

## ✨ FEATURES INCLUDED

✅ User Authentication (Sign up, Login, Logout)
✅ Athlete Profile Management
✅ Real-time Sensor Data (Heart rate, Body temp, Flow rate)
✅ Hydration Tracking Dashboard
✅ Activity Logging
✅ Arduino Sensor Integration Ready
✅ Row Level Security (RLS) for data privacy
✅ Responsive Design
✅ Tailwind CSS Styling

---

## 🔒 SECURITY NOTES

- **NEVER** share `SUPABASE_SERVICE_ROLE_KEY`
- **NEVER** commit `.env.local` to GitHub
- `NEXT_PUBLIC_*` keys are safe (they're visible in browser anyway)
- RLS policies ensure users only see their own data

---

## 📞 NEED HELP?

If something goes wrong:
1. Check the relevant guide above
2. Look at the Troubleshooting section
3. Verify all files are in correct locations
4. Try running `npm install` again

---

## 🚀 YOU'RE READY!

Pick a guide above and get started. The whole process takes about 15-20 minutes.

**Recommended order:**
1. Read `QUICK_START.md` (fastest way to get running)
2. Set up Supabase database
3. Create `.env.local` with API keys
4. Run `npm install` and `npm run dev`
5. Test the app at http://localhost:3000

Good luck! 💪
