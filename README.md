# Retail Inventory Management System (IMS)

A lightweight Flask web application with Bootstrap 5 styling and session-based authentication.

---

## 🚀 Quick Start

### 1. Install Dependencies
```bash
pip install Flask Flask-WTF
```

### 2. Run the Application
```bash
python app.py
```
Open your browser and navigate to: [http://127.0.0.1:5000](http://127.0.0.1:5000)

---

## 🔑 Default Credentials

| Username | Password | Role |
| :--- | :--- | :--- |
| `admin` | `password` (or `admin`) | Admin |
| `user` | `password` | User |

---

## 📂 Project Structure

```text
ims/
├── app.py              # Main Flask application & routing
├── login.py            # Form validation models (WTForms)
├── requirements.txt    # Python dependencies
├── .gitignore          # Ignored cache & database files
├── templates/
│   ├── base.html       # Master layout with Bootstrap 5
│   ├── index.html      # Dashboard template
│   └── login.html      # Login page template
└── static/
    └── css/
        └── style.css   # Custom CSS styles
```

---

## 🛠️ Git Commands Cheat Sheet

### First Time Setup (Pushing to GitHub)
```bash
# 1. Rename branch to main
git branch -M main

# 2. Add your GitHub repository link
git remote add origin https://github.com/rohish-shrees12/retail-ims.git

# 3. Push your code
git push -u origin main
```

### Every Time You Update Code (Workflow)
```bash
# 1. Check changed files
git status

# 2. Stage all changes
git add .

# 3. Commit changes with a descriptive message
git commit -m "Describe what you changed"

# 4. Push updates to GitHub
git push
```
