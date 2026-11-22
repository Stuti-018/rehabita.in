# Rehabita admin app + booking page: setup guide

You get two pages:
- `rehabita.in/book/` : public appointment request form (no login)
- `rehabita.in/admin/` : private admin app (login required, only you)

## 1. Create the database (Supabase, free plan is enough to start)
1. Go to supabase.com, sign up, click **New project**. Name it `rehabita`. Choose the **Mumbai (ap-south-1)** region. Save the database password somewhere safe.
2. Open **SQL Editor > New query**, paste the whole of `schema.sql`, click **Run**. It should say "Success".
   The rules in it are the real security: only your login can read patient data; the public form can only add a request.

## 2. Make it admin-only
1. **Authentication > Sign In / Providers > Email**: switch OFF "Allow new users to sign up". Save.
2. **Authentication > Users > Add user > Create new user**: email `rehabitaofficial@gmail.com`, a long unique password, tick "Auto Confirm User".
   (If you want a different login email, change it in `is_admin()` inside `schema.sql` before running it.)

## 3. Connect the pages
1. **Project Settings > API**: copy the **Project URL** and the **anon public** key.
2. Paste them into `config.js`. NEVER use or paste the `service_role` key.

## 4. Put it on your site
1. Copy the folders `admin`, `book` and the file `config.js` into the root of your GitHub repo for rehabita.in. Do NOT upload `schema.sql` or this guide.
2. Commit. GitHub Pages publishes it in a minute or two. Your GoDaddy settings need no change.
3. Add a "Book an appointment" button on your site linking to `/book/`.

## 5. Test (do all four)
1. Open `/book/`, submit a test request.
2. Open `/admin/`, log in. The request should appear under **Requests**. Press **Convert**, then add a visit and an invoice for it.
3. Log out and open `/admin/` again: you must see only the login box.
4. Delete the test patient, visit, invoice and request.

## Be aware
- The code in a public GitHub repo is visible to everyone. That is fine. Never commit exports, spreadsheets or patient data.
- Two-step verification is not built into the login yet. Use a long unique password and ask for 2FA to be added.
- The free Supabase plan has no automatic backups you can restore, and pauses a project after a week with no activity. Use the Export buttons weekly and keep the CSV files in your own Drive. Paid plans add daily backups.
- This form collects only name, phone, preferred time and a one-line reason. Have a lawyer check your privacy notice and consent wording under the DPDP Act.
- Your older test data in the Google Sheet is not moved over. Tell me if you have real records to import.
