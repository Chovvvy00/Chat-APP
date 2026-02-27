# Chat App Setup Guide

This README is for installing and running the full app from the project root.

## Project folders

- `Backend` = Laravel API + Reverb websocket server
- `Front End` = Vue 3 + Vite client app

## What to install first

Install these on your machine before setup:

1. Node.js `20.19+` (or `22.12+`) and npm
2. PHP `8.2+`
3. Composer `2+`
4. MySQL (or MariaDB)
5. (Recommended) Git

PHP should include common Laravel extensions (especially `pdo_mysql`, `mbstring`, `openssl`, `xml`, `ctype`, `json`, `tokenizer`, `fileinfo`).

## 1 Backend setup (Laravel)

Open a terminal in `Backend`:

```powershell
cd Backend
composer install
npm install
Copy-Item .env.example .env
```

Edit `Backend/.env` and set your database values:

- `DB_CONNECTION=mysql`
- `DB_HOST=127.0.0.1`
- `DB_PORT=3306`
- `DB_DATABASE=laravel_chat` (or your DB name)
- `DB_USERNAME=...`
- `DB_PASSWORD=...`

Create the database in MySQL, then run:

```powershell
php artisan key:generate
php artisan migrate
php artisan storage:link
```

Start backend services:

```powershell
composer run dev
```

This starts:

- Laravel API on `http://localhost:8000`
- Laravel Reverb on `http://localhost:8080`
- Queue worker
- Vite for backend assets

## 2 Frontend setup (Vue)

Open a second terminal in `Front End`:

```powershell
cd "Front End"
npm install
Copy-Item .env.example .env
```

Check `Front End/.env`:

- `VITE_API_URL=http://localhost:8000`
- `VITE_REVERB_APP_KEY=local`
- `VITE_REVERB_HOST=localhost`
- `VITE_REVERB_PORT=8080`
- `VITE_REVERB_SCHEME=http`

Start frontend:

```powershell
npm run dev
```

Vite will print the local URL (usually `http://localhost:5173`).

## Run order (quick version)

1. Start backend with `composer run dev` inside `Backend`
2. Start frontend with `npm run dev` inside `Front End`
3. Open the frontend URL from Vite in your browser

## Common issues

- `SQLSTATE...` errors: DB name/user/password in `Backend/.env` is wrong, or MySQL is not running.
- Frontend cannot reach API: check `VITE_API_URL` and confirm backend is on port `8000`.
- Realtime/chat updates not live: make sure Reverb is running on port `8080`.
