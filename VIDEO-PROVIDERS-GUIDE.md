# Video Providers & Upload Engine Guide

This document outlines the multi-provider video hosting architecture, API setups, automated upload pipelines, and streaming player configurations supported in AnimeBro.

---

## 1. Supported Video Providers

AnimeBro natively supports three high-performance third-party video storage/streaming platforms, as well as direct iframe/embed links:

1. **StreamHG** (`https://streamhg.com` / `https://streamhgapi.com/api/`)
2. **StreamRuby** (`https://streamruby.com` / `https://streamruby.com/api/`)
3. **EarnVids** (`https://earnvids.com` / `https://earnvidsapi.com/api/`)
4. **Direct URL / Custom Embeds** (Direct MP4 / M3U8 HLS / Iframe embeds from any host)

---

## 2. API Key Configuration

To configure video provider API keys:

1. Log in to the AnimeBro Admin Panel (`/admin/login.php`).
2. Go to **Settings** -> **Video Providers** (`/admin/settings.php?tab=providers`).
3. Fill in the API keys for each provider:
   - `streamhg_api_key`
   - `streamruby_api_key`
   - `earnvids_api_key`
4. Set default provider priority order and enabled/disabled states.
5. Click **Test Provider Connection** on any provider to verify the API key with the provider's server.
6. Click **Save Settings**.

---

## 3. Video Uploading Workflows

AnimeBro provides two streamlined workflows for adding video episodes and movies:

### Workflow A: Direct Local File Upload via Admin Panel
1. Go to **Admin** -> **Add Episode** or **Add Movie**.
2. Select the video file (.mp4, .mkv, .webm) from your computer.
3. Select which providers you want to upload to (e.g., StreamHG and StreamRuby simultaneously).
4. The system executes:
   - **Video Header Probing**: Extracts exact runtime/duration and resolution without requiring external ffmpeg binaries.
   - **Upload Server Discovery**: Queries provider API (`/upload/server`) for active upload node.
   - **Multipart Stream Upload**: Securely streams the file to the remote video host.
   - **Metadata Extraction**: Captures `file_code`, `embed_url`, direct stream link, and poster thumbnail.
   - **Database Storage**: Creates records in `episode_providers` / `movie_providers`.

### Workflow B: Remote URL Upload (Link Import)
1. In the episode/movie editor, paste a direct download URL or remote video link.
2. Select target providers.
3. The server instructs the provider's remote upload API to ingest the video directly without consuming your server's bandwidth.

### Workflow C: Manual Embed / Direct Link Entry
1. If you host video externally (e.g., Google Drive proxy, BunnyCDN, custom embed iframe, VidCloud, etc.), you can directly paste the iframe embed code or video URL into the server slot.

---

## 4. Frontend Player & Server Switching

On the watch page (`/watch/anime-slug/season-1/episode-1` or `/movie/movie-slug`):
- The platform renders a sleek video player with multiple server tabs (e.g., **Server 1: StreamHG**, **Server 2: StreamRuby**, **Server 3: EarnVids**, **Server 4: Direct HD**).
- If a user encounters buffering on one server, they can switch to another server seamlessly.
- Player settings (autoplay, auto-next episode, light/dark theater mode, auto-skip intro) persist via client-side localStorage.

---

## 5. Security & Bandwidth Optimization

- **Zero API Key Leakage**: Video provider API keys are never outputted in HTML, JS scripts, or network responses.
- **Server Offloading**: Video streams are delivered directly by the provider's CDN to the viewer, preserving your host's bandwidth and CPU.
- **Upload Timeout Tuning**: Large files (1GB+) require appropriate `upload_max_filesize`, `post_max_size`, and `max_execution_time` in your server's `php.ini`. See [PHP-REQUIREMENTS.md](../05-Deployment/PHP-REQUIREMENTS.md).
