                                  
# Website

**Date:** [Insert date]  
**Target:** [www.example-coffee-shop.com](http://www.example-coffee-shop.com)

## 🌐 Reconnaissance

- **Website Type:** Coffee shop e-commerce (menu, online orders, and customer feedback).
- **Tech Stack (Identified):**
    - **Frontend:** React.js
    - **Backend:** Node.js (Express)
    - **Database:** MongoDB

### Tools Used:

- **Reconnaissance:** `whois`, `nmap`, `httpx`.
- **Web Inspection:** Browser DevTools, Burp Suite, `whatweb`.

---

## 🔍 Unusual Observations (Potential Entry Points)

1. **Hidden API Endpoints:**
    
    - `/admin/api/` (403 Forbidden but exists)
    - `/api/orders` and `/api/users` returning verbose error messages.
2. **Insecure Cookies:**
    
    - Session cookie: `SESSIONID=123abc; HttpOnly` (Missing `Secure` flag).
3. **Exposed Configuration Files:**
    
    - `.env` file exposed: Contains credentials (DB connection string).
4. **Error Messages:**
    
    - Login page leaks user enumeration (e.g., "User not found" vs "Incorrect password").
5. **Cross-Site Scripting (XSS):**
    
    - Feedback form reflects input directly in the admin panel without sanitisation.

---

## 🛠️ Potential Vulnerabilities

1. **SQL Injection:**
    
    - Search feature (e.g., `?q=coffee`): No parameterised queries detected.
    - Test payload: `' OR '1'='1`.
2. **Directory Traversal:**
    
    - File upload in "Contact Us" form accepts `.zip` and `.exe` files.
    - Bypass test: `../../../etc/passwd`.
3. **Insecure Direct Object Reference (IDOR):**
    
    - API endpoints `/api/orders?user_id=123`: Changing `user_id` to another number retrieves different orders.
4. **CSRF (Cross-Site Request Forgery):**
    
    - No CSRF token on order submission form.
5. **Rate Limiting:**
    
    - No CAPTCHA or rate limiting on login form; prone to brute force attacks.

---

## 🧑‍💻 Key Learnings and Notes

- **Dynamic Input Validation:** Many forms on the website lack input validation; vulnerable to XSS, SQLi.
- **Information Leakage:** Server headers reveal framework versions (`X-Powered-By: Express 4.17.1`).
- **Session Management:** Missing critical cookie attributes (`Secure`, `SameSite`).

### Links to Useful Resources:

- XSS prevention: OWASP XSS Guide
- SQL Injection basics: SQL Injection Cheat Sheet
- CSRF explained: OWASP CSRF Guide

---

## 🖇️ Connections (For Note Linking)

- Relate **hidden API endpoints** to enumeration tools like `ffuf`.
- Link exposed `.env` file findings to **Misconfigured File Permissions** in web servers.
- Connect rate-limiting gaps to **Brute Force Attack Exploitation**.

---

Use this as a template in your **Obsidian vault** to build a graph of insights. You can also track specific testing results, tools, and unique observations!