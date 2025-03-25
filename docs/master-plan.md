# BicaraPintar Frontend Development Masterplan

## Project Overview

BicaraPintar is developing a static frontend landing page using Vue.js/Nuxt 3 with design inspiration from Kata.ai and product/service offerings similar to Volantis. The landing page will later be integrated with a Laravel backend.

### Key Objectives
- Create a visually appealing static frontend landing page with Kata.ai's design aesthetics
- Showcase BicaraPintar's products and services (similar to Volantis but with improved speed)
- Implement a mega menu showing connections between products and solutions
- Ensure all pages are SEO-optimized for Google indexing
- Prepare for future Laravel backend integration

### Tech Stack
- **Frontend**:
  - Framework: Nuxt 3
  - UI Component Library: PrimeVue
  - State Management: Vuex
  - Styling: Tailwind CSS with PrimeVue theming
  - Rich Text Editor: TipTap with Tailwind Typography
- **Backend** (future integration):
  - Framework: Laravel
  - Database: MySQL
  - Authentication: Laravel Sanctum
  - File Storage: Laravel Storage

## Design System

### Brand Identity
- **Company Name**: BicaraPintar
- **Design Inspiration**: Kata.ai
- **Typography**: Inter font family (same as Kata.ai)
- **Logo**: Custom logo (already available)

### Color Palette (based on Kata.ai)
- **Primary**: `#0070FF` (Brandeis Blue)
- **Secondary**: `#F97316` (Energetic orange)
- **Accent**: `#10B981` (Vibrant green)
- **Background**: `#FFFFFF` (Clean white)
- **Surface**: `#F3F4F6` (Soft gray for card backgrounds)
- **Text**: `#111827` (Deep, dark gray)

### PrimeVue Theme Configuration
```javascript
// primevue custom theme
:root {
  --primary-color: #0070FF;
  --primary-color-text: #ffffff;
  --surface-a: #ffffff;
  --surface-b: #f3f4f6;
  --surface-c: #e5e7eb;
  --surface-d: #d1d5db;
  --text-color: #111827;
  --text-color-secondary: #4b5563;
  --font-family: 'Inter', sans-serif;
}
```

### Design Principles
- **Clean & Minimal**: Focus on content with clean layouts and minimal distractions
- **Accessible**: Ensure good contrast and readability across all devices
- **Consistent**: Maintain consistent spacing, typography, and visual elements
- **Responsive**: Optimize for all screen sizes from mobile to desktop

## Site Architecture

### Main Pages
1. **Home**: Landing page with hero section and overview of products/services
2. **Products**: Overview and detailed pages for each product
3. **Services**: Overview and detailed pages for each service
4. **Case Studies**: Success stories and implementation examples
5. **News**: Company updates, blog posts, and industry insights
6. **Contact**: Contact form and company information

### Navigation Structure
- **Main Navigation**: Home, Products, Services, Case Studies, News, Company, Contact
- **Mega Menu**: 
  - Products dropdown with related solutions
  - Services dropdown with associated products
- **Footer Navigation**: Company info, quick links, social media, legal pages

## Component Architecture

### Global Components
- **Navbar**: Main navigation with mega menu (using PrimeVue Menu/MegaMenu)
- **Footer**: Site-wide footer with navigation and contact info
- **MegaMenu**: Expandable navigation with product-solution connections (PrimeVue MegaMenu)
- **CTAButton**: Reusable call-to-action button (styled PrimeVue Button)
- **Breadcrumbs**: Navigation aid for inner pages (PrimeVue Breadcrumb)

### Page-Specific Components
- **HeroSection**: Main banner with headline and primary CTA
- **ProductCard**: Thumbnail display of product with brief info (PrimeVue Card)
- **ServiceCard**: Thumbnail display of service with brief info (PrimeVue Card)
- **CaseStudyCard**: Thumbnail display of case study with brief info (PrimeVue Card)
- **NewsCard**: Thumbnail display of news article with brief info (PrimeVue Card)
- **ContactForm**: Form with validation for user inquiries (PrimeVue InputText, Dropdown, TextArea)

