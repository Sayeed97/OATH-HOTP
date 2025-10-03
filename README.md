# HOTP Demo Server

A **demo system for managing one-time passwords (HOTP)** with a simple dashboard. This project allows you to add users, verify HOTP codes, track OTP drift, and flag secrets for reset. It can be easily deployed on any local machine for testing and learning purposes.

**Tested using Hirsch uTrust NFC and GOV hardware security keys.**

---

## Features

* Add and manage users with HOTP secrets.
* Verify HOTP codes from users.
* Track OTP drift between the hardware security key and the server.
* Flag secrets that need resetting.
* Admin dashboard with the last 10 OTP activity logs.
* Fully deployable locally.

---

## Requirements

* Python 3.10+
* Flask
* Flask-SQLAlchemy
* pyotp

Here’s a complete `pip` installation instruction for all libraries required by your HOTP demo project:

```bash
# Core Flask framework
pip install Flask

# Database support with SQLAlchemy
pip install Flask-SQLAlchemy

# HOTP / OTP generation and verification
pip install pyotp

# Optional (for better security and flashing messages)
pip install itsdangerous Jinja2 Werkzeug click MarkupSafe
```

Or, to install all at once in a single command:

```bash
pip install Flask Flask-SQLAlchemy pyotp itsdangerous Jinja2 Werkzeug click MarkupSafe
```

> Tip: It’s a good practice to use a **virtual environment** before installing dependencies:

```bash
python -m venv venv
source venv/bin/activate   # Linux/macOS
venv\Scripts\activate      # Windows
```

Then run the `pip install` command inside the virtual environment.

## Getting Started (Local Deployment)

1. **Clone the repository**:

```bash
git clone https://github.com/yourusername/hotp-demo.git
cd hotp-demo
```

2. **Run the server**:

```bash
python server.py
```

3. **Access the application**:

* User portal: [http://127.0.0.1:5000/user](http://127.0.0.1:5000/user)
* Admin dashboard: [http://127.0.0.1:5000/admin](http://127.0.0.1:5000/admin)

> The server will automatically create a SQLite database (`users.db`) in the project directory.

---

## How to Use

### Admin Dashboard

* Add users with a username and a HEX secret.
* View all users with counters, drift, and reset flags.
* Delete users if needed.
* Track the last 10 OTP verification logs.

### User Portal

* Enter your username and HOTP code to verify.
* The server will handle counter tracking and drift.
* If OTP drift is too large, the admin will see a reset flag for the user.

---

## Notes

* This is a **demo project**, intended for learning and testing HOTP workflows.
* Tested with Hirsch uTrust NFC and GOV hardware security keys.
* Secrets are stored in plain text in the SQLite database — do **not** use real secrets for production.
* Works on any machine with Python installed.

---
# Code Breakdown

This document explains how the HOTP demo server is structured and how each part of the code works. It is intended for developers who want to understand, modify, or extend the project.

---

## 📂 Project Structure

```
OATH-HOTP/
│
├── server.py        # Main Flask app (routes, logic, DB handling)
├── models.py        # SQLAlchemy database models
├── templates/       # HTML templates for user and admin dashboards
│   ├── user.html
│   ├── admin.html
│   └── base.html
├── static/          # (Optional) CSS/JS files for styling
├── users.db         # SQLite database (auto-created at runtime)
└── CODE_BREAKDOWN.md
```

---

## ⚙️ Key Components

### 1. **`server.py`**

The main application file. Handles:

* Flask routes (`/user`, `/admin`)
* HOTP verification using **pyotp**
* Database updates (counters, drift, reset flag)
* Logging OTP activity

Flow inside `server.py`:

1. Load user details from DB (username, secret, counters).
2. Generate HOTP object with user’s secret.
3. Compare submitted OTP with server counter.
4. Update counters or mark reset flag.
5. Log verification result.

---

### 2. **`models.py`**

Defines the database schema using SQLAlchemy.
The `User` model includes:

* `id` → primary key
* `username` → unique identifier
* `secret` → base32 encoded HOTP secret
* `counter` → server-side counter
* `key_counter` → last counter from device
* `drift` → tracked mismatch between counters
* `reset` → flag if secret reset is required

Also includes `Log` model to store the last 10 OTP activity entries.

---

### 3. **Templates (`/templates`)**

HTML files rendered by Flask:

* **`user.html`** → User login page where username and OTP are submitted.
* **`admin.html`** → Admin dashboard for managing users, viewing counters, drift, reset flags, and logs.
* **`base.html`** → Shared layout for consistent styling.

Templates use **Jinja2** to dynamically insert data from Flask (like user list, OTP logs).

---

### 4. **Database (`users.db`)**

* SQLite database, auto-created on first run.
* Stores all users and OTP logs.
* Updates automatically whenever a user submits an OTP or admin modifies a record.

---

## 🔄 Workflow Breakdown

1. **User submits OTP** on `/user`.
2. Server checks OTP with `pyotp.HOTP.verify()`.
3. If:

   * ✅ Exact match → counters increment, drift reset.
   * ⚠️ Within drift window (e.g., +1 to +5) → OTP accepted, drift recorded.
   * ❌ Too far ahead or invalid → reset flag set.
4. Event is logged in the `Log` table.
5. Admin can review results on `/admin`.

---

## License

MIT License – feel free to use, modify, and experiment.

---

