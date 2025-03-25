# Bicara Pintar Frontend Project Structure

## Project Overview
This is a Nuxt.js frontend project for the Bicara Pintar application, organized with a modular and scalable architecture.

## Root Directory Structure
- `.clinerules-shorter-code-prompt`: Custom CLI rules configuration
- `.env.development`: Development environment variables
- `.roo/`: Roo system configuration directory
- `bicarapintar-frontend/`: Main project directory

## Detailed Project Structure

### Configuration Files
- `nuxt.config.js`: Nuxt.js configuration
- `package.json`: Project dependencies and scripts
- `tailwind.config.js`: Tailwind CSS configuration
- `tsconfig.json`: TypeScript configuration
- `.nvmrc`: Node version specification
- `netlify.toml`: Netlify deployment configuration

### Directory Breakdown

#### 1. `assets/`
Stores static assets for the project
- `css/`: Global CSS files
- `images/`: Static image assets
- `primeicons/`: PrimeVue icon assets
- `primevue/`: PrimeVue UI component assets

#### 2. `components/`
Vue components organized by feature/section
- `global/`: Reusable global components
- `home/`: Components specific to the home page
- `products/`: Product-related components
- `services/`: Service-related components
- `solutions/`: Solution-related components

#### 3. `composables/`
Vue composition functions
- `useSEO.js`: SEO-related composable

#### 4. `data/`
Static data files
- `products.js`: Product data
- `services.js`: Services data

#### 5. `layouts/`
Vue layout components
- `default.vue`: Default application layout

#### 6. `middleware/`
Nuxt middleware functions
- `adminAuth.js`: Admin authentication middleware
- `redirects.js`: Page redirection logic

#### 7. `pages/`
Nuxt.js page components
- Root pages like `index.vue`, `contact.vue`, `error.vue`
- Nested route pages for:
  - `company/`
  - `legal/`
  - `news/`
  - `products/`
  - `services/`
  - `solutions/`

#### 8. `plugins/`
Nuxt.js plugins
- `performance.js`: Performance optimization
- `primevue.js`: PrimeVue integration
- `toast.js`: Toast notification plugin
- `vuex.js`: Vuex store plugin

#### 9. `public/`
Public static files
- `robots.txt`: Search engine crawler instructions
- `images/`: Public image assets

#### 10. `server/`
Server-side configurations
- `tsconfig.json`: TypeScript server configuration

#### 11. `services/`
API service layers
- `adminApi.js`: Admin-related API calls
- `api.js`: General API service
- `dataAdapter.js`: Data transformation utilities

#### 12. `store/`
Vuex store modules
- `index.js`: Root store
- Specific store modules:
  - `caseStudies.js`
  - `news.js`
  - `products.js`
  - `services.js`
  - `solutions.js`
  - `admin/`: Admin-related store modules

#### 13. `utils/`
Utility functions and helpers
- `designSystem.js`: Design system utilities
- `migrationHelper.js`: Data migration helpers
- `storeHelpers.js`: Store-related utilities
- `structuredData.js`: Structured data generation

## Key Technologies
- Nuxt.js
- Vue 3
- Vuex
- Tailwind CSS
- PrimeVue
- TypeScript

## Deployment
Configured for Netlify deployment with `netlify.toml`