### PrimeVue Components to Use
- **Layout**: Card, Panel, Divider, Splitter
- **Form**: InputText, InputNumber, Dropdown, Calendar, Checkbox, RadioButton, TextArea
- **Data**: DataTable, DataView, Carousel, Timeline
- **Button**: Button, SplitButton, ToggleButton
- **Overlay**: Dialog, OverlayPanel, Tooltip, Menu
- **Media**: Image, Galleria
- **Menu**: Menu, MegaMenu, Menubar, Breadcrumb
- **Messages**: Message, Toast, ConfirmDialog

## Data Architecture

### Data Models

#### Product Model
```javascript
{
  id: Number,
  title: String,
  slug: String,
  shortDescription: String,
  fullDescription: String,
  features: Array[String],
  relatedSolutions: Array[{
    id: Number,
    title: String,
    slug: String
  }],
  image: String
}
```

#### Service Model
```javascript
{
  id: Number,
  title: String,
  slug: String,
  shortDescription: String,
  fullDescription: String,
  benefits: Array[String],
  relatedProducts: Array[{
    id: Number,
    title: String,
    slug: String
  }],
  caseStudies: Array[Number], // IDs of related case studies
  image: String
}
```

#### Case Study Model
```javascript
{
  id: Number,
  title: String,
  slug: String,
  client: String,
  industry: String,
  challenge: String,
  solution: String,
  results: String,
  testimonial: {
    quote: String,
    author: String,
    position: String
  },
  relatedProducts: Array[Number],
  relatedServices: Array[Number],
  images: Array[String]
}
```

### Store Structure (Vuex)
- **products.js**: State, getters, mutations and actions for products
- **services.js**: State, getters, mutations and actions for services
- **caseStudies.js**: State, getters, mutations and actions for case studies
- **news.js**: State, getters, mutations and actions for news articles

## Implementation Plan

### Directory Structure
```
bicarapintar-frontend/
├── assets/
│   ├── css/
│   │   ├── main.css
│   │   └── theme.css    # PrimeVue theme customizations
│   └── images/
│       └── logo.png
├── components/
│   ├── global/
│   │   ├── Navbar.vue
│   │   ├── Footer.vue
│   │   ├── MegaMenu.vue
│   │   └── CTAButton.vue
│   ├── home/
│   │   ├── HeroSection.vue
│   │   ├── ProductsSection.vue
│   │   ├── ServicesSection.vue
│   │   ├── CaseStudiesSection.vue
│   │   └── NewsSection.vue
│   ├── products/
│   │   ├── ProductCard.vue
│   │   └── ProductDetail.vue
│   └── services/
│       ├── ServiceCard.vue
│       └── ServiceDetail.vue
├── layouts/
│   └── default.vue
├── pages/
│   ├── index.vue
│   ├── products/
│   │   ├── index.vue
│   │   └── [slug].vue
│   ├── services/
│   │   ├── index.vue
│   │   └── [slug].vue
│   ├── case-studies/
│   │   ├── index.vue
│   │   └── [slug].vue
│   ├── news/
│   │   ├── index.vue
│   │   └── [slug].vue
│   └── contact.vue
├── store/
│   ├── index.js
│   ├── products.js
│   └── services.js
└── static/
    ├── favicon.ico
    └── robots.txt
```

### Setup and Installation

#### 1. Project Initialization
```bash
npx nuxi init bicarapintar-frontend
cd bicarapintar-frontend

# Install dependencies
npm install vuex@next
npm install @nuxtjs/tailwindcss
npm install primevue primeicons
npm install @tiptap/vue-3 @tiptap/starter-kit @tiptap/extension-typography
```

