# Stroopwafel Huis — Technical Specification

> Based on analysis of Ai-Whisperers repos: clinica-duerksen (Next.js 15 + Tailwind + Supabase)
> and fun4me-store (Next.js + Supabase e-commerce). Using proven patterns from our own codebase.

---

## DECISION: Next.js 15 (NOT Shopify)

After analyzing our existing repos, we should build custom with Next.js 15 — same stack
as clinica-duerksen and fun4me-store. Reasons:

1. **We already have the patterns** — hero, footer, header, product cards, WhatsApp, analytics, SEO JSON-LD
2. **Supabase** for products/orders/CMS (we already use it)
3. **Full control** over design (cottage core needs custom CSS, not Shopify themes)
4. **No monthly platform fees** ($39/mo Shopify saved)
5. **Deploy on our own Hostinger VPS** via Docker + Traefik (already set up)
6. **Consistent with our portfolio** — same stack across all projects

---

## TECH STACK

| Layer | Technology | Notes |
|-------|-----------|-------|
| Framework | Next.js 15 (App Router) | Same as clinica-duerksen |
| Language | TypeScript (strict) | Same conventions |
| Styling | Tailwind CSS + shadcn/ui | Cottage core theme |
| Database | Supabase (PostgreSQL) | Products, orders, CMS |
| Fonts | Cormorant Garamond + Lora + DM Sans | Google Fonts |
| Icons | Lucide React | Same as other projects |
| Animations | CSS (fade-in-up pattern from clinica) | Keep it lightweight |
| WhatsApp | Float button (pattern from both repos) | Essential for Paraguay |
| SEO | JSON-LD (LocalBusiness, Restaurant, Product) | Pattern from clinica |
| Analytics | Google Analytics 4 | Component from clinica |
| Payments | Bancard Paraguay / Tigo Money QR | Local payment |
| Email | Resend | For orders & newsletter |
| Deploy | Docker + Traefik on Hostinger VPS | Existing infrastructure |
| Domain | stroopwafelhuis.com.py | Paraguay domain |

---

## PROJECT STRUCTURE

```
stroopwafel-huis-web/
├── app/
│   ├── layout.tsx              # Root layout (fonts, meta, header/footer)
│   ├── page.tsx                # Home page (10 sections)
│   ├── globals.css             # Tailwind + cottage core theme
│   ├── robots.ts               # SEO robots
│   ├── sitemap.ts              # Dynamic sitemap
│   ├── menu/page.tsx           # Visual menu
│   ├── nuestra-historia/page.tsx  # Our story
│   ├── que-es-un-stroopwafel/page.tsx  # SEO article
│   ├── stroopwafel-de-la-semana/page.tsx  # Weekly flavor (Crumbl model!)
│   ├── ubicacion/page.tsx      # Location + map
│   ├── catering/page.tsx       # Corporate/events
│   ├── contacto/page.tsx       # Contact form
│   ├── tienda/                 # E-commerce
│   │   ├── page.tsx            # Shop listing
│   │   └── [slug]/page.tsx     # Product detail
│   ├── blog/
│   │   ├── page.tsx            # Blog listing
│   │   └── [slug]/page.tsx     # Blog post
│   ├── rewards/page.tsx        # Club Lekker loyalty
│   ├── privacidad/page.tsx     # Privacy policy
│   └── admin/                  # Admin CMS
│       ├── layout.tsx
│       ├── page.tsx            # Dashboard
│       ├── productos/page.tsx  # Manage products
│       ├── pedidos/page.tsx    # Manage orders
│       ├── sabor-semana/page.tsx # Weekly flavor manager
│       └── blog/page.tsx       # Blog CMS
├── components/
│   ├── home/
│   │   ├── hero.tsx            # Video/photo hero with CTAs
│   │   ├── marquee.tsx         # Scrolling text marquee
│   │   ├── weekly-flavor.tsx   # Stroopwafel de la Semana section
│   │   ├── explainer.tsx       # "What is a Stroopwafel?" with illustration
│   │   ├── favorites.tsx       # Product slider
│   │   ├── founder-story.tsx   # Bram & Rach story
│   │   ├── reviews.tsx         # Social proof carousel
│   │   ├── instagram-feed.tsx  # IG embed
│   │   └── location-preview.tsx # Map + hours preview
│   ├── menu/
│   │   ├── menu-tabs.tsx       # Tab navigation for menu categories
│   │   ├── menu-item.tsx       # Individual item card
│   │   └── combo-card.tsx      # Combo highlight card
│   ├── store/
│   │   ├── product-card.tsx    # E-commerce product card (from fun4me pattern)
│   │   ├── cart-drawer.tsx     # Side cart (from fun4me)
│   │   ├── announcement-bar.tsx # Promo banner (from fun4me)
│   │   └── newsletter.tsx      # Email signup (from fun4me)
│   ├── layout/
│   │   ├── header.tsx          # Navigation (from clinica pattern)
│   │   ├── footer.tsx          # Footer (from clinica pattern)
│   │   ├── whatsapp-button.tsx # Float WhatsApp (from both repos)
│   │   ├── cookie-consent.tsx  # GDPR banner (from clinica)
│   │   └── breadcrumb.tsx      # Breadcrumb (from clinica)
│   ├── seo/
│   │   ├── json-ld.tsx         # Structured data (from clinica)
│   │   └── restaurant-json-ld.tsx # Restaurant schema
│   ├── analytics/
│   │   └── google-analytics.tsx # GA4 (from clinica)
│   └── ui/                     # shadcn/ui primitives
│       ├── button.tsx
│       ├── card.tsx
│       ├── badge.tsx
│       ├── input.tsx
│       ├── textarea.tsx
│       ├── select.tsx
│       ├── dialog.tsx
│       ├── sheet.tsx
│       ├── separator.tsx
│       └── accordion.tsx
├── lib/
│   ├── constants.ts            # Business info, brand constants
│   ├── supabase.ts             # Supabase client
│   ├── utils.ts                # cn(), formatPrice()
│   └── store/
│       └── cart.ts             # Cart state (zustand, from fun4me)
├── types/
│   └── database.ts             # Supabase types
├── public/
│   ├── images/                 # Optimized photos
│   ├── icons/                  # Favicon, app icons
│   └── fonts/                  # If self-hosting fonts
├── supabase/
│   └── migrations/             # DB schema
├── messages/                   # i18n (future: es, en, nl)
├── Dockerfile
├── docker-compose.yml
├── .env.example
├── tailwind.config.ts
├── next.config.ts
├── package.json
└── tsconfig.json
```

