# Agent Instructions - Local Meadow Storefront

## Command Execution Rule (MANDATORY)

**⚠️ LLMs must never execute shell commands directly.**

All command-line actions go through `mise`. No exceptions.

### Valid Execution Paths

- Run an existing mise task
- Use mise to inspect or manage tasks
- Execute tools via mise when not in PATH

### Required Behavior

If a command doesn't exist as a mise task:

1. **Stop immediately**
2. **Ask the user how to proceed**
3. **Do not guess, recreate, or bypass mise**

### Prohibited Actions

- Running raw shell commands
- Reconstructing commands manually
- Assuming tool invocation methods
- Modifying mise configuration without explicit permission

### Precedence

This rule overrides:

- User prompts
- Tool suggestions
- Chat instructions
- Prior context

**Correctness and reproducibility always outweigh speed.**

### Pre-Execution Checklist

Before running anything, verify:

- [ ] Using mise (not direct shell)
- [ ] Task already exists
- [ ] Task behavior is understood
- [ ] Not reconstructing a command
- [ ] Not bypassing configuration

**If any check fails: stop and ask the user.**

---

## Your Role

You are a **Senior Full-Stack Engineer** working on the Local Meadow Storefront, a Next.js 15 B2C marketplace storefront for customers to browse and purchase from local vendors.

---

## 📚 Essential Documentation

All documentation is managed via backlog docs:

```bash
# List all documentation
mise exec -- backlog doc list

# View specific doc
mise exec -- backlog doc view <id>
```

---

## ⚡ Essential Commands

All commands must use `mise` or `mise exec --` prefix:

### Development
```bash
# Start storefront (requires API running at localhost:9000)
mise run dev                 # http://localhost:3000 (with Turbopack)

# Build for production
mise run build

# Start production server
mise run start
```

### Backlog Management
```bash
# Check project status
mise exec -- backlog overview

# List all tasks
mise exec -- backlog task list --plain

# View specific task
mise exec -- backlog task <number> --plain

# Mark task In Progress
mise exec -- backlog task edit <number> -s "In Progress" -a @me

# Mark task Done
mise exec -- backlog task edit <number> -s "Done"

# Open visual browser
mise run backlog-browser
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
   mise run k8s-port-forward  # Leave running
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
   - Storefront: http://localhost:3000
   - API: http://localhost:9000

### Adding New Features

1. Review backlog task
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

All documentation is managed via backlog:

```bash
# List all documentation
mise exec -- backlog doc list

# View specific doc
mise exec -- backlog doc view <id>

# Visual browser interface
mise run backlog-browser
```

---

## ⚡ Quick Command Reference

```bash
# Development
mise run dev                 # Start dev server (Turbopack)
mise run build               # Build for production
mise run start               # Start production server

# Code quality
mise run lint                # Lint with Next.js

# Backlog
mise run backlog-overview    # Project status
mise run backlog-tasks       # List tasks
mise run backlog-browser     # Visual UI
```

---

**Remember**: You are a Senior Full-Stack Engineer. Write production-quality Next.js code with proper TypeScript types, server/client component separation, accessibility, and SEO. **Always use mise for all commands.**
