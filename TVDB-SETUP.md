# TheTVDB API v4 Integration & Setup Guide

This guide provides complete instructions for obtaining, configuring, testing, and troubleshooting **TheTVDB API v4** integration within the AnimeBro platform.

---

## 1. Overview & Capabilities

AnimeBro utilizes **TheTVDB API v4** (`https://api4.thetvdb.com/v4`) through a centralized service layer (`common/tvdb_service.php` / `AnimeBro_TVDB`). This powers automatic scraping, metadata population, artwork retrieval, and bulk imports for:

- **Anime Series** (Title, Japanese title, synopsis, status, release year, genres, studio, rating, poster, banner/fanart)
- **Cartoons & Animation** (Western animation, episodic TV metadata)
- **Movies** (Feature films, theatrical releases, runtimes, cast, artwork)
- **Seasons** (Official TVDB season numbering, names, poster artwork)
- **Episodes** (Episode numbering, absolute order, episode titles, overviews, air dates, thumbnail screenshots)
- **Artwork CDN Auto-Resolution** (Automatically resolves relative paths to `https://artworks.thetvdb.com/`)

---

## 2. How to Obtain TVDB API Credentials

1. **Create an Account**: Navigate to [https://thetvdb.com](https://thetvdb.com) and create or log in to your account.
2. **Subscription / API Access**: TheTVDB v4 API requires an active subscription or developer tier. Check your account settings under **API Keys**.
3. **Generate API Key**:
   - Go to your account profile -> **API Keys**.
   - Create a new project / API Key (e.g., `AnimeBro Platform`).
   - Copy the generated **API Key** (e.g., a UUID / alphanumeric string).
4. **User Subscriber PIN (Optional / Recommended)**:
   - If using a user-linked subscription, copy your personal **Subscriber PIN** from your TVDB dashboard.

---

## 3. Configuring TVDB in Admin Panel

1. Log into your AnimeBro Admin Panel (`https://yourdomain.com/admin/login.php`).
2. Navigate to **System Settings** -> **API Integrations** (`/admin/settings.php?tab=apis`).
3. Locate the **TheTVDB API Configuration** card:
   - **TVDB API Key (v4)**: Paste your TVDB API key (`tvdb_api_key`).
   - **TVDB User PIN (Optional)**: Paste your PIN if applicable (`tvdb_pin`).
4. Click **Test TVDB Connection**:
   - The platform sends a live JWT authentication handshake to `https://api4.thetvdb.com/v4/login`.
   - On success, it displays `Connection Successful! JWT Token generated.` and caches the token.
5. Click **Save Settings**.

---

## 4. How the Auto-Scraper Works

### A. Single Anime / Movie Auto-Import
- In **Admin -> Add Anime** or **Admin -> Add Movie**, enter the series/movie title or TVDB ID.
- Click **Fetch from TVDB**.
- The system fetches the primary metadata, synopsis, status, genres, studio, and artwork, pre-filling all input fields instantly.

### B. Bulk Season & Episode Scraper
- In **Admin -> Manage Episodes -> TVDB Bulk Importer**, select the Anime and enter its TVDB ID.
- Click **Fetch Seasons & Episodes**.
- Choose the target season and import all episode titles, descriptions, and thumbnail stills in a single click.

---

## 5. Token Caching & Automatic Refresh

The platform minimizes API quota consumption by caching JWT authentication tokens in the `settings` database table:
- `tvdb_token`: The active JWT Bearer token.
- `tvdb_token_expires_at`: UNIX timestamp when the token expires (typically 30 days).
- **Auto-Refresh**: If the cached token is within 24 hours of expiry, `AnimeBro_TVDB::authenticate()` automatically requests a fresh token and updates the database transparently.

---

## 6. Troubleshooting TVDB Issues

| Issue / Symptom | Possible Cause | Resolution |
| :--- | :--- | :--- |
| `TVDB API Key is not configured` | Settings key is blank. | Enter API key in Admin Settings -> API Integrations. |
| `HTTP 401 Unauthorized` | Invalid key or expired subscription. | Verify credentials on thetvdb.com and re-enter in settings. |
| `cURL error: Operation timed out` | Server cannot reach `api4.thetvdb.com`. | Check outbound firewall/ports 80 & 443 on your web host. |
| `No results found` | Search query too specific or misspelled. | Search by exact TheTVDB ID number (e.g., `81797` for One Piece). |
| Image thumbnails broken | CDN path malformed. | Ensure `normalizeArtworkUrl()` helper is active; images use `https://artworks.thetvdb.com/`. |
