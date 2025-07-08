Here is a neatly formatted README.md section that includes build and deployment instructions for:

✅ Vendure Backend

✅ Qwik Storefront (Cloudflare Pages)

✅ Nginx Reverse Proxy Configuration (for backend if needed)

You can directly copy-paste the following into your README.md file:

markdown
Copy
Edit
# 🛒 RealMaya Commerce - Build & Deployment Guide

This guide provides complete, step-by-step instructions to build and deploy both the **Vendure backend** and the **Qwik Storefront frontend**, with Cloudflare Pages and optional Nginx reverse proxy configuration.

---

## 🧱 1. Vendure Backend (Node + GraphQL API)

### 📦 Install Dependencies
```bash
cd backend-vendure
npm install
🛠️ Build Backend
bash
Copy
Edit
npm run build
🚀 Start Backend (Development Mode)
bash
Copy
Edit
npm run start
🌐 Start Backend (Production Mode)
bash
Copy
Edit
npm run start:prod
📁 Environment Configuration (example .env)
ini
Copy
Edit
DATABASE_URL=postgres://username:password@localhost:5432/realmaya
PORT=3000
API_HOST=localhost
CORS_ORIGIN=http://localhost:5173,http://localhost:3000
SMTP_HOST=smtp.serversmtp.com
SMTP_PORT=587
SMTP_USER=your@domain.com
SMTP_PASS=yourpassword
🌐 2. Qwik Storefront (Cloudflare Pages)
⚙️ Install Dependencies
bash
Copy
Edit
cd storefront-qwik
npm install
🔨 Build for Production
bash
Copy
Edit
npm run build
🧪 Preview Locally (Optional)
bash
Copy
Edit
npm run start.prod
☁️ Cloudflare Pages Deployment
✅ Prerequisites:
Cloudflare account: https://dash.cloudflare.com/

GitHub repo connected to Cloudflare Pages

Select "None" as the framework preset

Set build command and output as below:

⚙️ Cloudflare Pages Configuration:
yaml
Copy
Edit
Build Command: npm run build
Output Folder: dist
Root Directory: /
🌍 Custom Domain Setup
In Cloudflare Pages → Custom Domains → Add Domain (e.g., www.realmaya.com)

DNS → Point your domain/subdomain (via CNAME or A record if not proxied)

Keep DNS managed in GoDaddy, but nameservers can remain as-is unless you plan full Cloudflare DNS migration.

🔁 3. Nginx Reverse Proxy (Optional for Backend)
If you're serving the backend from a VM (e.g., Ubuntu server), here is a sample NGINX reverse proxy config:

📄 /etc/nginx/sites-available/realmaya-backend.conf
nginx
Copy
Edit
server {
    listen 80;
    server_name api.realmaya.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
}
🔗 Enable the config:
bash
Copy
Edit
sudo ln -s /etc/nginx/sites-available/realmaya-backend.conf /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
✅ Deployment Notes
Vendure backend runs on a VM (Ubuntu recommended) with public access.

Frontend is deployed to Cloudflare Pages with automatic HTTPS and CDN.

Custom domain is managed via GoDaddy (you may later migrate DNS to Cloudflare for better speed & security).

SMTP settings are configured via .env for turboSMTP.

Ensure your PostgreSQL DB is up and exposed securely to the Vendure app.
