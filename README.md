# Easy Rentalz · Invoices

Phone page for photographing vehicle invoices and filing them against the right car.

- Open on a phone, enter the passcode once, take or pick photos. Each photo is one invoice ("+ page" adds pages).
- The text is read on the phone (Tesseract.js); vendor, date, invoice #, line items, subtotal, tax, total, odometer
  and a category are filled in for you to check. A plate or VIN printed on the invoice picks the car.
- Saving sends the invoice to the `invoice-intake` Supabase function, which checks the passcode, stores the photo
  under `receipts/invoices/<VIN>/` and adds a row to `expenses` (source = `phone`), so it shows in the dashboard's
  costs and ROI.

This page holds no keys: the database is reached only through the passcode-protected function.
The passcode is not in this repo. To change it, store the new passcode's SHA-256 in
`public.app_secrets` (name `invoice_passcode_sha256`).
