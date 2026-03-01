# API Health "Fix Overdue" – Why It Persists & How to Fix

## ✅ Latest: Deployed bundle-widget-app-3 (API fixes included)

The app has been deployed with:
- API version 2025-04
- `markets(type: REGION)` for New Catalog APIs compliance
- Webhooks api_version 2025-04

**Next:** Wait up to 14 days for API health status to clear. Use **Monitoring → API health → Last day** to confirm no new deprecated calls.

---

## Why the Error Can Persist

### 1. **14-Day Rolling Window**
Shopify's API health report shows calls from the **last 14 days**. Even after you deploy fixes:
- Any deprecated calls made in the past 14 days still count
- Status can take **up to 14 days** to change to OK after you stop making deprecated calls
- Source: [API health report](https://shopify.dev/docs/api/usage/versioning/api-health)

### 2. **Deployment Required**
The API version changes (2025-04, `type: REGION`) are in your code but must be **deployed** to take effect:
```bash
shopify app deploy
```
Until you deploy, the live app is still using the old code and making deprecated calls.

### 3. **8 Webhook Errors**
Webhook failures can affect API health. Common causes:
- **App URL**: Your `shopify.app.toml` has `application_url = "https://example.com"` – webhooks deliver to this URL. If the app isn't running at that URL, webhooks fail.
- **Webhook API version**: You may need to [manually upgrade webhook API version](https://shopify.dev/docs/apps/build/webhooks/subscribe/use-newer-api-version) in the Partner Dashboard.

### 4. **Verify No Deprecated Calls**
After deploying, use the API health report:
- Go to **Monitoring → API health**
- Filter by **Last day** to see if new deprecated calls are still happening
- If empty after 24 hours, you've stopped making deprecated calls (status will clear within 14 days)

---

## Action Checklist

- [ ] Run `shopify app deploy` to push the API fixes
- [ ] Set correct `application_url` in Partner Dashboard (or `shopify.app.toml`) – your real app URL, not example.com
- [ ] In Partner Dashboard: **Settings → Webhooks** – confirm API version is 2025-04 or later
- [ ] Wait up to 14 days after last deprecated call for status to change to OK
- [ ] Use **Last day** filter in API health to confirm no new deprecated calls
