# Multi-Vite React App with Docker and Nginx

This project sets up **three Vite-based React applications** (`app1`, `app2`, and `app3`) in **Docker containers**, served behind a central **Nginx reverse proxy**.

Each app is available via a custom local domain:

- http://app1.local
- http://app2.local
- http://app3.local

---

## Project Structure

```
docker-react-multi-apps/
├── app1/            # Vite React app 1
│   └── Dockerfile
├── app2/            # Vite React app 2
│   └── Dockerfile
├── app3/            # Vite React app 3
│   └── Dockerfile
├── nginx/
│   └── default.conf # Nginx reverse proxy config
├── docker-compose.yml
└── README.md
```

---

## Prerequisites

- [Docker Desktop](https://www.docker.com/products/docker-desktop)
- [Node.js](https://nodejs.org/) (only for developing apps locally)
- Add domains to your system’s `hosts` file

---

## Step 1: Add Local Domains

**Edit your hosts file:**

- **Windows:** `C:\Windows\System32\drivers\etc\hosts`
- **macOS/Linux:** `/etc/hosts`

Add:

```
127.0.0.1 app1.local
127.0.0.1 app2.local
127.0.0.1 app3.local
```

---

## Step 2: Create or Replace Vite Apps

If you want to replace the Vite apps, delete all contents except **Dockerfile** inside `app1`, `app2`, and `app3` respectively. Do not delete any **Dockerfile** in any of those folders.

To replace the Vite apps with TypeScript Vite React apps, do the following commands:

```bash
cd app1
npm create vite@latest . -- --template react-ts
npm install
npm run build
cd ..

cd app2
npm create vite@latest . -- --template react-ts
npm install
npm run build
cd ..

cd app3
npm create vite@latest . -- --template react-ts
npm install
npm run build
cd ..
```

Or use your own Vite/React apps.

---

## Step 3: Build and Start All Containers

From the root folder:

```bash
docker-compose up --build
```

Docker will:

- Build and bundle your React apps
- Start each app container with Nginx serving static files
- Start a reverse proxy container that routes traffic based on domain

---

## Step 4: Open in Browser

Visit each app in your browser:

- http://app1.local
- http://app2.local
- http://app3.local

---

## Cleanup

To stop the containers:

```bash
docker-compose down
```

---

## Troubleshooting

- Make sure Docker Desktop is running
- Recheck your `hosts` file for typos
- Use `docker-compose logs nginx` to debug reverse proxy
- If you change any Nginx config, restart with:

```bash
docker-compose down
docker-compose up --build
```

---

## Notes

- The apps are built with `npm run build` and served via **Nginx**, not `npm run dev`
- You can modify each app individually and rebuild using Docker
- Nginx reverse proxy is configured in `nginx/default.conf`