#### 2. Nuxt Configuration for PrimeVue
```javascript
// nuxt.config.js
export default {
  modules: [
    '@nuxtjs/tailwindcss',
  ],
  
  css: [
    'primevue/resources/themes/saga-blue/theme.css', // Default theme
    'primevue/resources/primevue.css',               // Core styling
    'primeicons/primeicons.css',                     // Icons
    '~/assets/css/theme.css',                        // Your custom theme
  ],
  
  build: {
    transpile: ['primevue']
  },
  
  // Register PrimeVue components globally
  plugins: [
    { src: '~/plugins/primevue.js' }
  ]
}
```

#### 3. PrimeVue Plugin
```javascript
// plugins/primevue.js
import { defineNuxtPlugin } from '#app'
import PrimeVue from 'primevue/config'
import Button from 'primevue/button'
import Card from 'primevue/card'
import Menu from 'primevue/menu'
import MegaMenu from 'primevue/megamenu'
import InputText from 'primevue/inputtext'
import Textarea from 'primevue/textarea'
import Dropdown from 'primevue/dropdown'
import DataTable from 'primevue/datatable'
import Column from 'primevue/column'
import Dialog from 'primevue/dialog'
import Carousel from 'primevue/carousel'
import Toast from 'primevue/toast'
import ToastService from 'primevue/toastservice'

export default defineNuxtPlugin(nuxtApp => {
  nuxtApp.vueApp.use(PrimeVue, { ripple: true })
  nuxtApp.vueApp.use(ToastService)
  
  // Register components
  nuxtApp.vueApp.component('Button', Button)
  nuxtApp.vueApp.component('Card', Card)
  nuxtApp.vueApp.component('Menu', Menu)
  nuxtApp.vueApp.component('MegaMenu', MegaMenu)
  nuxtApp.vueApp.component('InputText', InputText)
  nuxtApp.vueApp.component('Textarea', Textarea)
  nuxtApp.vueApp.component('Dropdown', Dropdown)
  nuxtApp.vueApp.component('DataTable', DataTable)
  nuxtApp.vueApp.component('Column', Column)
  nuxtApp.vueApp.component('Dialog', Dialog)
  nuxtApp.vueApp.component('Carousel', Carousel)
  nuxtApp.vueApp.component('Toast', Toast)
})
```

### Key Component Implementations

#### 1. PrimeVue MegaMenu Component
```vue
<!-- components/global/MegaMenu.vue -->
<template>
  <div class="card">
    <MegaMenu :model="megaMenuItems" />
    <!-- Mobile menu for responsive design -->
    <Menu :model="mobileMenuItems" class="md:hidden" />
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import { useStore } from 'vuex';

const store = useStore();
const products = computed(() => store.state.products.items);
const services = computed(() => store.state.services.items);

// Convert products to menu items format
const productMenuItems = computed(() => {
  return products.value.map(product => {
    const relatedSolutions = product.relatedSolutions.map(solution => ({
      label: solution.title,
      to: `/services/${solution.slug}`
    }));
    
    return {
      label: product.title,
      to: `/products/${product.slug}`,
      items: [
        {
          label: 'Related Solutions',
          items: relatedSolutions
        }
      ]
    };
  });
});

// Convert services to menu items format
const serviceMenuItems = computed(() => {
  return services.value.map(service => {
    const relatedProducts = service.relatedProducts.map(product => ({
      label: product.title,
      to: `/products/${product.slug}`
    }));
    
    return {
      label: service.title,
      to: `/services/${service.slug}`,
      items: [
        {
          label: 'Powered By',
          items: relatedProducts
        }
      ]
    };
  });
});

// Main mega menu structure
const megaMenuItems = ref([
  {
    label: 'Home',
    to: '/',
    icon: 'pi pi-home'
  },
  {
    label: 'Products',
    icon: 'pi pi-box',
    items: [
      [
        {
          label: 'Our Products',
          items: productMenuItems
        }
      ]
    ]
  },
  {
    label: 'Services',
    icon: 'pi pi-cog',
    items: [
      [
        {
          label: 'Our Services',
          items: serviceMenuItems
        }
      ]
    ]
  },
  {
    label: 'Case Studies',
    to: '/case-studies',
    icon: 'pi pi-file'
  },
  {
    label: 'News',
    to: '/news',
    icon: 'pi pi-bell'
  },
  {
    label: 'Company',
    icon: 'pi pi-building',
    items: [
      [
        {
          label: 'About Us',
          items: [
            { label: 'Our Story', to: '/company/about' },
            { label: 'Team', to: '/company/team' },
            { label: 'Careers', to: '/company/careers' }
          ]
        }
      ]
    ]
  },
  {
    label: 'Contact Us',
    to: '/contact',
    icon: 'pi pi-envelope'
  }
]);

// Mobile menu structure (simplified for mobile view)
const mobileMenuItems = ref([
  { label: 'Home', to: '/', icon: 'pi pi-home' },
  { label: 'Products', to: '/products', icon: 'pi pi-box' },
  { label: 'Services', to: '/services', icon: 'pi pi-cog' },
  { label: 'Case Studies', to: '/case-studies', icon: 'pi pi-file' },
  { label: 'News', to: '/news', icon: 'pi pi-bell' },
  { label: 'Company', to: '/company', icon: 'pi pi-building' },
  { label: 'Contact Us', to: '/contact', icon: 'pi pi-envelope' }
]);
</script>

<style scoped>
/* Custom styling to match Kata.ai design aesthetic */
::v-deep .p-megamenu {
  background-color: transparent;
  border: none;
  padding: 0;
}

::v-deep .p-megamenu .p-menuitem-link {
  padding: 0.75rem 1.25rem;
}

::v-deep .p-megamenu .p-menuitem-text {
  color: var(--text-color);
}

::v-deep .p-megamenu .p-menuitem-link:hover .p-menuitem-text {
  color: var(--primary-color);
}
</style>
```

