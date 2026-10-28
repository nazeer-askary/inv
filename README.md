# Easy Rentalz · Invoices

Phone page for photographing vehicle invoices and filing them against the right car.

- First time on a phone: **Join** with the team code plus full name, email, phone and a passcode of your own.
  After that the phone stays signed in; on a new phone use **Sign in** (email + your passcode).
- Take or pick photos. Each photo is one invoice ("+ page" adds pages).
- The text is read on the phone (Tesseract.js); vendor, date, invoice #, line items, subtotal, tax, total, odometer
  and a category are filled in for you to check. A plate or VIN printed on the invoice picks the car.
- Saving sends the invoice to the `invoice-intake` Supabase function, which checks the passcode, stores the photo
  under `receipts/invoices/<VIN>/` and adds a row to `expenses` (source = `phone`), so it shows in the dashboard's
  costs and ROI.

Every invoice records who uploaded it (`expenses.uploaded_by`, `uploaded_by_name`) and from which device.

This page holds no keys: the database is reached only through the `invoice-intake` function.
People are in `invoice_users` (passcodes stored as salted PBKDF2 hashes); signed-in phones in `invoice_sessions`.
The team code is not in this repo. To change it, store the new code's SHA-256 in `public.app_secrets`
(name `invoice_passcode_sha256`). To reset someone's passcode, delete their `invoice_users` row (their past
invoices keep their name) and have them join again; to block someone, set `active = false`.
