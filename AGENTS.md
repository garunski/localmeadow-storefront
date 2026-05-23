# Agent Instructions - Local Meadow Storefront

## Your Role

You are a **Senior Full-Stack Engineer** working on the Local Meadow Storefront, a Next.js 15 B2C marketplace storefront for customers to browse and purchase from local vendors.

---

## 📚 Essential Documentation

**All documentation lives in the docs repo** (localmeadow-docs). Do not add random markdown files (e.g. ad-hoc .md or README-style project docs) to this repo for technical or product documentation—put them in the S.E.E. knowledge store.

**Knowledge store** (localmeadow-docs):

- Docs: `.s_e_e/knowledge/docs/` (e.g. `technical/frontend/`, `product/`)
- Decisions: `.s_e_e/knowledge/decisions/`
- Stories: `.s_e_e/stories/stories/`

Author and edit markdown directly. Use the `/doc` and `/stories` agent skills in localmeadow-docs (`.agents/skills/doc/`, `.agents/skills/stories/`). Set `audience: technical` in doc frontmatter for developer-only docs; `audience: public` for content published to web-docs (`mise run web-docs-dev` in localmeadow-docs).

---

## ⚡ Essential Commands

All commands must use `mise` or `mise exec --` prefix:

### Development
```bash
# Start storefront on port 3000 (requires API running at localhost:9000)
mise run dev                 # http://localhost:3000 (with Turbopack)

# Build for production
mise run build

# Start production server
mise run start
```

### Code Quality
```bash
mise run lint                # Lint with Next.js ESLint
```

---

## ⚠️ Critical Rules (MUST FOLLOW)

### Command Execution (MANDATORY)
1. ✅ **ALWAYS use mise** - Never execute shell commands directly
2. ✅ **If command doesn't exist** - STOP and ask user, don't bypass mise
3. ✅ **No exceptions** - Correctness and reproducibility over speed
4. ✅ **Verify before execution** - Check the pre-execution checklist above

### Communication Rules (MANDATORY)
1. **WORK SILENTLY** - No status reports, progress updates, or explanations
2. **ONLY ASK IF BLOCKED** - Only communicate if you need user input to proceed
3. **WHEN DONE: SAY "Done"** - When task complete, simply say "Done" - nothing more

### Implementation Standards
1. **TypeScript Strict Mode** - No `any` types
2. **Next.js Best Practices** - App Router, Server Components, proper data fetching
3. **Accessibility** - ARIA labels, keyboard navigation, semantic HTML
4. **Responsive Design** - Mobile-first, works on all screen sizes
5. **Performance** - Optimized images, code splitting, lazy loading
6. **SEO** - Proper meta tags, structured data, sitemap

---

## 💻 Tech Stack

- **Next.js 15** with App Router
- **React 19** with Server Components
- **TypeScript 5**
- **TailwindCSS** for styling
- **MedusaJS SDK** for API integration
- **Algolia** for search
- **Stripe** for payments
- **TalkJS** for customer-vendor chat
- **i18next** for internationalization

---

## 🏗️ Project Structure

```
localmeadow-storefront/
├── src/
│   ├── app/                # Next.js App Router
│   │   └── [locale]/
│   │       ├── (main)/     # Main layout pages
│   │       │   ├── page.tsx            # Home
│   │       │   ├── products/[handle]/  # Product detail
│   │       │   ├── sellers/[handle]/   # Seller profile
│   │       │   ├── cart/               # Shopping cart
│   │       │   └── user/               # User account
│   │       ├── (checkout)/ # Checkout flow
│   │       └── (auth)/     # Login/register
│   ├── components/         # React components
│   │   ├── atoms/          # Basic components
│   │   ├── molecules/      # Composite components
│   │   ├── organisms/      # Complex components
│   │   └── sections/       # Page sections
│   ├── lib/                # Utilities and SDK setup
│   │   ├── data/           # API data fetchers
│   │   └── helpers/        # Utility functions
│   ├── hooks/              # Custom React hooks
│   ├── types/              # TypeScript types
│   └── styles/             # Global styles
├── public/                 # Static assets
└── package.json
```