#### 2. Hero Section with PrimeVue Components
```vue
<!-- components/home/HeroSection.vue -->
<template>
  <div class="bg-gradient-to-b from-blue-50 to-white py-16 md:py-28">
    <div class="container mx-auto px-4">
      <div class="flex flex-col md:flex-row items-center">
        <div class="md:w-1/2 mb-8 md:mb-0">
          <h1 class="text-4xl md:text-5xl lg:text-6xl font-bold mb-4 text-text">
            Turn Scattered Data into <span class="text-primary">AI-Powered Insights</span>
          </h1>
          <p class="text-lg md:text-xl text-gray-600 mb-8">
            BicaraPintar helps enterprises unify their data sources and leverage AI to extract actionable insights and build automated solutions.
          </p>
          <div class="flex flex-col sm:flex-row gap-4">
            <Button label="Get Started" class="p-button-primary p-button-lg" />
            <Button label="Schedule Demo" class="p-button-outlined p-button-lg" />
          </div>
        </div>
        <div class="md:w-1/2">
          <img src="~/assets/images/hero-illustration.svg" alt="BicaraPintar Platform" class="w-full" />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
// No additional script needed
</script>
```

#### 3. Product Detail Page with PrimeVue
```vue
<!-- pages/products/[slug].vue -->
<template>
  <div>
    <div class="bg-surface py-12">
      <div class="container mx-auto px-4">
        <Breadcrumb :model="breadcrumbItems" class="mb-6" />
        
        <div class="flex flex-col md:flex-row items-center">
          <div class="md:w-1/2 mb-8 md:mb-0">
            <h1 class="text-3xl md:text-4xl font-bold mb-4">{{ product.title }}</h1>
            <p class="text-lg text-gray-600 mb-6">{{ product.fullDescription }}</p>
            <div class="mb-6">
              <h3 class="text-xl font-semibold mb-3">Key Features</h3>
              <ul class="space-y-2">
                <li v-for="(feature, index) in product.features" :key="index" class="flex items-start">
                  <i class="pi pi-check text-primary mr-2"></i>
                  <span>{{ feature }}</span>
                </li>
              </ul>
            </div>
            <Button label="Request Demo" class="p-button-primary" icon="pi pi-arrow-right" iconPos="right" />
          </div>
          <div class="md:w-1/2">
            <img :src="product.image" :alt="product.title" class="w-full rounded-lg shadow-lg" />
          </div>
        </div>
      </div>
    </div>
    
    <div class="py-12">
      <div class="container mx-auto px-4">
        <h2 class="text-2xl font-bold mb-8 text-center">Solutions Powered by {{ product.title }}</h2>
        
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
          <Card v-for="solution in product.relatedSolutions" :key="solution.id" class="shadow-md hover:shadow-lg transition">
            <template #header>
              <div class="h-2 bg-primary"></div>
            </template>
            <template #title>
              {{ solution.title }}
            </template>
            <template #content>
              <p class="text-gray-600 mb-4">How {{ product.title }} enables this solution...</p>
            </template>
            <template #footer>
              <Button label="Learn more" icon="pi pi-arrow-right" iconPos="right" 
                      @click="$router.push(`/services/${solution.slug}`)" 
                      class="p-button-text p-button-primary" />
            </template>
          </Card>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, ref } from 'vue';
import { useStore } from 'vuex';
import { useRoute } from 'vue-router';

const store = useStore();
const route = useRoute();
const slug = route.params.slug;

const product = computed(() => store.getters['products/getProductBySlug'](slug));

// Breadcrumb configuration
const breadcrumbItems = ref([
  { label: 'Home', to: '/' },
  { label: 'Products', to: '/products' },
  { label: product.value?.title || 'Product Details', to: '' }
]);
</script>
```

