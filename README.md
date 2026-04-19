# UPPC Member Verifier

Two-page web app for the United Panay Pickleball Club that lets members look themselves up, generate a personal QR code, and lets officers verify membership by scanning that QR at events.

Hosted on GitHub Pages: <https://ajtfermin.github.io/uppc/>

## Pages

- **`lookup.html`** — Member self-service. Search by Membership ID + Name or Birthdate + Name. On success, a QR code is rendered that members screenshot and keep on their phones.
- **`index.html`** — Verifier. The QR code points here with the member's details as URL parameters. The page looks the member up and displays their current status.

## Data source

A shared Google Sheet is the source of truth. Both pages fetch its public CSV export at runtime — no server, no build step, no database.

The sheet is structured as three tables: `Members` (one row per person, permanent `UPPC-#####` ID), `Payments` (one row per transaction, long format so each year's maintenance just appends rows), and `Verifier` (a thin QUERY view that the pages actually read).

## Membership status

Computed live in the sheet from the `Payments` table:

- **Active** — has paid this year's maintenance fee
- **Inactive** — paid the one-time membership but not this year's maintenance
- **Unpaid** — has not paid the one-time membership fee

Status auto-rolls over on January 1 each year without any schema change.

## Local development

No toolchain required — just serve the files:

```sh
python3 -m http.server 8080
```

Then open <http://localhost:8080/lookup.html>.