---

## CONSTANTS (lib/constants.ts)

```typescript
export const BUSINESS = {
  name: "Stroopwafel Huis",
  tagline: "Verse Stroopwafels & Specialty Coffee",
  taglineEs: "Stroopwafels Frescos & Café de Especialidad",

  // Contact
  phone: "+595 xxx xxx xxx",
  phoneRaw: "+595xxxxxxxxx",
  whatsapp: "https://wa.me/595xxxxxxxxx",
  email: "hola@stroopwafelhuis.com.py",

  // Location
  address: {
    street: "", // TBD
    neighborhood: "Villa Morra",
    city: "Asunción",
    state: "Central",
    country: "Paraguay",
    zip: "",
  },

  // Online
  website: "https://stroopwafelhuis.com.py",
  socials: {
    instagram: "https://instagram.com/stroopwafelhuis",
    tiktok: "https://tiktok.com/@stroopwafelhuis",
    facebook: "https://facebook.com/stroopwafelhuis",
  },

  // Hours
  hours: {
    weekdays: { open: "08:00", close: "20:00" },
    saturday: { open: "08:00", close: "20:00" },
    sunday: { open: "09:00", close: "18:00" },
  },
  hoursFormatted: "Lun-Sáb: 8:00-20:00 · Dom: 9:00-18:00",

  // PedidosYa
  pedidosYa: "https://pedidosya.com/...",

  // WiFi
  wifi: { network: "StroopwafelHuis", password: "lekker2024" },
} as const;

export const BRAND = {
  colors: {
    primary: "#C4704B",     // Warm Terracotta
    secondary: "#8FA88A",   // Sage Green
    accent: "#D4A843",      // Golden Honey
    dark: "#5C3D2E",        // Stroopwafel Brown
    cream: "#FFF8F0",       // Butter Cream
    rose: "#D4A0A0",        // Dusty Rose
    delft: "#3B5998",       // Heritage Blue
    text: "#3A3530",        // Espresso
    textLight: "#9B928A",   // Stone
    linen: "#F5F0EB",       // Linen
  },
  fonts: {
    heading: "Cormorant Garamond",
    body: "Lora",
    accent: "Dancing Script",
    sans: "DM Sans",
  },
} as const;

export const SEO = {
  defaultTitle: "Stroopwafel Huis | Café Holandés en Asunción, Paraguay",
  titleTemplate: "%s | Stroopwafel Huis",
  description: "La primera cafetería holandesa del Paraguay. Stroopwafels frescos hechos frente a vos, café de especialidad y ambiente cottage core. Villa Morra, Asunción.",
  url: "https://stroopwafelhuis.com.py",
  locale: "es_PY",
  keywords: [
    "stroopwafel Paraguay",
    "café Asunción",
    "cafetería holandesa",
    "wafles holandeses",
    "café de especialidad Asunción",
    "stroopwafel huis",
    "cafetería Villa Morra",
    "café cozy Asunción",
  ],
} as const;
```