#### 4. Services Section with PrimeVue Carousel
```vue
<!-- components/home/ServicesSection.vue -->
<template>
  <div class="py-16 bg-surface">
    <div class="container mx-auto px-4">
      <div class="text-center mb-12">
        <h2 class="text-3xl md:text-4xl font-bold mb-4">Our Services</h2>
        <p class="text-lg text-gray-600 max-w-2xl mx-auto">
          We offer a comprehensive range of services to help your business leverage data and AI for growth.
        </p>
      </div>
      
      <Carousel :value="services" :numVisible="3" :numScroll="1" :responsiveOptions="responsiveOptions" class="shadow-lg">
        <template #header>
          <div class="flex justify-between items-center">
            <h3 class="text-xl font-semibold">Featured Services</h3>
            <Button label="View All" class="p-button-text" @click="$router.push('/services')" />
          </div>
        </template>
        <template #item="slotProps">
          <div class="p-4">
            <Card>
              <template #header>
                <img :src="slotProps.data.image" :alt="slotProps.data.title" class="w-full h-48 object-cover" />
              </template>
              <template #title>
                {{ slotProps.data.title }}
              </template>
              <template #subtitle>
                <span class="text-sm text-gray-500">Digital Solutions</span>
              </template>
              <template #content>
                <p class="line-clamp-3">{{ slotProps.data.shortDescription }}</p>
              </template>
              <template #footer>
                <Button 
                  label="Learn More" 
                  icon="pi pi-arrow-right" 
                  @click="$router.push(`/services/${slotProps.data.slug}`)" 
                  class="p-button-outlined" 
                />
              </template>
            </Card>
          </div>
        </template>
      </Carousel>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import { useStore } from 'vuex';

const store = useStore();
const services = computed(() => store.state.services.items);

const responsiveOptions = ref([
  {
    breakpoint: '1024px',
    numVisible: 3,
    numScroll: 1
  },
  {
    breakpoint: '768px',
    numVisible: 2,
    numScroll: 1
  },
  {
    breakpoint: '560px',
    numVisible: 1,
    numScroll: 1
  }
]);
</script>
```

