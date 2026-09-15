# Cash Register X Automatic V9.1 — Checkout + Database Fix

This build is based directly on the uploaded V9.1 ONLINE_ONLY_CLEAN project.

## What was fixed in the application

- Checkout now saves the parent `sales` row first and uses the exact `sales.id` returned by Supabase for every `sale_items.sale_id`.
- Checkout stops if the parent sale fails instead of continuing into the foreign-key error.
- Each sale-item insert is checked for errors.
- If a sale item fails, the parent sale is removed to avoid a half-saved transaction.
- If the V9.1 `pwd_discount` column is missing, checkout retries without that optional column so a sale can still be completed. Run `MASTER_DATABASE_FIX.sql` to permanently enable PWD tracking.
- Sales/receipts/exports/dashboard are resilient if older sales rows do not yet have the PWD flag column.
- Cache-busted script URLs were updated to prevent GitHub Pages from serving the previous JavaScript.

## One database step

Supabase cannot be altered by a GitHub Pages browser app. Therefore, the only database-side step is to run:

**`MASTER_DATABASE_FIX.sql`**

Open Supabase → SQL Editor → paste/run that ONE file.

It includes the V9.1 columns, PWD discount column, username login setup, Admin/Staff RLS policies, inventory numeric units, and the correct `sale_items -> sales` foreign key. It is written to be safe to run on the existing database and does not delete business data.

After running it, redeploy this ZIP to GitHub Pages and hard-refresh with `Ctrl + Shift + R`.

## Important

Do not put a Supabase service-role key in `config.js` or GitHub Pages. The browser should only use the publishable/anon key.
