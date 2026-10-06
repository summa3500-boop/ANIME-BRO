# SEO & Social Meta Configuration Guide

AnimeBro includes a centralized, high-performance SEO engine (`common/seo_helper.php`) designed to maximize search engine rankings, ensure clean indexing, and generate rich social cards.

---

## 1. Core SEO Features

- **Dynamic Metadata Generation**: Automatic generation of optimized `<title>`, `<meta name="description">`, and keywords for anime, movies, seasons, episodes, categories, and landing pages.
- **Canonical URL Engine**: Generates absolute HTTPS canonical tags (`<link rel="canonical">`), stripping tracking parameters (`?utm_*`, `?fbclid`, `?ref`) to eliminate duplicate content issues.
- **Structured Data (Schema.org JSON-LD)**:
  - `WebSite` & `Organization` on homepage.
  - `TVSeries` on anime/cartoon pages.
  - `Movie` on movie pages.
  - `TVEpisode` & `VideoObject` on watch episode pages.
  - `BreadcrumbList` across all catalog pages.
- **OpenGraph & Twitter Card Tags**: Dynamically outputs `og:title`, `og:description`, `og:image`, `og:url`, `og:type`, `twitter:card` (`summary_large_image`).
- **Dynamic XML Sitemap**: Generated on the fly via `sitemap.php` with automated disk synchronization to `sitemap.xml`.
- **Dynamic Robots.txt**: Served via `robots.php` or static `robots.txt` with admin indexing switches.
- **Search Engine Verification**: Native settings for Google Search Console, Bing Webmaster Tools, Yandex, and Pinterest verification meta tags.

---

## 2. Managing SEO via Admin Panel

Navigate to **Admin Panel -> Settings -> SEO & Analytics** (`/admin/settings.php?tab=seo`):

| Setting Key | Description & Best Practice |
| :--- | :--- |
| `seo_site_title` | Default title suffix (e.g., `AnimeBro - Watch Free HD Anime Online`). |
| `seo_meta_description` | Primary sitewide description (150–160 characters). |
| `seo_meta_keywords` | Comma-separated target keywords. |
| `seo_indexing_mode` | Toggle `index, follow` (Production) or `noindex, nofollow` (Staging/Maintenance). |
| `seo_google_verification` | Google Search Console HTML tag content token. |
| `seo_bing_verification` | Bing Webmaster Tools verification code. |
| `seo_social_default_banner` | Fallback social share image when an anime has no custom backdrop. |
| `seo_twitter_handle` | Platform Twitter/X handle (e.g., `@AnimeBroOfficial`). |

---

## 3. Dynamic XML Sitemaps

The platform provides standards-compliant XML sitemaps following the [Sitemaps.org 0.9 schema](https://www.sitemaps.org/schemas/sitemap/0.9):

- **Master Sitemap URL**: `https://yourdomain.com/sitemap.xml` (or `https://yourdomain.com/sitemap.php?type=all`)
- **Modular Sub-Sitemaps**:
  - `https://yourdomain.com/sitemap.php?type=anime` — All active anime and cartoon titles
  - `https://yourdomain.com/sitemap.php?type=movies` — All active movies
  - `https://yourdomain.com/sitemap.php?type=episodes` — All video watch URLs
  - `https://yourdomain.com/sitemap.php?type=pages` — Static pages (Terms, Privacy, DMCA, Contact)

### Submitting to Google Search Console:
1. Log into [Google Search Console](https://search.google.com/search-console).
2. Go to **Sitemaps** in the sidebar.
3. Enter `sitemap.xml` and click **Submit**.

---

## 4. Robots.txt Configuration

The file `robots.php` (rewritten to `/robots.txt`) outputs directives:

```txt
User-agent: *
Allow: /
Allow: /home
Allow: /anime/
Allow: /cartoon/
Allow: /movie/
Allow: /watch/
Disallow: /admin/
Disallow: /api/
Disallow: /includes/
Disallow: /common/

Sitemap: https://yourdomain.com/sitemap.xml
```

---

## 5. 301/302 Redirect Manager

If you rename an anime slug or restructure URLs:
1. The platform contains a built-in `seo_redirects` database table.
2. In `common/seo_helper.php`, incoming requests are intercepted before route execution.
3. If an old URL matches an entry, an immediate permanent `301 Moved Permanently` header is returned, preserving all inbound search engine link equity.
