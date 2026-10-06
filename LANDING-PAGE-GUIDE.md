# Landing Page Management & Customization Guide

This document explains the dual-page architecture, public landing page system, and full admin visual management suite for AnimeBro.

---

## 1. Dual-Page Architecture

AnimeBro features an enterprise dual-entry routing system:

1. **Public Landing Page (`/` / `landing_page.php`)**:
   - The primary visitor-facing promotional homepage.
   - High-converting hero banner, value propositions, feature highlights, content showcases, multi-language support preview, how-it-works guide, FAQ accordion, community links, and CTA banners.
   - Zero admin controls or edit forms are visible to public visitors.
   - Fully responsive, SEO-optimized with OpenGraph tags, JSON-LD schema, and fast-loading vanilla CSS/JS.

2. **Main Streaming Platform (`/home` / `index.php`)**:
   - The interactive content catalog, video streaming portal, search/filter discovery, watchlist, user profiles, and episode player.

3. **Admin Landing Page Editor (`/admin/landing_page.php`)**:
   - A dedicated, authenticated, role-protected management dashboard where site administrators can customize every section of the public landing page in real time.

---

## 2. Dynamic Website Logo Synchronization

> **Important Architecture Rule:**  
> The Landing Page header logo **does not** use a separate upload or disconnected configuration. It dynamically syncs with the primary website logo configured in:  
> **Admin Panel -> Settings -> General Settings -> Website Logo** (`site_logo`).

When you change the Website Logo in **Settings**, both the streaming platform (`/home`) and the public landing page (`/`) update instantly.

---

## 3. The 8 Editor Tabs Walkthrough

Access the editor via **Admin Panel -> Website Management -> Landing Page** (`/admin/landing_page.php`).

```
┌────────────────────────────────────────────────────────────────────────┐
│                      Landing Page Admin Editor                         │
├─────────┬─────────┬──────────┬───────────┬────────────┬───────┬────────┤
│ 1.Hero  │ 2.About │ 3.Feat.  │ 4.Showcase│ 5.Steps/Lng│ 6.FAQ │ 7.Soc. │
└─────────┴─────────┴──────────┴───────────┴────────────┴───────┴────────┘
```

### Tab 1: Global & Hero Settings
- **Landing Page Status**: Toggle Landing Page active or direct bypass to `/home`.
- **Hero Badge Text**: Pill badge above main heading (e.g., `⚡ #1 Streaming Platform`).
- **Hero Main Heading**: Bold hero headline with gradient text support.
- **Hero Subtitle & Description**: Lead paragraph detailing library size and quality.
- **Primary CTA Button**: Text (e.g., `Start Watching Anime`) and URL (`/home` or custom).
- **Secondary CTA Button**: Text (e.g., `Explore Cartoons`) and URL.
- **Stats Counters**: Total anime count, HD episodes, active members, daily updates.

### Tab 2: About Section
- **About Badge & Heading**: Section title and sub-heading.
- **About Description**: Extended copy introducing your streaming platform.
- **4 Value Highlights**: Fast streaming speed, zero intrusive ads, multi-language audio/subs, cross-device compatibility.

### Tab 3: Features Manager
- Add, edit, reorder, or delete feature highlight cards.
- Customizable icons (FontAwesome classes or SVG), title, description, and badge tags.

### Tab 4: Content Showcases
- Configure showcase carousels for **Anime**, **Cartoons**, and **Movies**.
- Pick spotlight titles, custom poster cards, rating badges, and direct links.

### Tab 5: Languages & How It Works
- **Supported Languages**: Add language tags (English Sub, English Dub, Japanese, Spanish, Hindi, etc.) with custom color badges.
- **How It Works Steps**: 3-step or 4-step user onboarding flow (e.g., `1. Search Title -> 2. Select Episode -> 3. Stream in HD`).

### Tab 6: FAQs & Final CTA
- **FAQ Accordion**: Add frequently asked questions with rich HTML answers.
- **Final Call to Action**: Full-width bottom banner with conversion headline, background styling, and action button.

### Tab 7: Community & Social Links
- Discord invite link and online member counter.
- Telegram channel/group link and subscriber counter.
- Official Twitter/X, Instagram, and Reddit links.

### Tab 8: Section Visibility & Ordering
- Enable or disable any individual section with a single toggle switch without deleting content.

---

## 4. Saving & Live Verification

1. When editing any tab, click **Save Changes** at the bottom of the section.
2. The system validates inputs server-side, saves to the database (`settings`, `landing_features`, `landing_steps`, `landing_faqs`, `landing_socials`, `landing_showcase`), and renders a confirmation notification.
3. Click the **Preview Landing Page** button in the header bar to view your live changes at `/` in a new tab.
