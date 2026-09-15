# Gym & Shop Website Template — Setup Guide

A single-file HTML template for gyms, fitness studios, and fitness product shops. Includes an online store with cart, UPI/card payments, and a built-in admin panel — no separate backend server required.

## Features

- **Home page** — hero section, stats bar, class listing, trainer profiles, membership plans
- **Shop** — product grid, cart drawer with quantity controls, categories (apparel / supplements)
- **Checkout** — Razorpay integration (UPI, Cards, Netbanking, Wallets)
- **Profile** — user info, refer & earn section
- **Admin panel** — add/edit/delete classes, trainers, products, and custom pages; manage/update order status
- **Custom pages** — create new pages/tabs from the admin panel without touching code
- **Responsive** — mobile-first design, works on all screen sizes

## What you need before setup

1. A [Supabase](https://supabase.com) account (free tier works) — this is your database
2. A [Razorpay](https://razorpay.com) account (for accepting payments — requires KYC)
3. Basic ability to edit a text file and host static HTML (Netlify, Vercel, GitHub Pages, or any web host)

## Setup Steps

### 1. Create your Supabase project
- Go to supabase.com → New Project
- Once created, go to **Project Settings → API** and copy your:
  - `Project URL`
  - `anon public` key (this is safe to use in frontend code — it is NOT the secret key)

### 2. Create the database tables
In Supabase, go to the **SQL Editor** and create tables for:
- `classes` (name, description, level, trainer, video)
- `trainers` (name, role, bio)
- `products` (name, description, price, image_emoji, category, stock)
- `custom_pages` (slug, title, icon, content, show_in_tabs, sort_order)
- `orders` (customer info, items, amount, status)

> Tip: enable Row Level Security (RLS) on these tables and set policies so only your admin login can insert/update/delete, while the public can only read.

### 3. Connect Supabase to the template
Open `index.html`, find where the Supabase client is initialized, and replace with your own:
```js
const sb = supabase.createClient('YOUR_PROJECT_URL', 'YOUR_ANON_KEY');
```

### 4. Connect Razorpay
- Sign up at razorpay.com, complete KYC
- Go to **Settings → API Keys**, generate keys
- Replace the `key` field in the checkout options with your **Key ID** (public, safe for frontend)
- **Important:** Order creation and payment verification must happen on a small backend/server (Razorpay does not allow pure frontend-only payment flows for security). Use a serverless function (Supabase Edge Functions, Vercel Functions, or any Node/PHP backend) to:
  - Create the order before opening checkout
  - Verify the payment signature after success
- Never expose your Razorpay **Key Secret** anywhere in frontend code.

### 5. Customize branding
- Replace "IRONFORM" text throughout with your business name
- Update colors in the `:root` CSS variables at the top of the `<style>` block
- Replace Google Fonts links if you want different fonts (currently uses Anton + Inter, both free for commercial use)

### 6. Add your content
Use the admin panel in the live site (or directly in Supabase table editor) to add:
- Your classes/exercises
- Your trainers
- Your products
- Any custom pages you want as extra tabs

### 7. Deploy
Upload `index.html` to any static host: Netlify, Vercel, GitHub Pages, or your own hosting.

## License / Usage Terms
*(Fill this in before selling — e.g. "Single-site use license — one license per live deployed website. Resale or redistribution of the raw source code as a template is not permitted.")*

## Support
*(Add your contact/support policy here — e.g. email support for 30 days post-purchase, paid customization available.)*
