# Build for Tomorrow

This sample hackathon website uses Flask and SQLite for public team registrations and a private organizer dashboard.

## Run locally

Use Python 3.10 or newer, then run these commands from the project folder:

```powershell
python -m pip install -r requirements.txt
python app.py create-admin --username admin
python app.py seed-demo-data
python app.py
```

When creating the admin account, enter a strong password of at least 12 characters at the hidden password prompts. The password is stored as a hash, not plaintext. Then open:

- Public event site: `http://127.0.0.1:5000/`
- Admin sign-in: `http://127.0.0.1:5000/admin/login`

The sample admin username is `admin`; the password is the one you set during account creation. Demo registrations use fictional names and `.test` email addresses. They are labeled **Sample** and excluded from real registration totals. Remove them with the dashboard button or rerun the seed command after removing them.

The database is created at `instance/registrations.sqlite3`. Do not open `index.html` directly using `file://`; the registration form submits to Flask.

## Admin dashboard

The private dashboard supports searching registrations, viewing team details, and exporting a CSV. Admin login uses hashed passwords, CSRF-protected forms, secure session settings, and a temporary IP-based lock after repeated failed login attempts. The admin routes do not expose registration data to logged-out visitors.

Create additional admin accounts with:

```powershell
python app.py create-admin --username another-organizer
```

## API

- `GET /api/health` — simple server health check.
- `POST /api/registrations` — accepts JSON fields matching the public form. A successful request returns HTTP 201 and a registration ID.
- Admin routes: `/admin/login`, `/admin`, `/admin/export.csv`.

## Before deployment

This project is a learning/demo starter, not a fully hardened production service. Before handling real registrations:

- Replace all sample event information and confirm prize and eligibility details with the organizers.
- Publish an accurate privacy notice and decide who may access registrations and how long they are kept.
- Set a long, random `FLASK_SECRET_KEY` in the hosting provider's secret settings. Set `SESSION_COOKIE_SECURE=1` when HTTPS is configured.
- Use a production WSGI server behind HTTPS; do not expose Flask's built-in development server.
- Keep the SQLite database and backups private. Use managed PostgreSQL if the hosting environment does not provide persistent local storage or if multiple app instances are needed.
- Add operational backups, monitoring, spam controls, and a process for deleting or exporting personal data upon organizer request.
- Use unique administrator passwords and grant accounts only to authorized event organizers.

## Tests

Run the backend and admin tests with:

```powershell
python -m unittest -v
```
