# AnimeBro - Complete Platform Overview & Documentation

> **Professional Handover & Architecture Guide for Buyers and Developers**

---

## 1. Executive Summary

**AnimeBro** is a comprehensive, high-performance web platform built with modern PHP, MySQL/MariaDB, and Tailwind CSS. It delivers an end-to-end streaming solution for **Anime series**, **Cartoon series**, and **Anime movies**, featuring multi-audio dub support (Tamil, Hindi, English, Malayalam, Telugu, Multi-Audio), integrated multi-server video provider uploads, TheTVDB v4 automated metadata scraping, responsive PWA support, and a conversion-oriented public Landing Page with dedicated Admin Panel management.

---

## 2. Core Platform Capabilities

- **Front Door Landing Page (`/`):** High-converting long-form landing page with dynamic hero banner, featured showcases, 3-step guide, FAQ accordion, community channels, and global website logo synchronization.
- **Main Streaming Portal (`/home`):** Rich anime/cartoon/movie catalog, hero banner slider, trending rankings, genre/dub filtering, search, and continue watching.
- **Player & Multi-Server Streaming:** Responsive video player supporting MP4, HLS (.m3u8), Embed codes, Server 1 & Server 2 toggles, custom player promotions, and light/dark theatre modes.
- **Content Hierarchy:**
  - **Anime:** Series &rarr; Seasons &rarr; Episodes (Standard & Continuous/Absolute episode numbering supported).
  - **Cartoons:** Dedicated cartoon series and episode manager.
  - **Movies:** Standalone feature films with dedicated multi-provider servers.
- **Automated Metadata Scraping:** TheTVDB API v4 integration for one-click import of posters, banners, season/episode overviews, air dates, and character metadata.
- **Integrated Video Providers:** Direct multi-provider file uploader supporting **StreamHG**, **StreamRuby**, and **EarnVids** with upload progress tracking, automated file code extraction, and connection testing.
- **User Engagement:** User registration, avatars library, watchlist, watch history progress tracking, threaded comments, and broken link report system.
- **Administrative Control:** Role-based staff management, detailed SEO suite (XML sitemap, dynamic robots.txt, OpenGraph tags), data import/export backups, and master settings panel.

---

## 3. Technology Stack & Environment

| Layer | Technology |
| :--- | :--- |
| **Backend Language** | PHP 7.4 - 8.3+ (Standard procedural/OOP hybrid, zero heavy framework overhead) |
| **Database** | MySQL 5.7+ / MariaDB 10.3+ with PDO extension and UTF8mb4 encoding |
| **Frontend Styling** | Tailwind CSS (CDN-based / custom utility classes), FontAwesome 6 |
| **Web Server** | Apache 2.4+ with `mod_rewrite`, `mod_headers`, and `mod_mime` enabled |
| **Metadata API** | TheTVDB API v4 (JSON REST with automated bearer token caching) |
| **Storage Architecture**| Local structured uploads (`uploads/posters`, `uploads/banners`, `uploads/avatars`, etc.) |

---

## 4. Documentation Sitemap

Every aspect of this platform is documented in this package:

### 📂 `01-Documentation/`
- [`INSTALLATION-GUIDE.md`](file:///c:/Users/MAARA/Desktop/ANIME/ANIMEBRO-SALE-PACKAGE/01-Documentation/INSTALLATION-GUIDE.md): Complete A-to-Z setup on new hosting or VPS.
- [`ADMIN-GUIDE.md`](file:///c:/Users/MAARA/Desktop/ANIME/ANIMEBRO-SALE-PACKAGE/01-Documentation/ADMIN-GUIDE.md): Section-by-section Admin Panel walkthrough.
- [`CONFIGURATION-GUIDE.md`](file:///c:/Users/MAARA/Desktop/ANIME/ANIMEBRO-SALE-PACKAGE/01-Documentation/CONFIGURATION-GUIDE.md): Master configuration and environment variables.
- [`DATABASE-GUIDE.md`](file:///c:/Users/MAARA/Desktop/ANIME/ANIMEBRO-SALE-PACKAGE/01-Documentation/DATABASE-GUIDE.md): Schema architecture, relationships, and queries.
- [`TVDB-SETUP.md`](file:///c:/Users/MAARA/Desktop/ANIME/ANIMEBRO-SALE-PACKAGE/01-Documentation/TVDB-SETUP.md): TVDB v4 API key acquisition, PIN, and scraping workflows.
- [`VIDEO-PROVIDERS-GUIDE.md`](file:///c:/Users/MAARA/Desktop/ANIME/ANIMEBRO-SALE-PACKAGE/01-Documentation/VIDEO-PROVIDERS-GUIDE.md): StreamHG, StreamRuby, EarnVids setup and upload pipeline.
- [`LANDING-PAGE-GUIDE.md`](file:///c:/Users/MAARA/Desktop/ANIME/ANIMEBRO-SALE-PACKAGE/01-Documentation/LANDING-PAGE-GUIDE.md): Public Landing Page vs. `/home` architecture and Admin editor.
- [`SEO-GUIDE.md`](file:///c:/Users/MAARA/Desktop/ANIME/ANIMEBRO-SALE-PACKAGE/01-Documentation/SEO-GUIDE.md): XML Sitemaps, robots.txt, OpenGraph, and metadata.
- [`FAVICON-PWA-GUIDE.md`](file:///c:/Users/MAARA/Desktop/ANIME/ANIMEBRO-SALE-PACKAGE/01-Documentation/FAVICON-PWA-GUIDE.md): PWA webmanifest, service worker, and icon sets.
- [`URL-ROUTING-GUIDE.md`](file:///c:/Users/MAARA/Desktop/ANIME/ANIMEBRO-SALE-PACKAGE/01-Documentation/URL-ROUTING-GUIDE.md): Clean URL rewrites and Apache `.htaccess` rules.
- [`TROUBLESHOOTING.md`](file:///c:/Users/MAARA/Desktop/ANIME/ANIMEBRO-SALE-PACKAGE/01-Documentation/TROUBLESHOOTING.md): Diagnosis and solutions for common operational issues.
- [`BACKUP-RESTORE-GUIDE.md`](file:///c:/Users/MAARA/Desktop/ANIME/ANIMEBRO-SALE-PACKAGE/01-Documentation/BACKUP-RESTORE-GUIDE.md): Full database, files, and uploads backup procedures.
- [`UPDATE-GUIDE.md`](file:///c:/Users/MAARA/Desktop/ANIME/ANIMEBRO-SALE-PACKAGE/01-Documentation/UPDATE-GUIDE.md): Guidelines for future feature development and safe deployments.
- [`SECURITY-GUIDE.md`](file:///c:/Users/MAARA/Desktop/ANIME/ANIMEBRO-SALE-PACKAGE/01-Documentation/SECURITY-GUIDE.md): Security practices, CSRF protection, and credential rotation.

### 📂 `02-Database/`
- `database-export.sql`: Clean database schema and seed data export.
- `DATABASE-STRUCTURE.md`: Full table-by-table schema reference.
- `DATABASE-IMPORT-GUIDE.md`: Step-by-step SQL import instructions via phpMyAdmin and CLI.

### 📂 `03-Configuration/`
- `config.example.php`: Sanitized configuration template.
- `.env.example`: Environment variables reference.
- `API-KEYS-TEMPLATE.md`: Template for managing external credentials.
- `THIRD-PARTY-SERVICES.md`: Inventory of external APIs and services.

### 📂 `05-Deployment/`
- `HOSTING-REQUIREMENTS.md`, `PHP-REQUIREMENTS.md`, `APACHE-REQUIREMENTS.md`, `DEPLOYMENT-GUIDE.md`, `HTTPS-SETUP.md`.

### 📂 `06-Admin-Reference/`
- `ADMIN-MENU-MAP.md`, `FEATURE-CHECKLIST.md`, `ADMIN-QUICK-REFERENCE.md`.

### 📂 `07-Sale-Handover/`
- `HANDOVER-CHECKLIST.md`, `ASSET-INVENTORY.md`, `THIRD-PARTY-ACCOUNT-CHECKLIST.md`, `OWNERSHIP-TRANSFER-CHECKLIST.md`, `LICENSE-AND-RIGHTS.md`, `SUPPORT-TERMS-TEMPLATE.md`, `FINAL-ACCEPTANCE-CHECKLIST.md`.

### 📂 `08-Testing/`
- `PRE-SALE-TEST-CHECKLIST.md`, `DEPLOYMENT-TEST-CHECKLIST.md`, `FUNCTIONAL-TEST-CHECKLIST.md`.