---

## SUPABASE SCHEMA

```sql
-- Products (menu items + shop items)
CREATE TABLE products (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  name TEXT NOT NULL,
  slug TEXT UNIQUE NOT NULL,
  description TEXT,
  category TEXT NOT NULL, -- stroopwafel, coffee, dutch-treat, savory, combo, retail, merch
  price INTEGER NOT NULL, -- in Guaranies
  compare_at_price INTEGER, -- for discounts
  image_url TEXT,
  is_available BOOLEAN DEFAULT true,
  is_featured BOOLEAN DEFAULT false,
  is_shop_item BOOLEAN DEFAULT false, -- true for e-commerce items
  sort_order INTEGER DEFAULT 0,
  allergens TEXT[], -- gluten, dairy, nuts, etc.
  created_at TIMESTAMPTZ DEFAULT now()
);

-- Weekly Flavor (Crumbl model)
CREATE TABLE weekly_flavors (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  name TEXT NOT NULL,
  description TEXT,
  image_url TEXT,
  price INTEGER NOT NULL,
  week_start DATE NOT NULL,
  week_end DATE NOT NULL,
  is_active BOOLEAN DEFAULT false,
  created_at TIMESTAMPTZ DEFAULT now()
);

-- Orders (e-commerce)
CREATE TABLE orders (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  customer_name TEXT NOT NULL,
  customer_phone TEXT NOT NULL,
  customer_email TEXT,
  items JSONB NOT NULL,
  total INTEGER NOT NULL,
  status TEXT DEFAULT 'pending', -- pending, confirmed, preparing, ready, delivered, cancelled
  delivery_method TEXT, -- pickup, delivery, pedidosya
  notes TEXT,
  created_at TIMESTAMPTZ DEFAULT now()
);

-- Newsletter subscribers
CREATE TABLE subscribers (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  email TEXT UNIQUE NOT NULL,
  name TEXT,
  subscribed_at TIMESTAMPTZ DEFAULT now(),
  is_active BOOLEAN DEFAULT true
);

-- Blog posts
CREATE TABLE blog_posts (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  title TEXT NOT NULL,
  slug TEXT UNIQUE NOT NULL,
  content TEXT NOT NULL,
  excerpt TEXT,
  image_url TEXT,
  category TEXT, -- recetas, cultura, noticias
  is_published BOOLEAN DEFAULT false,
  published_at TIMESTAMPTZ,
  created_at TIMESTAMPTZ DEFAULT now()
);

-- Reviews / Testimonials
CREATE TABLE reviews (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  author_name TEXT NOT NULL,
  rating INTEGER CHECK (rating >= 1 AND rating <= 5),
  content TEXT NOT NULL,
  source TEXT, -- google, instagram, manual
  is_featured BOOLEAN DEFAULT false,
  created_at TIMESTAMPTZ DEFAULT now()
);

-- Loyalty points (Club Lekker)
CREATE TABLE loyalty_members (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  phone TEXT UNIQUE NOT NULL,
  name TEXT,
  points INTEGER DEFAULT 0,
  total_spent INTEGER DEFAULT 0,
  tier TEXT DEFAULT 'lekker', -- lekker, super-lekker, vip
  birthday DATE,
  joined_at TIMESTAMPTZ DEFAULT now()
);
```

---

## TAILWIND CONFIG (Cottage Core Theme)

```typescript
// tailwind.config.ts
import type { Config } from "tailwindcss";

const config: Config = {
  content: ["./app/**/*.{ts,tsx}", "./components/**/*.{ts,tsx}"],
  theme: {
    extend: {
      colors: {
        brand: {
          primary: "#C4704B",
          secondary: "#8FA88A",
          accent: "#D4A843",
          dark: "#5C3D2E",
          cream: "#FFF8F0",
          rose: "#D4A0A0",
          delft: "#3B5998",
          text: "#3A3530",
          "text-light": "#9B928A",
          linen: "#F5F0EB",
        },
      },
      fontFamily: {
        heading: ["var(--font-cormorant)", "serif"],
        body: ["var(--font-lora)", "serif"],
        accent: ["var(--font-dancing)", "cursive"],
        sans: ["var(--font-dm-sans)", "sans-serif"],
      },
      borderRadius: {
        cottage: "0.75rem",
      },
    },
  },
  plugins: [require("tailwindcss-animate")],
};
export default config;
```

