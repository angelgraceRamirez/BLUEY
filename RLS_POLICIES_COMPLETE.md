# SUPABASE RLS POLICIES - COMPLETE SETUP GUIDE

**Last Updated:** 2026-04-22  
**Project ID:** cuparnvpiaxcuswbstla

---

## WHAT IS RLS?

**RLS = Row Level Security**

- Users can ONLY see their own data
- Secure and prevents unauthorized access
- Implemented at the database level (very safe)

**Example:** User A cannot see User B's sensor readings or profile

---

## STEP-BY-STEP SETUP

### STEP 1: Go to Supabase Dashboard

1. Visit: https://app.supabase.com/
2. Sign in with your Supabase account
3. Click on project: **cuparnvpiaxcuswbstla**
4. Left sidebar → Click **Database** → **Tables**

---

### STEP 2: Enable RLS on `profiles` Table

1. Click the **`profiles`** table
2. Click the **Auth** tab (top right)
3. Toggle **RLS** to **ON** (green switch)
4. Save changes

**Add Policies:**

1. Click **New policy** button
2. Click **Create policy from template**
3. Select: **"Enable read access for users based on their UID"**
   - Column: `id`
   - Click **Review** → **Save**

4. Click **New policy** again
5. Select: **"Enable insert for authenticated users"**
   - Click **Review** → **Save**

6. Click **New policy** again
7. Select: **"Enable update for users based on their UID"**
   - Column: `id`
   - Click **Review** → **Save**

8. Click **New policy** again
9. Select: **"Enable delete for users based on their UID"**
   - Column: `id`
   - Click **Review** → **Save**

---

### STEP 3: Enable RLS on `sensor_readings` Table

1. Click the **`sensor_readings`** table
2. Click the **Auth** tab
3. Toggle **RLS** to **ON**
4. Save changes

**Add Policies:**

1. Click **New policy** button
2. Select: **"Enable read access for users based on their UID"**
   - Column: `user_id`
   - Click **Review** → **Save**

3. Click **New policy**
4. Select: **"Enable insert for authenticated users"**
   - Click **Review** → **Save**

5. Click **New policy**
6. Select: **"Enable update for users based on their UID"**
   - Column: `user_id`
   - Click **Review** → **Save**

7. Click **New policy**
8. Select: **"Enable delete for users based on their UID"**
   - Column: `user_id`
   - Click **Review** → **Save**

---

### STEP 4: Enable RLS on `activity_logs` Table

1. Click the **`activity_logs`** table
2. Click the **Auth** tab
3. Toggle **RLS** to **ON**
4. Save changes

**Add Policies:**

1. Click **New policy**
2. Select: **"Enable read access for users based on their UID"**
   - Column: `user_id`
   - Click **Review** → **Save**

3. Click **New policy**
4. Select: **"Enable insert for authenticated users"**
   - Click **Review** → **Save**

5. Click **New policy**
6. Select: **"Enable update for users based on their UID"**
   - Column: `user_id`
   - Click **Review** → **Save**

7. Click **New policy**
8. Select: **"Enable delete for users based on their UID"**
   - Column: `user_id`
   - Click **Review** → **Save**

---

## ALTERNATIVE: Using SQL (Copy & Paste)

If you prefer to use SQL instead of templates, go to **SQL Editor** and paste these:

### SQL for `profiles` table:

```sql
-- profiles: Read own profile
CREATE POLICY "Users can read own profile"
ON profiles FOR SELECT
USING (auth.uid() = id);

-- profiles: Insert own profile
CREATE POLICY "Users can insert own profile"
ON profiles FOR INSERT
WITH CHECK (auth.uid() = id);

-- profiles: Update own profile
CREATE POLICY "Users can update own profile"
ON profiles FOR UPDATE
USING (auth.uid() = id)
WITH CHECK (auth.uid() = id);

-- profiles: Delete own profile
CREATE POLICY "Users can delete own profile"
ON profiles FOR DELETE
USING (auth.uid() = id);
```

### SQL for `sensor_readings` table:

