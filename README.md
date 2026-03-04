# 🧃 OWASP Juice Shop – HW3 Login Form

A deliberately vulnerable login page built for **TAMU HW3 (OWASP Juice Shop)** to demonstrate client-side validation and XSS exploitation.

---

## 📋 Assignment Overview

**Course:** Web Security  
**Assignment:** HW3 – OWASP Juice Shop  
**Parts covered:** Part 2 (Login Form) + Part 3 (XSS Exploit)

---

## 🚀 How to Run

No build tools required. Just open the file in any browser:

```bash
# Option 1: Double-click the file
open login.html

# Option 2: Serve locally (recommended for testing)
python3 -m http.server 8080
# Then visit http://localhost:8080/login.html
```

---

## ✅ Features Implemented

### Client-Side Validation
| Rule | Implementation |
|------|---------------|
| Empty field prevention | Checks `value.trim()` before submission |
| Email `@` check | `email.includes('@')` |
| Password min 8 chars | `password.length < 8` |
| Live inline feedback | `blur` event listeners on each input |

### UI / UX
- Juice Shop-inspired dark theme with orange accent
- Real-time field validation on blur
- Enter key support
- Visual field states (red = invalid, green = valid)

---

## ⚠️ Intentional Vulnerability (Part 3)

The form contains a **deliberate XSS vulnerability** for demonstration purposes.

### Vulnerable Code (line ~135 in `login.html`)
```javascript
// ❌ VULNERABLE — reflects raw user input into the DOM
document.getElementById('reflected-email').innerHTML = email;
```

### How to Exploit (Reflected XSS)
1. Open `login.html` in a browser
2. In the **Email** field, paste:
   ```
   <img src=x onerror="alert('XSS!')">
   ```
3. Click **Login** (or press Enter)
4. An alert box will pop up — confirming script execution

### Fix
```javascript
// ✅ SECURE — escapes HTML entities automatically
document.getElementById('reflected-email').textContent = email;
```
Changing `innerHTML` → `textContent` prevents the browser from parsing the input as HTML, neutralizing the XSS vector.

---

## 🔒 Security Notes

| Area | Status | Note |
|------|--------|------|
| Empty field check | ✅ Secure | Both fields required |
| Email format | ✅ Validated | `@` presence checked |
| Password length | ✅ Validated | Min 8 characters enforced |
| XSS via innerHTML | ❌ Vulnerable | **Intentional for Part 3 demo** |
| Server-side auth | ⚠️ Simulated | Client-only (no real backend) |

---

## 📁 File Structure

```
.
├── login.html      # Main login form (all-in-one: HTML + CSS + JS)
└── README.md       # This file
```

---

## 🗒️ Part 2 Write-up (100 words)

I implemented a single-file HTML/JS login form styled after OWASP Juice Shop. The form validates both fields client-side before submission: it checks that the email field is non-empty and contains the `@` character, and that the password is at least 8 characters long. Empty submissions are blocked immediately. I used `blur` event listeners for live inline validation feedback with visual cues (red/green borders). One intentional vulnerability was introduced for Part 3: the `reflected-email` div uses `innerHTML` to display the entered email, which allows a reflected XSS payload to execute arbitrary JavaScript when the login button is clicked.
