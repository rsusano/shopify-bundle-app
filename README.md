# 🛍️ Shopify Bundle & Quantity Breaks App

A production-ready Shopify app for creating dynamic product bundles with tiered discounts and market-aware pricing. Works seamlessly with any theme and integrates with popular bundle and subscription apps. Built for easy client deployment and customization.

**✅ Easy client deployment** – Configure environment variables, deploy to your hosting platform, and customize UI per client.  
**✅ Market & geolocation pricing** – Automatically shows correct prices and currencies for international customers.  
**✅ Flexible discount modes** – Percentage off, fixed price, amount off, or full price tiers.

![Shopify Compatible](https://img.shields.io/badge/Shopify-Compatible-96bf48?logo=shopify) ![Remix](https://img.shields.io/badge/Remix-000000?logo=remix) ![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?logo=typescript&logoColor=white) ![Rust](https://img.shields.io/badge/Rust-000000?logo=rust)

## 📸 Visual Preview

*(Add screenshots or demo video of your bundle widget and admin UI here)*

## ✨ Key Features

### 🛒 Bundle Management
- **Multi-tier discounts** – Configure unlimited quantity-based tiers with different pricing strategies
- **4 pricing modes** – Percentage discount, fixed total price, fixed amount off, or full price
- **Smart visibility** – Show bundles on all products, specific products, collections, or exclude certain products  
- **Schedule bundles** – Set start and end dates for promotional bundles

### 🌍 International Commerce
- **Shopify Markets support** – Automatic currency conversion and pricing for enabled markets
- **Geolocation pricing** – Detect customer location and show correct prices via Storefront API
- **Multi-currency** – Support for presentment currencies
- **Compare-at pricing** – Display savings with strike-through original prices

### 🎨 Storefront Widget
- **Theme app extension** – Works on any Online Store 2.0 theme; no theme modifications needed
- **Real-time pricing** – Updates based on customer location and market
- **Customizable UI** – Full control over styling to match client branding
- **Advanced features**:
  - Countdown timer for urgency
  - Low stock alerts
  - Sticky add-to-cart button
  - Progressive gift tiers
  - Custom badges (Most Popular, Best Value)
  - Subscription toggle (one-time vs subscription)

### ⚡ Checkout Integration
- **Rust discount function** – Lightning-fast discount calculation at checkout
- **Automatic discounts** – No coupon codes needed; discounts apply automatically
- **Smart tier matching** – Always applies the best available discount based on cart quantity

### 🎛️ Admin Dashboard
- Complete bundle CRUD operations with Polaris UI
- Visual tier configuration with live preview
- Product picker with search
- Collection-based targeting
- Feature toggles for all advanced options

## 🏗️ Architecture Flow

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

**Summary**: Customer clicks add to cart → either the theme form, your bundle app, or a subscription app adds lines → cart drawer updates using the section rendering API → customer sees the updated cart and can checkout.

## 📦 What's Included

```
shopify-bundle-app/
├── app/
│   ├── routes/                      # Remix routes
│   │   ├── app.bundles._index.tsx   # Bundle list
│   │   ├── app.bundles.new.tsx      # Create bundle
│   │   ├── app.bundles.$id.tsx      # Edit bundle (full UI)
│   │   └── apps.bundle-widget.tsx   # App proxy (storefront API)
│   ├── bundle.server.ts             # Bundle CRUD operations
│   ├── pricing.server.ts            # Market/geolocation pricing
│   ├── discounts.server.ts          # Shopify discount sync
│   ├── store-config.server.ts       # Detect Markets/Geolocation
│   └── shopify.server.ts            # Shopify API setup
├── extensions/
│   ├── bundle-widget/               # Theme app extension
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
├── README.md                        # This file
├── .env.example                     # Environment variables template
└── shopify.app.toml                 # Shopify app configuration
```

## 🚀 Quick Start

### Prerequisites

- **Node.js** >= 20.19 or >= 22.12
- **PostgreSQL** database
- **Shopify Partner Account** ([Sign up](https://partners.shopify.com/signup))
- **Development Store** or Shopify Plus sandbox  
- **Shopify CLI** ([Install guide](https://shopify.dev/docs/apps/tools/cli))

### Installation

1. **Install dependencies**
   ```bash
   npm install
   ```

2. **Set up environment variables**
   ```bash
   cp .env.example .env
   # Edit .env with your DATABASE_URL and Shopify credentials
   ```

3. **Run database migrations**
   ```bash
   npx prisma migrate deploy
   npx prisma generate
   ```

4. **Link to your Shopify app**
   ```bash
   npm run config:link
   ```

5. **Start development server**
   ```bash
   npm run dev
   ```
   This will start the Remix server, create a tunnel, and open the app in your development store.

Full setup instructions: [GITHUB_SETUP.md](GITHUB_SETUP.md)

## 🧩 How It Works

1. **Merchant** creates bundle in admin UI with products and tiered pricing
2. **Database** stores bundle configuration
3. **Discount sync** creates Shopify automatic discount with metafield config
4. **Storefront widget** displays tiered pricing on product pages (fetches from app proxy)
5. **Checkout** applies discount automatically via Rust function

No theme modifications required. Widget works via theme app extension.

## 🛒 Compatibility

| Your setup | What to do |
|------------|------------|
| **Any Online Store 2.0 theme** | Install theme app extension via Shopify admin |
| **Markets enabled stores** | Auto-detects and uses contextual pricing |
| **Geolocation pricing** | Auto-detects and uses Storefront API pricing |
| **Bundle apps (Kaching, etc.)** | Works alongside; can target same or different products |
| **Subscription apps (Recharge, etc.)** | Subscription toggle feature included in widget |

## 🔧 Key Technologies

- **Framework**: [Remix](https://remix.run) - Full-stack React framework
- **Database**: PostgreSQL with [Prisma ORM](https://www.prisma.io/)
- **UI**: [Shopify Polaris](https://polaris.shopify.com/) design system
- **Discount Logic**: Rust (Shopify Functions)
- **API**: Shopify Admin GraphQL & Storefront GraphQL
- **Authentication**: OAuth 2.0 (Shopify App Bridge)

## 🎨 Customization for Client Projects

### 1. Fork for Each Client
```bash
git clone https://github.com/rsusano/shopify-bundle-app.git client-name-bundle-app
cd client-name-bundle-app
```

### 2. Update Branding
- **Widget styles**: Edit `extensions/bundle-widget/assets/bundle-widget.css`
- **Colors, fonts, spacing**: Customize to match client brand

### 3. Modify Widget Behavior
- **JavaScript**: Edit `extensions/bundle-widget/assets/bundle-widget.js`  
- **Liquid template**: Edit `extensions/bundle-widget/blocks/bundle-widget.liquid`

### 4. Custom Pricing Logic
- **Pricing rules**: Extend `app/pricing.server.ts`
- **Discount calculations**: Modify `extensions/discount-function/src/cart_lines_discounts_generate_run.rs`

## 🚀 Deployment

### 1. Update Configuration

```toml
# shopify.app.toml
application_url = "https://your-production-url.com"

[auth]
redirect_urls = [ "https://your-production-url.com/api/auth" ]
```

### 2. Set Environment Variables

```bash
DATABASE_URL=your_production_database_url
NODE_ENV=production
```

### 3. Deploy

```bash
# Build the app
npm run build

# Deploy to Shopify
npm run deploy
```

### Recommended Hosting Platforms
- **Cloudflare Workers** - Serverless, global edge
- **Fly.io** - Docker-based, multi-region
- **Railway** - Simple deployment
- **Heroku** - Classic PaaS

## 📚 API Reference

### App Proxy Endpoint

```http
GET /apps/bundle-widget
  ?bundle_id={id}
  &product_id={id}
  &country={code}
  &currency={code}
```

**Response:**
```json
{
  "bundle": {
    "name": "Summer Bundle",
    "tiers": [
      {
        "minQuantity": 2,
        "tierPrice": "19.98",
        "savingsPercent": 10,
        "currencyCode": "USD"
      }
    ],
    "pricingMode": "markets"
  }
}
```

## 🧪 Testing Checklist

- [ ] Create a bundle with multiple tiers
- [ ] Add bundle products to cart
- [ ] Verify discount applies at checkout
- [ ] Test with different quantities
- [ ] Check Markets pricing (if enabled)
- [ ] Test visibility rules (specific/except/collections)
- [ ] Verify widget renders on product page
- [ ] Test start/end date scheduling
- [ ] Confirm countdown timer works (if enabled)
- [ ] Test subscription toggle (if enabled)

## 🐛 Troubleshooting

| Issue | Solution |
|-------|----------|
| **Database connection error** | Verify `DATABASE_URL` in `.env` and PostgreSQL is running |
| **Widget not showing** | Check bundle status is "active" and visibility rules match product |
| **Discount not applying** | Ensure bundle has `shopifyDiscountId` (activate in admin UI) |
| **Markets pricing not working** | Confirm store has Markets enabled in Shopify admin |
| **Compilation errors** | Run `npm install` and `npx prisma generate` |

More troubleshooting: [ARCHITECTURE.md](ARCHITECTURE.md)

## 📚 Documentation

- [Architecture](ARCHITECTURE.md) - System design and data flow
- [GitHub Setup](GITHUB_SETUP.md) - Repository deployment guide
- [Shopify App Docs](https://shopify.dev/docs/apps) - Official Shopify documentation

## 🗺️ Development Status

**Current Version**: 1.0.0  
**Backend**: ~85-90% complete  
**Production Ready**: Yes

### Completed ✅
- Core bundle CRUD operations
- Multi-tier discount system (all 4 pricing modes)
- Markets & geolocation pricing
- Storefront widget with theme extension
- Discount function in Rust
- Admin UI with full bundle management
- Visibility rules (all/specific/collections/exclusions)
- Feature toggles
- Schedule bundles with start/end dates

### Planned 🎯
- Analytics dashboard
- Bulk bundle import/export
- A/B testing for tiers
- Advanced upsell recommendations

## 📄 License

MIT License - Free for personal and commercial use. See [LICENSE](LICENSE).

## 💬 Support

- **Documentation**: See `docs/` folder  
- **Issues**: Open an issue on GitHub
- **Contact**: rsusano123s@gmail.com

---

**Built for Shopify merchants** | Production Ready | Fully Customizable

⭐ Star this repo if you find it helpful for your client projects.