#### 5. Contact Form with PrimeVue Form Components
```vue
<!-- pages/contact.vue -->
<template>
  <div class="py-12">
    <div class="container mx-auto px-4">
      <div class="text-center mb-12">
        <h1 class="text-3xl md:text-4xl font-bold mb-4">Contact Us</h1>
        <p class="text-lg text-gray-600 max-w-2xl mx-auto">
          Have questions or ready to start your digital transformation journey? Get in touch with our team.
        </p>
      </div>
      
      <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
        <div>
          <h2 class="text-2xl font-semibold mb-6">Send Us a Message</h2>
          
          <form @submit.prevent="submitForm" class="space-y-6">
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
              <div class="p-field">
                <label for="firstName" class="block text-sm font-medium text-gray-700 mb-1">First Name*</label>
                <InputText id="firstName" v-model="form.firstName" class="w-full" :class="{'p-invalid': submitted && !form.firstName}" />
                <small v-if="submitted && !form.firstName" class="p-error">First name is required.</small>
              </div>
              
              <div class="p-field">
                <label for="lastName" class="block text-sm font-medium text-gray-700 mb-1">Last Name*</label>
                <InputText id="lastName" v-model="form.lastName" class="w-full" :class="{'p-invalid': submitted && !form.lastName}" />
                <small v-if="submitted && !form.lastName" class="p-error">Last name is required.</small>
              </div>
            </div>
            
            <div class="p-field">
              <label for="email" class="block text-sm font-medium text-gray-700 mb-1">Email*</label>
              <InputText id="email" v-model="form.email" class="w-full" :class="{'p-invalid': submitted && !form.email}" />
              <small v-if="submitted && !form.email" class="p-error">Email is required.</small>
            </div>
            
            <div class="p-field">
              <label for="company" class="block text-sm font-medium text-gray-700 mb-1">Company</label>
              <InputText id="company" v-model="form.company" class="w-full" />
            </div>
            
            <div class="p-field">
              <label for="subject" class="block text-sm font-medium text-gray-700 mb-1">Subject*</label>
              <Dropdown id="subject" v-model="form.subject" :options="subjectOptions" optionLabel="name" placeholder="Select a subject" class="w-full" :class="{'p-invalid': submitted && !form.subject}" />
              <small v-if="submitted && !form.subject" class="p-error">Subject is required.</small>
            </div>
            
            <div class="p-field">
              <label for="message" class="block text-sm font-medium text-gray-700 mb-1">Message*</label>
              <Textarea id="message" v-model="form.message" rows="5" class="w-full" :class="{'p-invalid': submitted && !form.message}" />
              <small v-if="submitted && !form.message" class="p-error">Message is required.</small>
            </div>
            
            <div>
              <Button type="submit" label="Send Message" icon="pi pi-send" class="p-button-primary" />
            </div>
          </form>
        </div>
        
        <div>
          <h2 class="text-2xl font-semibold mb-6">Get in Touch</h2>
          
          <div class="bg-white shadow-md rounded-lg p-6 mb-6">
            <h3 class="text-xl font-semibold mb-4">Our Office</h3>
            <div class="space-y-4">
              <div class="flex items-start">
                <i class="pi pi-map-marker text-primary mr-3 mt-1"></i>
                <div>
                  <p class="font-medium">Head Office</p>
                  <p>Equity Tower, 37th Floor, Unit D & H</p>
                  <p>Jend. Sudirman Kav. 52-53 (SCBD)</p>
                  <p>South Jakarta 12190, Indonesia</p>
                </div>
              </div>
              
              <div class="flex items-start">
                <i class="pi pi-phone text-primary mr-3 mt-1"></i>
                <div>
                  <p class="font-medium">Phone</p>
                  <p>+62-811-2521-6666</p>
                </div>
              </div>
              
              <div class="flex items-start">
                <i class="pi pi-envelope text-primary mr-3 mt-1"></i>
                <div>
                  <p class="font-medium">Email</p>
                  <p>info@bicarapintar.com</p>
                </div>
              </div>
            </div>
          </div>
          
          <div class="bg-white shadow-md rounded-lg p-6">
            <h3 class="text-xl font-semibold mb-4">Connect With Us</h3>
            <div class="flex space-x-4">
              <a href="#" class="w-10 h-10 rounded-full bg-blue-100 flex items-center justify-center text-primary hover:bg-primary hover:text-white transition">
                <i class="pi pi-facebook"></i>
              </a>
              <a href="#" class="w-10 h-10 rounded-full bg-blue-100 flex items-center justify-center text-primary hover:bg-primary hover:text-white transition">
                <i class="pi pi-twitter"></i>
              </a>
              <a href="#" class="w-10 h-10 rounded-full bg-blue-100 flex items-center justify-center text-primary hover:bg-primary hover:text-white transition">
                <i class="pi pi-linkedin"></i>
              </a>
              <a href="#" class="w-10 h-10 rounded-full bg-blue-100 flex items-center justify-center text-primary hover:bg-primary hover:text-white transition">
                <i class="pi pi-instagram"></i>
              </a>
            </div>
          </div>
        </div>
      </div>
    </div>
    
    <Toast />
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { useToast } from 'primevue/usetoast';

const toast = useToast();
const submitted = ref(false);

const form = ref({
  firstName: '',
  lastName: '',
  email: '',
  company: '',
  subject: null,
  message: ''
});

const subjectOptions = ref([
  { name: 'General Inquiry', value: 'general' },
  { name: 'Product Information', value: 'product' },
  { name: 'Service Information', value: 'service' },
  { name: 'Partnership Opportunity', value: 'partnership' },
  { name: 'Technical Support', value: 'support' }
]);

const submitForm = () => {
  submitted.value = true;
  
  // Validate form
  if (!form.value.firstName || !form.value.lastName || !form.value.email || !form.value.subject || !form.value.message) {
    toast.add({
      severity: 'error',
      summary: 'Error',
      detail: 'Please fill in all required fields',
      life: 3000
    });
    return;
  }
  
  // Form is valid, proceed with submission
  // In a real app, you would send this data to your backend
  console.log('Form submitted:', form.value);
  
  toast.add({
    severity: 'success',
    summary: 'Success',
    detail: 'Your message has been sent. We\'ll get back to you soon!',
    life: 5000
  });
  
  // Reset form
  form.value = {
    firstName: '',
    lastName: '',
    email: '',
    company: '',
    subject: null,
    message: ''
  };
  submitted.value = false;
};
</script>
```

