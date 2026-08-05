# CampusFlix Ticketing Site (Simple Edition)

Same site as before, condensed into as few files as possible so it's easy to
upload from a phone — no folders to fight with except one `public` folder.

## What's inside

```
campusflix-simple/
  server.js         → the entire backend: data storage, admin login, shows,
                       tickets, M-Pesa (Daraja), QR codes — all in one file
  package.json
  .env.example
  public/
    index.html        → homepage (shows list)
    show.html          → checkout + M-Pesa payment flow
    ticket.html          → confirmed ticket with QR code
    admin.html            → login + dashboard + add show + scan tickets,
                             all as one page (switches views with JS)
```

Only **one folder** (`public`) to worry about when uploading — everything
else sits at the top level.

## 1. Install

Needs [Node.js](https://nodejs.org) 18+.

```bash
cd campusflix-simple
npm install
```

## 2. Configure

```bash
cp .env.example .env
```

**Admin password** — generate a bcrypt hash and paste it into
`ADMIN_PASSWORD_HASH` in `.env`:
```bash
node -e "console.log(require('bcryptjs').hashSync('your-password-here', 10))"
```
Set `ADMIN_USERNAME` to whatever you like.

**M-Pesa (Daraja)** — register at
[developer.safaricom.co.ke](https://developer.safaricom.co.ke), create an
app, and fill in `MPESA_CONSUMER_KEY`, `MPESA_CONSUMER_SECRET`, and
`MPESA_PASSKEY` in `.env`. Leave `MPESA_ENV=sandbox` while testing.
`MPESA_CALLBACK_URL` must be a public HTTPS address — it won't work with
`localhost`, so this only fully works once deployed (see below).

## 3. Run locally

```bash
npm start
```
Public site: `http://localhost:3000`
Admin: `http://localhost:3000/admin.html`

## 4. Deploy for free (Render)

1. Push this folder to a GitHub repo
2. On [render.com](https://render.com): **New → Web Service**, connect the
   repo
3. Build command: `npm install` — Start command: `npm start`
4. Add every value from `.env` under Render's **Environment** tab
5. Once live, update `MPESA_CALLBACK_URL` to
   `https://your-app.onrender.com/api/mpesa/callback`

## Using it

- **Buyers** go to the homepage, pick a show, pay with M-Pesa, get a QR ticket.
- **You** go to `/admin.html`:
  - Log in
  - **Shows** — see sales, publish/unpublish, delete
  - **Add a Show** — poster upload with live preview
  - **Scan Tickets** — type a ticket code at the door to mark it used

## Notes

- Data is stored in JSON files under a `data/` folder, created automatically
  the first time the server runs — no database setup needed.
- Poster images are saved in `public/uploads/`.
- If this ever needs to grow into a bigger codebase, splitting `server.js`
  back into separate route files is straightforward — nothing here locks
  you in.
