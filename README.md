# Team Project — E‑commerce Phone Store

A responsive phone store built with React 18, TypeScript, and Vite. The project uses SCSS (with Bulma), React Router, React Transition Group, and Font Awesome. Linting and formatting are set up via ESLint, Prettier, and Stylelint. Cypress is included and verified during post‑install. One‑command deployment to GitHub Pages is available.


[Demo](https://team-project-phones-store.netlify.app)


## Tech stack
- React 18 + TypeScript
- Vite
- SCSS + Bulma
- React Router, React Transition Group
- Font Awesome
- ESLint, Prettier, Stylelint
- Cypress

## Key features
- **Catalog & product pages**: phones, tablets, and accessories with sorting, pagination, and detailed product view (images, specs, variants).
- **Cart & checkout**: add/remove/update quantities, guest cart via `localStorage`, authenticated cart persisted in Supabase; Stripe checkout with custom theme.
- **Favorites (Wishlist)**: guest favorites via `localStorage`, sync to Supabase after login.
- **Auth & profiles**: email/password auth using Supabase; profile fetch/update, orders history and order items.
- **Orders**: order creation on successful payment; order items saved with price at purchase.
- **Search**: header search dropdown across catalog.
- **Theming & UI**: Tailwind‑based tokens in `index.css`, dark mode context, shadcn‑ui components, animations, 3D cards.
- **Internationalization (i18n)**: ready configuration for `i18next` (currently commented), English and Ukrainian dictionaries in `src/i18n.ts`.
- **Data migration**: script to seed Supabase from JSON files with image URL transformation and year enrichment.

## Pages and routes (high‑level)
- `/` Home: carousels, categories, brand‑new and hot price lists.
- `/phones`, `/tablets`, `/accessories`: category listings.
- `/product/:productId`: detailed product page with color/capacity variations, add to cart, favorites, zoom.
- `/cart`: cart management and proceed to checkout.
- `/checkout`: Stripe Elements payment.
- `/payment-success`: post‑payment order persistence and success/error statuses.
- `/favourites`: wishlist page.
- `/profile`: user profile and orders overview (after auth).
- `/*`: not found page.

## State management
- **Zustand stores**:
  - `useCartStore`: items, loading, errors, merge/sync for guest ↔ auth, Supabase persistence (`cart_items`).
  - `useFavoritesStore`: favorites merge/sync, Supabase persistence (`favorite_items`).
  - `useAuthStore`: session, profile, orders, order items; initializes on mount and on auth state change.
- **Local storage** for guests: keys `guestCart` and `guestFavorites`.

## Data model (Supabase)
- Tables used: `products`, `cart_items`, `favorite_items`, `profiles`, `orders`, `order_items`.
- `scripts/migrate.mjs` reads JSON under `api/`, enriches with `year` from `api/products.json`, transforms image paths to Supabase Storage public URLs, and inserts into `products`.

## Payments
- Stripe Elements on `/checkout` creates a PaymentIntent via a Supabase edge function `create-payment-intent`.
- On success redirect to `/payment-success`, which saves an `orders` record and related `order_items`, clears cart, and shows the order id.
- Environment variables required: `VITE_STRIPE_PUBLISHABLE_KEY` (client), `STRIPE_SECRET_KEY` (edge function).

## Authentication
- Supabase email/password auth with toasts and validation.
- On sign‑in: guest cart and favorites are merged into the user’s data and persisted.

## Internationalization
- `src/i18n.ts` contains ready‑to‑use `i18next` setup with English and Ukrainian dictionaries (commented out by default). Enable by uncommenting and wiring the provider in app entry.

## Project structure
```text
Project-E-commerce_store/
├─ api/
│  ├─ accessories.json
│  ├─ phones.json
│  ├─ products.json
│  └─ tablets.json
├─ public/
│  ├─ _redirects
│  └─ vite.svg
├─ scripts/
│  └─ migrate.mjs
├─ src/
│  ├─ App.tsx
│  ├─ main.tsx
│  ├─ index.css
│  ├─ assets/
│  ├─ components/
│  │  ├─ ui/ (buttons, cards, dialogs, etc.)
│  │  └─ Pagination/
│  ├─ context/
│  ├─ features/
│  │  ├─ accessories/  ├─ auth/  ├─ cart/  ├─ contacts/
│  │  ├─ credit/       ├─ favourites/  ├─ pageNotFound/
│  │  ├─ phones/       ├─ profile/     ├─ rights/
│  │  └─ tablets/      └─ user/
│  ├─ hooks/
│  ├─ i18n.ts
│  ├─ img/ (... product and banner images ...)
│  ├─ lib/
│  ├─ pages/
│  ├─ types/
│  ├─ utils/
│  └─ vite-env.d.ts
├─ styles/
│  └─ fonts/
├─ supabase/
│  ├─ config.toml
│  └─ functions/
│     └─ create-payment-intent/
│        ├─ deno.json
│        └─ index.ts
├─ components.json
├─ eslint.config.js
├─ netlify.toml
├─ index.html
├─ package.json
├─ package-lock.json
├─ tsconfig.app.json
├─ tsconfig.json
├─ tsconfig.node.json
├─ vite.config.ts
└─ README.md
```

## Styling
Write styles in SCSS. Bulma is included for utility classes and components. Stylelint helps keep styles consistent.
