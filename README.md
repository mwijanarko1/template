# Mikhail Builds Website Template for Beginners

A modern, beginner-friendly Next.js template for rapid web development.

## Tech Stack

- **Framework**: Next.js 16 with App Router
- **Language**: TypeScript for type safety
- **UI Library**: React 19
- **Styling**: Tailwind CSS v4 with custom animations
- **Development**: Turbopack for fast builds
- **Linting**: Oxlint (primary, with anti-slop rules) plus ESLint / Next.js config
- **Testing**: Vitest with Testing Library
- **Build Tool**: PostCSS with Autoprefixer

## Features

- **Modern UI Components**: Pre-built Hero component with animated gradient backgrounds
- **Responsive Design**: Mobile-first approach with Tailwind CSS
- **TypeScript Support**: Full type checking and IntelliSense
- **Custom Animations**: Blob animations and smooth transitions
- **Developer Experience**: Optimized with Turbopack for fast local development

## Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) (v18 or higher)
- [Bun](https://bun.sh/) (recommended package manager for this template)

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/mwijanarko1/template.git
   cd template
   ```

2. **Install dependencies**
   ```bash
   bun install
   ```

3. **Start the development server**
   ```bash
   bun run dev
   ```

## Project Structure

```
template/
├── AGENTS.md                  # Agent rules + CLI profile placeholders
├── docs/
│   ├── CODEBASE_MAP.md        # Architecture / navigation map
│   ├── security-checklist.md
│   ├── web-compliance-checklist.md
│   └── technical-seo-checklist.md
├── src/
│   ├── app/                   # Next.js App Router entrypoints
│   ├── components/            # UI components (Hero + tests)
│   ├── lib/                   # Shared helpers (env validation)
│   └── test/                  # Vitest setup
├── tools/oxlint/anti-slop/    # Custom oxlint rules used across Mikhail Builds repos
├── oxlint.config.mjs          # Primary linter config
├── eslint.config.mjs          # ESLint / Next.js config
├── vitest.config.mts          # Unit test config
├── next.config.mjs
├── postcss.config.mjs
├── tailwind.config.js
└── package.json
```

## Documentation

- [`AGENTS.md`](AGENTS.md): working rules for coding agents, including GitHub / Cloudflare / Convex / Clerk / Resend profile placeholders (`YOUR_*`)
- [`docs/CODEBASE_MAP.md`](docs/CODEBASE_MAP.md): modules, routes, workflows, and risks
- [`docs/security-checklist.md`](docs/security-checklist.md): security gate (`security-audit` / `security-vulnerability-mitigation`)
- [`docs/web-compliance-checklist.md`](docs/web-compliance-checklist.md): privacy, a11y, consumer (`website-compliance`)
- [`docs/technical-seo-checklist.md`](docs/technical-seo-checklist.md): crawl/index/metadata (`technical-seo`)

## Scripts

```bash
bun run dev         # Next.js + Turbopack
bun run lint        # Oxlint (primary)
bun run lint:eslint # ESLint / next core-web-vitals
bun run typecheck   # TypeScript
bun run test        # Vitest watch mode
bun run test:run    # Vitest single run
bun run build       # Production build
```

## Testing

Tests use Vitest and React Testing Library. Add `*.test.tsx` files next to your components.

## Environment

Copy `.env.example` to `.env.local` and fill in values. Use `getEnv()` from `src/lib/env.ts` for validated access. Extend the Zod schema when adding Clerk, Convex, or other services.

## Customization

### Styling
- Modify `tailwind.config.js` to add custom colors, fonts, or animations
- Update `src/app/globals.css` for global styles
- Components use Tailwind utility classes for easy customization

### Components
- Add new components in `src/components/`
- Import and use them in your pages
- Follow the existing Hero component pattern

## Deployment

### Build for Production
```bash
bun run build
```

### Start Production Server
```bash
bun run start
```

The template is ready to deploy to Vercel, Netlify, or any other hosting platform that supports Next.js.

## Contributing

This template is designed to be extended and customized. Feel free to:
- Add new components
- Modify the styling
- Extend functionality
- Share your improvements

## License

This project is open source and available under the [MIT License](LICENSE).