## SEO Optimization

### 1. Nuxt Configuration for SEO
```javascript
// nuxt.config.js
export default {
  head: {
    title: 'BicaraPintar - AI-Powered Data Solutions',
    meta: [
      { charset: 'utf-8' },
      { name: 'viewport', content: 'width=device-width, initial-scale=1' },
      {
        hid: 'description',
        name: 'description',
        content: 'BicaraPintar helps enterprises unify their data sources and leverage AI to extract actionable insights and build automated solutions.'
      },
      { name: 'format-detection', content: 'telephone=no' }
    ],
    link: [
      { rel: 'icon', type: 'image/x-icon', href: '/favicon.ico' },
      { rel: 'preconnect', href: 'https://fonts.googleapis.com' },
      { rel: 'preconnect', href: 'https://fonts.gstatic.com', crossorigin: true },
      { rel: 'stylesheet', href: 'https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap' }
    ]
  },
  
  // For dynamic routes like product and service pages
  generate: {
    routes() {
      const products = require('./store/products').state().items
      const services = require('./store/services').state().items
      
      const productRoutes = products.map(product => `/products/${product.slug}`)
      const serviceRoutes = services.map(service => `/services/${service.slug}`)
      
      return [...productRoutes, ...serviceRoutes]
    }
  },
  
  // Additional modules
  modules: [
    '@nuxtjs/tailwindcss',
    '@nuxtjs/sitemap'
  ],
  
  // Sitemap configuration
  sitemap: {
    hostname: 'https://bicarapintar.com',
    gzip: true,
  },
}
```

