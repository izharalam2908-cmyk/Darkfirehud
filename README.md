# DARKFIRE HUB — Mobile-Friendly Direct UPI QR + PRO Store + Admin Settings

This starter replaces online gateway checkout with a direct UPI QR flow. Admin sets the merchant UPI ID and payee name in the admin panel. Customers select a product, receive a QR for the exact price, pay using their UPI app, then submit their UTR/payment reference. The order appears in Admin as `pending_review`. The admin must verify the payment in their bank/UPI records before approving; only approved orders unlock the configured product content and an access code.

**Important:** This is a starter project, not a hosted/live payment service. A QR and UTR do not automatically prove money was received. This version does not connect to bank records, does not automatically reconcile payment, and does not automatically refund. Use a UPI ID that you own or are authorized to collect payments with.

## Features
- Direct UPI deep-link QR generated per product and amount.
- Customer order tracking token and UTR/reference submission.
- Duplicate reference check and pending-review queue.
- Admin-only settings for UPI ID, payee name, payment instructions, and owner-uploaded QR photo (PNG/JPG/WebP, max 3 MB). If a QR photo is uploaded, it is shown in customer checkout; the customer must enter the exact displayed amount in their UPI app if the static QR does not prefill it.
- Admin can add, edit, price, label/version, activate/deactivate, configure unlock content, and mark products as regular or PRO Store products. Existing products with orders can be deactivated instead of deleted.
- Manual approve/reject, access code generation, and content unlock after approval.
- SQLite persistence.
- Admin-managed sensitivity defaults (General, Red Dot, 2x, 4x, Sniper, Free Look), default theme/language/sound/motion, Pro Store visibility and store notice.
- Separate PRO Version Store on the customer panel. A PRO product is unlocked only after the order is manually approved.

## Setup (PC/laptop recommended)
1. Install Node.js 20 or newer from https://nodejs.org/.
2. Extract this ZIP, open a terminal in the extracted `DARKFIRE_HUB_PAYMENT` folder.
3. Copy `.env.example` to `.env`.
4. Put a long random value in `ADMIN_TOKEN`. Generate one with `node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"`.
5. Run `npm install` and then `npm start`.
6. Open `http://localhost:3000` for the customer website.
7. Open `http://localhost:3000/admin.html` for the admin panel and enter the exact `ADMIN_TOKEN`.
8. In **Direct UPI QR Settings**, enter your real UPI ID and payee display name, or upload your owner QR photo and save. The QR image is stored in the local SQLite settings database.
9. In **Product & Version Editor**, edit the sample products or create a new product. Example product ID: `science-city-v2`; name: `Science City — Version 2`; set the price, version label/username label, store description, and content/instructions delivered after approval. Product IDs are lowercase letters/numbers/hyphens and cannot be changed after creation.
10. Open the customer site, buy a regular or PRO Store product, scan the shown QR with a UPI app, pay the exact amount displayed, then submit the UTR/reference number from the successful transaction. If you use a static uploaded QR, the amount may need to be entered manually in the UPI app.
11. In Admin → Orders, independently verify the exact amount and reference in your UPI/bank transaction history. Only then click **Payment Verified — Approve & Unlock**.

In Admin → **All User Panel Settings**, set the default sensitivity sliders, theme, language, sound, reduced animation, Pro Store visibility and customer-facing store notice. In Product & Version Editor, tick **Is this product in the separate PRO Store** for premium items.

## Hosting and security before taking real payments
- Deploy to a trusted Node.js host with HTTPS and persistent disk for `darkfire-orders.sqlite`.
- Set `ADMIN_TOKEN` in the host's environment variables. Never expose it in frontend files or share it.
- Confirm your payment provider/bank rules, merchant eligibility, privacy notice, terms, and refund policy. If you are under 18, a legally eligible adult should own and manage the merchant account.
- Test the full order/UTR/admin flow with a small controlled transaction first.
- A UTR can be fabricated or copied; do not approve based only on the submitted text or a screenshot. Confirm actual funds in the merchant account.
- Keep backups and protect the database. Add admin login/MFA, rate limiting, monitoring, and privacy/retention policies before larger-scale production.

## Limitations
- Manual bank verification only; no bank API or automatic confirmation.
- No automatic refund.
- Customers should save their order tracking token; email delivery is not configured.
- The product content field delivers text instructions, not a protected downloadable file.
- The QR preview button in admin is a preview of a sample ₹49 payment QR; customer checkout QR is generated from each order's actual amount.
