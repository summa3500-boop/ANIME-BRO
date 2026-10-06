# AnimeBro - Complete Administrator Guide

> **Official Operating Manual for Website Administrators and Content Managers**

---

## 1. Overview of Admin Panel Architecture

The AnimeBro Admin Panel is protected by server-side session authentication (`require_admin_auth()`) with granular permission gates (`is_super_admin()` and `has_permission()`). It features a unified navigation bar, real-time pending report counters, and quick access shortcuts.

Access URL:
```text
https://yourdomain.com/admin/
```

---

## 2. Navigation Groups & Module Breakdown

### 📊 Group 1: Overview
- **Dashboard (`index.php`):**
  - Displays total Anime, Movies, Cartoons, Episodes, Registered Users, and pending Comment/Report counters.
  - Quick action buttons: **Add Anime**, **Add Movie**, **Landing Page Manager**, **Video Providers**, and **Website Settings**.

---

### 🎬 Group 2: Content Management
- **Anime Management (`anime.php`, `manage_anime.php`):**
  - **Purpose:** Manage series titles, synopsis, genre categories, audio dub languages, posters, and banners.
  - **TVDB Auto-Import:** Enter title to search TVDB API v4 and automatically populate synopsis, release year, poster, and banner.
  - **Continuous Episode Mode:** Toggle continuous episode numbering (e.g. Ep 1 to 500 across seasons) vs. season-relative episode numbering (S01E01).
- **Movie Management (`movies.php`, `manage_movie.php`):**
  - **Purpose:** Manage full-length animated movies with direct multi-server video sources.
  - **Fields:** Title, Category, Language, Rating, Year, Duration, Server 1 & Server 2 embed codes, and Multi-Provider associations.
- **Cartoon Series (`cartoons.php`, `manage_cartoon.php`):**
  - **Purpose:** Dedicated management for western/kids cartoon series separated from Japanese anime.
- **Seasons Management (`seasons.php`):**
  - **Purpose:** Create, edit, and reorder seasons per series title. Supports custom season names and season posters.
- **Episodes Management (`episodes.php`):**
  - **Purpose:** Upload, link, and organize episodes per season.
  - **Multi-Server Streaming:** Configure Server 1 and Server 2 video links, source types (MP4, HLS, Embed), and multi-provider sources.
- **Categories / Genres (`categories.php`):**
  - **Purpose:** Add, edit, and delete anime/movie genre tags (Action, Fantasy, Shounen, Romance, etc.).

---

### 📡 Group 3: Streaming & Cloud Providers
- **Video Providers (`video_providers.php`):**
  - **Purpose:** Connect and manage external video hosting accounts (**StreamHG**, **StreamRuby**, **EarnVids**).
  - **Upload Station:** Multi-file drag-and-drop uploader with real-time progress bars and auto-association to episodes.
  - **Remote URL Uploader:** Transfer videos from external URLs directly into your provider storage.
  - **Connection Tester:** Verify API key validity and account status directly from the interface.
- **Server 2 Promo Banner (`server2_promo.php`):**
  - **Purpose:** Configure interactive promotional banners displayed above Server 2 video players.

---

### 🌐 Group 4: Website Configuration
- **Landing Page Manager (`landing_page.php`):**
  - **Purpose:** Full visual editor for the front-door landing page (`/`).
  - **Tabs:** Global & Hero, About, Features, Content Showcases, Languages & How It Works, FAQ & Final CTA, Community & Socials, and Section Order/Toggles.
- **Hero Banner Slider (`hero_banners.php`):**
  - **Purpose:** Manage the top carousel featured slider on the streaming homepage (`/home`).
- **Custom Pages (`pages.php`):**
  - **Purpose:** Edit static informational pages: Terms of Service, Privacy Policy, DMCA Disclaimer, About Us, and Contact.
- **Language / Dub Settings (`language_settings.php`):**
  - **Purpose:** Configure available audio dubs (Tamil, Hindi, English, Malayalam, Telugu, Multi-Audio) and their banner badges.
- **SEO Management (`seo.php`):**
  - **Purpose:** Configure global meta tags, site verification tokens (Google, Bing, Yandex), and generate XML sitemaps.

---

### 👥 Group 5: Community & Moderation
- **Comments (`comments.php`):**
  - **Purpose:** Moderate user comments across all episodes and movies. Approve, reject, or delete comments.
- **Broken Link Reports (`reports.php`):**
  - **Purpose:** Review user-submitted playback issue and dead link reports with direct links to edit the affected episode.
- **Users (`users.php`):**
  - **Purpose:** Manage registered user accounts, toggle user statuses (Active/Blocked), and reset passwords.
- **Avatar Library (`avatars.php`):**
  - **Purpose:** Upload and curate anime profile avatars available for user profiles.

---

### ⚙️ Group 6: System Administration
- **Staff Management (`staff.php`, `manage_staff.php`):**
  - **Purpose:** Create administrative staff accounts with customized role permissions (Super Admin, Editor, Moderator).
- **Import / Export (`import_export.php`):**
  - **Purpose:** Download JSON/SQL backups of site content and import bulk datasets.
- **Settings (`settings.php`):**
  - **Purpose:** Master platform configuration: Site Name, Tagline, **Website Logo**, TVDB v4 credentials, Provider keys, Maintenance Mode toggle, and license management.

---

## 3. Best Practices for Administrators

1. **Website Logo:** Always upload your official website logo from **Admin Settings &rarr; Website Logo**. The system automatically syncs this logo across both the main streaming portal and the Landing Page.
2. **Metadata Accuracy:** Use TVDB auto-search to ensure high-resolution posters and accurate season/episode ordering.
3. **Multi-Server Redundancy:** Always configure at least two streaming servers (e.g., Server 1 StreamHG, Server 2 StreamRuby/Embed) to guarantee zero downtime if an external video provider undergoes maintenance.
4. **Routine Backups:** Perform weekly database exports from **System &rarr; Import / Export** or phpMyAdmin.