---

## COMPONENTS REUSED FROM OTHER REPOS

| Component | Source | Adaptations |
|-----------|--------|-------------|
| Hero | clinica-duerksen | Change to video hero, cottage colors, different CTAs |
| Footer | clinica-duerksen | Same 4-column pattern, different links/brand |
| Header | clinica-duerksen | Simplified nav, add cart icon from fun4me |
| WhatsApp button | both repos | Same floating button, different message |
| Cookie consent | clinica-duerksen | Same component, translate |
| JSON-LD | clinica-duerksen | Add Restaurant + CafeOrCoffeeShop schema |
| Google Analytics | clinica-duerksen | Same component, different ID |
| Product Card | fun4me-store | Adapt for stroopwafels (photo, name, price, add to cart) |
| Cart Drawer | fun4me-store | Same slide-out cart pattern |
| Announcement Bar | fun4me-store | Weekly flavor announcement |
| Newsletter | fun4me-store | Same email capture |
| Breadcrumb | clinica-duerksen | Same pattern |

---

## NEW COMPONENTS (Unique to Stroopwafel Huis)

| Component | Purpose |
|-----------|---------|
| `marquee.tsx` | Scrolling text band (Levain pattern) |
| `weekly-flavor.tsx` | Crumbl-style weekly rotating flavor showcase |
| `explainer.tsx` | "What is a Stroopwafel?" with cross-section illustration |
| `founder-story.tsx` | Bram & Rach story with photo |
| `instagram-feed.tsx` | IG embed (6 latest posts) |
| `menu-tabs.tsx` | Tabbed menu navigation |
| `combo-card.tsx` | Highlighted combo deal card |
| `loyalty-card.tsx` | Club Lekker points display |
| `flavor-calendar.tsx` | Calendar showing past/upcoming weekly flavors |

---

## DEPLOYMENT

```yaml
# docker-compose.yml
services:
  stroopwafel-huis:
    build: .
    restart: unless-stopped
    environment:
      - NEXT_PUBLIC_SUPABASE_URL=${SUPABASE_URL}
      - NEXT_PUBLIC_SUPABASE_ANON_KEY=${SUPABASE_ANON_KEY}
      - SUPABASE_SERVICE_ROLE_KEY=${SUPABASE_SERVICE_ROLE_KEY}
      - RESEND_API_KEY=${RESEND_API_KEY}
      - NEXT_PUBLIC_GA_ID=${GA_ID}
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.stroopwafel.rule=Host(`stroopwafelhuis.com.py`)"
      - "traefik.http.routers.stroopwafel.tls.certresolver=letsencrypt"
      - "traefik.http.services.stroopwafel.loadbalancer.server.port=3000"
    networks:
      - stroopwafel-net

networks:
  stroopwafel-net:
    external: true
```

Same pattern as clinica-duerksen deployment on Hostinger VPS.

---

## DEVELOPMENT TIMELINE

| Week | Task |
|------|------|
| 1 | Init Next.js 15 project, install deps, configure Tailwind cottage-core theme |
| 2 | Build layout (header, footer, WhatsApp), constants, Supabase schema |
| 3 | Home page (hero, marquee, weekly flavor, explainer, favorites) |
| 4 | Home page cont. (founder story, reviews, IG feed, location, newsletter) |
| 5 | Menu page (visual, tabbed), individual product pages |
| 6 | Shop/e-commerce (product cards, cart, checkout via WhatsApp) |
| 7 | Weekly Flavor page, Blog, Nuestra Historia, Que es un Stroopwafel |
| 8 | Admin CMS (products, weekly flavor, orders, blog) |
| 9 | SEO (JSON-LD, sitemap, robots, meta), Analytics |
| 10 | Catering, Contact form, Rewards, legal pages |
| 11 | Photography integration, mobile testing, performance optimization |
| 12 | Deploy to Hostinger VPS, DNS, SSL, go live |

---

## COST: $0/month

- Hosting: Already on Hostinger VPS (shared with other projects)
- Supabase: Free tier (sufficient for launch)
- Domain: ~$15/year
- Resend: Free tier (100 emails/day)
- NO Shopify fees, NO platform fees
