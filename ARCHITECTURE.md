## Shopify Bundle App – Architecture

This document gives a high–level view of how the bundle app works across the admin, database, discount function, and storefront.

### System Overview

The app has four main pieces:

1. **Merchant Admin (Remix + Polaris)** – where merchants create and manage bundles, products, and tiers.
2. **Database (PostgreSQL via Prisma)** – stores bundle configuration, shop settings, and session data.
3. **Discount Function (Rust)** – runs at checkout to calculate and apply the correct discount.
4. **Storefront Widget (Theme App Extension)** – shows bundle tiers and pricing on product pages.

### Architecture Flow (High Level)

```text
┌─────────────────────────────────────────────────────────────┐
│                     MERCHANT ADMIN                           │
│  ┌──────────────────────────────────────────────────────┐   │
│  │  Creates bundle with products + tiers                │   │
│  │  (Remix app with Polaris UI)                         │   │
│  └────────────────┬─────────────────────────────────────┘   │
│                   ▼                                          │
│  ┌──────────────────────────────────────────────────────┐   │
│  │  PostgreSQL Database (Prisma ORM)                    │   │
│  │  Syncs to Shopify Automatic Discount                 │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│              SHOPIFY DISCOUNT FUNCTION (Rust)                │
│  Reads cart → Matches bundle products → Applies discount    │
└─────────────────────────────────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│                  CUSTOMER STOREFRONT                         │
│  ┌──────────────────────────────────────────────────────┐   │
│  │  Theme App Extension (Liquid + JavaScript)           │   │
│  │  Fetches bundle via app proxy                        │   │
│  │  Shows tiered pricing with real-time calculations    │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

**Summary**: Merchant configures bundles in the admin → config is stored in PostgreSQL (via Prisma) and synced to Shopify discounts → the Rust discount function applies the correct tier at checkout → the storefront widget shows available tiers and savings to the customer.

For a quick version of this diagram and tree, see the `Architecture Flow` and `What's Included` sections in `README.md`.

### Customer Cart Flow (Storefront)

This flow mirrors the style of the diagram you shared and focuses on what happens when a customer adds to cart and how different apps (theme, bundle app, subscription app) participate.

```text
┌──────────────────────────────┐
│ Customer clicks Add to cart │
└───────────────┬─────────────┘
                ▼
        ┌──────────────────────┐
        │   Who adds to cart?  │
        └─────────┬────────────┘
          ┌───────┴───────────────┐
          ▼                       ▼
  ┌──────────────────┐    ┌───────────────────┐
  │ Theme form /     │    │ Bundle app (this  │
  │ Quick add        │    │ app, e.g. Kaching)│
  └────────┬─────────┘    └────────┬──────────┘
           │                       │
           ▼                       ▼
  ┌──────────────────┐    ┌───────────────────┐
  │ Theme adds via   │    │ App adds via API  │
  │ fetch (cart API) │    └───────────────────┘
  └────────┬─────────┘
           │
           ▼
  ┌────────────────────────────────────────────┐
  │ Dispatch `cart-drawer:open` or `cart:update` │
  └─────────────────────────┬──────────────────┘
                            ▼
                 ┌──────────────────────┐
                 │ Cart drawer script   │
                 │ receives event       │
                 └─────────┬────────────┘
                           ▼
                 ┌──────────────────────┐
                 │      Open dialog     │
                 └─────────┬────────────┘
                           ▼
                 ┌──────────────────────┐
                 │ Section HTML in      │
                 │ event? (Yes / No)    │
                 └─────────┬────────────┘
                     Yes   │    No
                           ▼
                 ┌──────────────────────┐
                 │ Fetch section via    │
                 │ Section Rendering API│
                 └─────────┬────────────┘
                           ▼
                 ┌──────────────────────┐
                 │ Replace drawer       │
                 │ content with HTML    │
                 └─────────┬────────────┘
                           ▼
                 ┌──────────────────────┐
                 │ Drawer shows updated │
                 │ cart; customer can   │
                 │ review and checkout  │
                 └──────────────────────┘
```

This is the storefront side of the architecture: your bundle app participates in the "who adds to cart?" decision (by adding lines via API and applying discounts), while the theme handles the cart drawer and rendering logic.