```sql
-- sensor_readings: Read own readings
CREATE POLICY "Users can read own sensor readings"
ON sensor_readings FOR SELECT
USING (auth.uid() = user_id);

-- sensor_readings: Insert readings
CREATE POLICY "Users can insert sensor readings"
ON sensor_readings FOR INSERT
WITH CHECK (auth.uid() = user_id);

-- sensor_readings: Update own readings
CREATE POLICY "Users can update own sensor readings"
ON sensor_readings FOR UPDATE
USING (auth.uid() = user_id)
WITH CHECK (auth.uid() = user_id);

-- sensor_readings: Delete own readings
CREATE POLICY "Users can delete own sensor readings"
ON sensor_readings FOR DELETE
USING (auth.uid() = user_id);
```

### SQL for `activity_logs` table:

```sql
-- activity_logs: Read own logs
CREATE POLICY "Users can read own activity logs"
ON activity_logs FOR SELECT
USING (auth.uid() = user_id);

-- activity_logs: Insert logs
CREATE POLICY "Users can insert activity logs"
ON activity_logs FOR INSERT
WITH CHECK (auth.uid() = user_id);

-- activity_logs: Update own logs
CREATE POLICY "Users can update own activity logs"
ON activity_logs FOR UPDATE
USING (auth.uid() = user_id)
WITH CHECK (auth.uid() = user_id);

-- activity_logs: Delete own logs
CREATE POLICY "Users can delete own activity logs"
ON activity_logs FOR DELETE
USING (auth.uid() = user_id);
```

---

## HOW TO USE SQL EDITOR

1. Left sidebar → **SQL Editor**
2. Click **New query** button (or paste in blank area)
3. Copy and paste the SQL code above
4. Click the blue **Run** button (bottom right)
5. Wait for "Success" message
6. Repeat for each table

---

## VERIFICATION CHECKLIST

After setting up RLS, verify:

- ✅ `profiles` table: RLS enabled
- ✅ `profiles` table: 4 policies (SELECT, INSERT, UPDATE, DELETE)
- ✅ `sensor_readings` table: RLS enabled
- ✅ `sensor_readings` table: 4 policies (SELECT, INSERT, UPDATE, DELETE)
- ✅ `activity_logs` table: RLS enabled
- ✅ `activity_logs` table: 4 policies (SELECT, INSERT, UPDATE, DELETE)

**To verify:**
1. Go to each table
2. Click **Auth** tab
3. Check RLS is ON
4. Count the policies listed

---

## HOW IT WORKS

### `auth.uid()` Function

- Returns the current logged-in user's ID
- Only works inside RLS policies
- Automatically set by Supabase Auth

### Example:

```sql
USING (auth.uid() = user_id)
```

This means: "Only allow if the logged-in user's ID matches the `user_id` in this row"

---

## COMMON MISTAKES TO AVOID

❌ **Don't:** Enable RLS without adding policies (data becomes inaccessible)
✅ **Do:** Enable RLS, then add policies

❌ **Don't:** Use wrong column name (e.g., `user_id` instead of `id`)
✅ **Do:** Match the column that stores the user ID

❌ **Don't:** Forget to enable ALL operations (SELECT, INSERT, UPDATE, DELETE)
✅ **Do:** Enable all 4 policy types

---

## TROUBLESHOOTING

### "RLS Policy Denied" Error

**Cause:** User doesn't have permission to access data
**Solution:** Check if policies are correctly set up for that table

### "No rows returned" When Data Exists

**Cause:** RLS is blocking access due to wrong policy
**Solution:** Verify `auth.uid()` matches the user ID column

### "Insert failed"

**Cause:** Missing INSERT policy
**Solution:** Add INSERT policy for the table

---

## FINAL CHECKLIST

Before moving to VS Code:

- [ ] Created 3 tables (profiles, sensor_readings, activity_logs)
- [ ] Enabled RLS on all 3 tables
- [ ] Added 4 policies to each table
- [ ] Copied API keys to `.env.local` file
- [ ] Created `lib/supabase/client.ts`
- [ ] Created `lib/supabase/server.ts`
- [ ] Ran `npm install`
- [ ] Ready to run `npm run dev`

---

## NEXT STEPS

1. Complete RLS setup above
2. Create `.env.local` with API keys
3. Copy code files to VS Code
4. Run: `npm install`
5. Run: `npm run dev`
6. Go to: http://localhost:3000

You're done! Your HydroFlow app is secure and ready to use.