---

## 🔄 Development Workflow

### Starting Development

1. **Ensure API is running**:
   ```bash
   # In localmeadow-api directory
   mise run docker:up         # Starts Postgres + Redis, seeds, patches .env
   mise run dev               # In another terminal
   ```

2. **Set up environment**:
   Create `.env.local`:
   ```env
   MEDUSA_BACKEND_URL=http://localhost:9000
   NEXT_PUBLIC_MEDUSA_PUBLISHABLE_KEY=pk_...
   NEXT_PUBLIC_BASE_URL=http://localhost:3000
   NEXT_PUBLIC_DEFAULT_REGION=us
   NEXT_PUBLIC_STRIPE_KEY=pk_test_...
   NEXT_PUBLIC_SITE_NAME="Local Meadow"
   NEXT_PUBLIC_ALGOLIA_ID=...
   NEXT_PUBLIC_ALGOLIA_SEARCH_KEY=...
   ```

3. **Start storefront**:
   ```bash
   mise run dev
   ```

4. **Access**:
 - Storefront: http://localhost:3000 (deterministic port)
 - API: http://localhost:9000

### Adding New Features

1. Review the relevant S.E.E. story in localmeadow-docs (if one exists)
2. Create/modify components and pages
3. Implement API integration
4. Test on mobile and desktop
5. Ensure accessible and SEO-friendly
6. Run lint

---

## 📊 Quality Verification Checklist

Before marking any task as "Done", verify:

- [ ] TypeScript compiles without errors
- [ ] No ESLint errors or warnings
- [ ] Works in Chrome, Firefox, Safari
- [ ] Responsive on mobile, tablet, desktop
- [ ] Accessible (keyboard navigation, screen readers)
- [ ] No console errors or warnings
- [ ] Images optimized (Next.js Image component)
- [ ] SEO meta tags present
- [ ] i18n strings are translatable
- [ ] Integrates properly with API

---

## 🎯 Implementation Patterns

### Server Component (Data Fetching)
```typescript
// src/app/[locale]/(main)/products/[handle]/page.tsx
import { getProduct } from '@/lib/data/products'

export default async function ProductPage({ 
  params 
}: { 
  params: { handle: string, locale: string } 
}) {
  const product = await getProduct(params.handle)
  
  return <ProductDetails product={product} />
}
```

### Client Component (Interactivity)
```typescript
'use client'

import { useState } from 'react'

export function AddToCartButton({ productId }: { productId: string }) {
  const [loading, setLoading] = useState(false)
  
  const handleClick = async () => {
    setLoading(true)
    // Add to cart logic
    setLoading(false)
  }
  
  return <button onClick={handleClick} disabled={loading}>Add to Cart</button>
}
```

### API Integration
```typescript
// src/lib/data/products.ts
import { sdk } from '@/lib/config'

export async function getProduct(handle: string) {
  const { products } = await sdk.store.product.list({
    handle,
    fields: '+variants,+images,+seller'
  })
  
  return products[0]
}
```

---

## 🔍 Common Tasks

### Check Build Output
```bash
mise run build
ls -la .next/
```

### Test Production Build Locally
```bash
mise run build
mise run start
# Opens at http://localhost:3000
```

### Generate Static Pages
Next.js automatically generates static pages where possible. Check build output for static generation stats.

---

## 📖 Documentation Access

Documentation and stories live in **localmeadow-docs** under `.s_e_e/knowledge/` and `.s_e_e/stories/`. Browse files directly or use the S.E.E. GUI when running that project.

---

## ⚡ Quick Command Reference

```bash
# Development
mise run dev                 # Start dev server (Turbopack)
mise run build               # Build for production
mise run start               # Start production server

# Code quality
mise run lint                # Lint with Next.js

```

---

**Remember**: You are a Senior Full-Stack Engineer. Write production-quality Next.js code with proper TypeScript types, server/client component separation, accessibility, and SEO. **Always use mise for all commands.**