### 2. Meta Tags for Dynamic Pages
```vue
<!-- pages/products/[slug].vue -->
<script setup>
import { computed, useHead } from 'nuxt/app';
import { useStore } from 'vuex';
import { useRoute } from 'vue-router';

const store = useStore();
const route = useRoute();
const slug = route.params.slug;

const product = computed(() => store.getters['products/getProductBySlug'](slug));

// Set dynamic meta tags for SEO
useHead({
  title: `${product.value?.title || 'Product'} - BicaraPintar`,
  meta: [
    {
      hid: 'description',
      name: 'description',
      content: product.value?.shortDescription || 'BicaraPintar product'
    },
    {
      hid: 'og:title',
      property: 'og:title',
      content: `${product.value?.title || 'Product'} - BicaraPintar`
    },
    {
      hid: 'og:description',
      property: 'og:description',
      content: product.value?.shortDescription || 'BicaraPintar product'
    },
    {
      hid: 'og:image',
      property: 'og:image',
      content: product.value?.image || '/images/default-product.png'
    }
  ]
});
</script>
```

## Development Timeline

### Week 1: Setup and Foundation
- **Day 1-2**: Project initialization and dependency setup
  - Create Nuxt 3 project
  - Set up Tailwind CSS and PrimeVue
  - Configure theme to match Kata.ai design
- **Day 3-4**: Basic layout components
  - Create default layout
  - Implement Navbar with PrimeVue MegaMenu
  - Create Footer component
- **Day 5**: Data structure and store setup
  - Define data models
  - Set up Vuex store with dummy data
  - Create initial API service layer

### Week 2: Core Pages and Components
- **Day 1-2**: Homepage development
  - Create Hero section
  - Implement Products and Services sections
  - Add Case Studies and News preview sections
- **Day 3-4**: Product and Service pages
  - Create listing pages for Products and Services
  - Develop detailed pages for individual Products/Services
  - Implement connection between Products and Solutions
- **Day 5**: Case Studies and News pages
  - Create listing pages
  - Develop individual detail pages

### Week 3: Advanced Features and Refinement
- **Day 1-2**: Contact page and forms
  - Create Contact page with PrimeVue form components
  - Implement form validation
  - Add maps and contact information
- **Day 3-4**: Responsive design
  - Test and optimize for mobile devices
  - Create mobile menu alternatives
  - Fix any responsive layout issues
- **Day 5**: Visual refinements
  - Add animations and transitions
  - Refine component styling
  - Ensure design consistency

### Week 4: Optimization and Deployment
- **Day 1-2**: SEO optimization
  - Implement meta tags
  - Generate sitemap
  - Add structured data for rich snippets
- **Day 3-4**: Performance optimization
  - Optimize image loading
  - Implement lazy loading for components
  - Test and improve page speed
- **Day 5**: Final testing and deployment
  - Cross-browser testing
  - Set up deployment pipeline
  - Launch static site

## Future Integration with Laravel Backend

To prepare for future integration with Laravel backend:

1. **API Architecture**
   - Create API service layer in Nuxt frontend
   - Design endpoints to match Laravel API structure
   - Use environment variables to switch between static data and API endpoints

2. **Authentication Integration**
   - Prepare for Laravel Sanctum authentication
   - Create login/register components (disabled in static version)
   - Set up auth middleware for protected routes

3. **File Upload Preparation**
   - Create file upload components compatible with Laravel Storage
   - Implement image preview functionality
   - Set up placeholder functions for future API integration

## Conclusion

This masterplan outlines a comprehensive approach to building a static frontend landing page for BicaraPintar, with design inspiration from Kata.ai and product/service offerings similar to Volantis. By leveraging Nuxt 3, PrimeVue, and Tailwind CSS, we can create a modern, responsive, and visually appealing website that effectively showcases BicaraPintar's products and services.

The integration of PrimeVue components will accelerate development and ensure a consistent UI experience, while Tailwind CSS will allow for custom styling to match the Kata.ai design aesthetic. The product-solution connection feature will help visitors understand how BicaraPintar's products can solve their specific problems through various solutions.

With careful attention to SEO optimization, responsive design, and future Laravel backend integration, this static frontend will provide a solid foundation for BicaraPintar's online presence and can be easily extended as the business grows.