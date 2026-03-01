## Project Structure

This repo is organized to keep the Shopify app, theme extension, discount function, and infrastructure clearly separated.

### High‑level tree

```text
shopify-bundle-app/
├── app/
│   ├── routes/                      # Remix routes (admin + app proxy)
│   ├── bundle.server.ts             # Bundle CRUD operations
│   ├── pricing.server.ts            # Market/geolocation pricing
│   ├── discounts.server.ts          # Shopify discount sync
│   ├── store-config.server.ts       # Detect Markets/Geolocation
│   └── shopify.server.ts            # Shopify API setup
├── extensions/
│   ├── bundle-widget/               # Theme app extension (storefront widget)
│   │   ├── blocks/bundle-widget.liquid
│   │   ├── assets/bundle-widget.js
│   │   └── assets/bundle-widget.css
│   └── discount-function/           # Rust discount function
│       └── src/cart_lines_discounts_generate_run.rs
├── prisma/
│   ├── schema.prisma                # Database models
│   └── migrations/                  # PostgreSQL migrations
├── docs/
│   ├── ARCHITECTURE.md              # Detailed system design
│   └── GITHUB_SETUP.md              # Repository setup guide
├── README.md                        # Main documentation + quick start
├── .env.example                     # Environment variables template
└── shopify.app.toml                 # Shopify app configuration
```

Some of these files are stubs or planned; use this layout as the guide when adding new functionality so everything stays consistent.

