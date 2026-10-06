# Security Architecture & Best Practices Guide

This document provides a comprehensive security overview of AnimeBro, detailing the defensive protections implemented across the codebase, as well as hardening checklists for production deployments.

---

## 1. Core Security Defenses

### A. SQL Injection Prevention
- **100% Prepared Statements**: All database operations throughout the platform and admin panel use PDO prepared statements with parameterized inputs.
- No dynamic, unescaped SQL string concatenation is permitted in query logic.

### B. Cross-Site Request Forgery (CSRF) Protection
- All admin forms and critical user actions require a cryptographically secure CSRF token generated via `csrf_token()`.
- State-changing POST requests are verified server-side with `require_csrf()`. Mismatched or missing tokens trigger immediate `403 Forbidden` termination.

### C. Cross-Site Scripting (XSS) Mitigation
- All user-supplied output rendered in HTML templates is escaped using `htmlspecialchars($string, ENT_QUOTES, 'UTF-8')` or dedicated sanitization functions (`e()`).
- Rich text fields (such as DMCA pages or custom announcements) are sanitized with whitelist filtering.

### D. Password Hashing & Authentication Security
- Passwords for administrators and registered members are hashed using industry-standard `password_hash($password, PASSWORD_DEFAULT)` (Bcrypt with adaptive work factor).
- Plaintext passwords are never logged, echoed, or stored in cookies.

### E. Session Security & Isolation
- Admin sessions and user sessions use isolated session namespaces (`$_SESSION['admin_user']`, `$_SESSION['user']`).
- Session IDs are regenerated upon authentication (`session_regenerate_id(true)`) to prevent session fixation attacks.
- Session cookies are set with `HttpOnly` and `SameSite=Lax` / `Strict` attributes.

### F. File Upload Hardening
- File uploads in the admin media library validate file extensions, MIME types, and image magic bytes.
- Direct script execution (.php, .phtml, .cgi, .pl) inside the `/uploads/` directory is blocked via server configuration.

---

## 2. Post-Sale Credential Rotation Checklist

Upon acquiring the AnimeBro platform, execute this mandatory security rotation:

1. **Rotate Master Admin Password**:
   - Log into Admin Panel -> **Settings** -> **Admin Security** (`/admin/settings.php#adminSecuritySection`).
   - Change both the admin username and password to unique, high-entropy credentials.
2. **Rotate Database User Credentials**:
   - Change the MySQL user password in cPanel / MySQL CLI.
   - Update `common/db_config.php` with the new password.
3. **Rotate Third-Party API Keys**:
   - Generate new API keys on **TheTVDB**, **StreamHG**, **StreamRuby**, and **EarnVids**.
   - Update them in **Admin Settings -> API Integrations & Video Providers**.
4. **Enforce HTTPS / SSL**:
   - Install an SSL certificate (Let's Encrypt / Cloudflare) and verify all traffic redirects to `https://`.
5. **Protect Sensitive Files**:
   - Verify that `.htaccess` blocks web access to `common/db_config.php`.

---

## 3. Server Hardening Recommendations

| Area | Recommended Production Configuration |
| :--- | :--- |
| **PHP Display Errors** | Set `display_errors = Off` and `log_errors = On` in `php.ini`. |
| **HTTP Security Headers** | Ensure `.htaccess` sends `X-Frame-Options`, `X-Content-Type-Options`, and `X-XSS-Protection`. |
| **Directory Indexing** | Ensure `Options -Indexes` is active to prevent directory browsing. |
| **Cloudflare WAF** | Place your domain behind Cloudflare to enable DDoS protection, bot management, and web application firewall filtering. |
