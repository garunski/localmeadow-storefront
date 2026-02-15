# Local Meadow Storefront

Next.js-based B2C marketplace storefront for Local Meadow.

**Documentation**: In the **localmeadow-docs** repo under `backlog/docs/` and `backlog/decisions/`. From that repo: `mise exec -- backlog doc list` then `mise exec -- backlog doc view <id>`.

## Features

- Product browsing and search (Algolia)
- Shopping cart and checkout
- Stripe payment integration
- Customer accounts
- Order history
- Multi-vendor product display
- Responsive design
- Internationalization (i18n)

## Prerequisites

- Node.js 20+
- Local Meadow API running at `http://localhost:9000`

## Quick Start

```bash
# Install dependencies
npm install
# or
yarn install

# Set up environment variables
cp .env.example .env.local
# Edit .env.local with your API and service credentials

# Start development server
npm run dev
# or
yarn dev
```

The storefront will be available at `http://localhost:3000`

## Environment Variables

```env
MEDUSA_BACKEND_URL=http://localhost:9000
NEXT_PUBLIC_MEDUSA_PUBLISHABLE_KEY=pk_...
NEXT_PUBLIC_BASE_URL=http://localhost:3000
NEXT_PUBLIC_DEFAULT_REGION=us

NEXT_PUBLIC_STRIPE_KEY=pk_test_...

NEXT_PUBLIC_SITE_NAME="Local Meadow"
NEXT_PUBLIC_SITE_DESCRIPTION="Local Meadow Marketplace"

NEXT_PUBLIC_ALGOLIA_ID=...
NEXT_PUBLIC_ALGOLIA_SEARCH_KEY=...

NEXT_PUBLIC_VENDOR_URL=http://localhost:3001
```

## Scripts

- `npm run dev` - Start development server with Turbopack
- `npm run build` - Build for production
- `npm run start` - Start production server
- `npm run lint` - Lint code

## Project Structure

```
src/
├── app/            # Next.js app directory
├── components/     # React components
├── lib/            # Utilities and SDK setup
├── modules/        # Feature modules
└── styles/         # Global styles
```

## Deployment

This is a Next.js application and can be deployed to:
- Vercel (recommended)
- DigitalOcean App Platform
- Any Node.js hosting platform

```bash
npm run build
npm run start
```

## Documentation

- [Next.js Documentation](https://nextjs.org/docs)
- [MercurJS Storefront](https://www.mercurjs.com/docs/storefront)
- [Medusa JS SDK](https://docs.medusajs.com/resources/js-sdk)

## License

MIT
