# NaxAI Monitor - Deployment Guide

## Current Deployment

**Self-hosted on the NaxAI server**

- **URL:** `http://localhost:3001` (internal)
- **Status:** Running (first-run setup required)
- **Process:** `node server/server.js` (PID on server)

## Quick Start

```bash
# Clone
git clone https://github.com/RobGenins/naxai-monitor.git
cd naxai-monitor

# Install & run
npm install --omit=dev --no-audit
npm run download-dist
node server/server.js
```

Server starts on `http://localhost:3001`. Complete the setup wizard on first visit.

## Docker (using upstream image)

```yaml
# compose.yaml
services:
  naxai-monitor:
    image: louislam/uptime-kuma:2
    restart: unless-stopped
    volumes:
      - ./data:/app/data
    ports:
      - "3001:3001"
```

## Render Deployment

1. Go to [Render Dashboard](https://dashboard.render.com/)
2. Click **New +** → **Web Service**
3. Connect the `RobGenins/naxai-monitor` repo
4. Configure:
   - **Name:** naxai-monitor
   - **Runtime:** Node
   - **Build Command:** `npm install && npm run download-dist`
   - **Start Command:** `node server/server.js`
   - **Plan:** Free
5. Add a **Persistent Disk** at `/opt/render/project/src/data` (required for SQLite)
6. Click **Create Web Service**

## Creating a Public Status Page

This is the key demo feature. Status pages let you share monitor status publicly without requiring login.

### Steps:

1. **Complete initial setup** at `http://localhost:3001` (create admin account, configure database)
2. **Add monitors** - go to **Add Monitor** and configure the services you want to track
3. **Create a Status Page:**
   - Navigate to **Status Pages** in the sidebar
   - Click **New Status Page**
   - Give it a **Name** and **Slug** (e.g., `naxai-status`)
   - Select which monitors to display
   - Customize the description, logo, and theme
   - Toggle **Published** to ON
4. **Access the public status page:**
   - Default: `http://localhost:3001/status`
   - Custom slug: `http://localhost:3001/status/naxai-status`
   - RSS feed: `http://localhost:3001/status/naxai-status/rss`

### Status Page Features:
- Public URL (no login required)
- Custom domain mapping (e.g., `status.naxai.us`)
- Custom branding (logo, colors, description)
- Incident announcements
- Monitor status overview (UP/DOWN/MAINTENANCE)
- 90-day uptime percentage badges
- RSS feed for subscribers
- Embeddable status badges

### Custom Domain Setup:
1. In Status Page settings, set the **Domain** field (e.g., `status.naxai.us`)
2. Create a CNAME DNS record pointing to your NaxAI Monitor server
3. The status page will be served at that domain

## Rebranding Notes

This is a fork of [Uptime Kuma](https://github.com/louislam/uptime-kuma) v2.3.2 by Louis Lam.
Changes made:
- All user-facing "Uptime Kuma" text → "NaxAI Monitor"
- Package name, repo URL updated
- Page title, manifest, and About dialog rebranded
- Server logs and PM2 config updated
