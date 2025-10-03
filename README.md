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

## License

MIT License – feel free to use, modify, and experiment.

---

If you want, I can now **draft a visually polished README with screenshots, example OTPs, and tips for testing with NFC/GOV keys** so it’s ready for GitHub. Do you want me to do that?
