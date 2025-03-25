# BicaraPintar CMS Admin Implementation Guide

## Table of Contents
- [BicaraPintar CMS Admin Implementation Guide](#bicarapintar-cms-admin-implementation-guide)
  - [Table of Contents](#table-of-contents)
  - [Introduction](#introduction)
  - [Architecture Overview](#architecture-overview)
    - [High-Level Architecture](#high-level-architecture)
  - [Technical Requirements](#technical-requirements)
    - [Dependencies](#dependencies)
    - [Browser Compatibility](#browser-compatibility)
    - [Responsive Design](#responsive-design)
  - [Module Implementation](#module-implementation)
    - [Authentication \& Authorization](#authentication--authorization)
      - [Login System](#login-system)
      - [Role-Based Access Control](#role-based-access-control)
    - [Dashboard](#dashboard)
    - [Content Management](#content-management)
      - [Common Features](#common-features)
      - [Content Type-Specific Features](#content-type-specific-features)
    - [Media Management](#media-management)
    - [User Management](#user-management)
    - [TipTap Editor Integration](#tiptap-editor-integration)
  - [Component Structure](#component-structure)
    - [Reusable Components](#reusable-components)
    - [Layout Components](#layout-components)
  - [API Integration](#api-integration)
    - [Admin API Service](#admin-api-service)
    - [State Management](#state-management)
  - [Deployment Strategy](#deployment-strategy)
  - [Testing Plan](#testing-plan)
    - [Unit Testing](#unit-testing)
    - [Integration Testing](#integration-testing)
    - [End-to-End Testing](#end-to-end-testing)
  - [Timeline \& Milestones](#timeline--milestones)
    - [Phase 1: Foundation (2 weeks)](#phase-1-foundation-2-weeks)
    - [Phase 2: Core Components (3 weeks)](#phase-2-core-components-3-weeks)
    - [Phase 3: Content Management (4 weeks)](#phase-3-content-management-4-weeks)
    - [Phase 4: User Management (2 weeks)](#phase-4-user-management-2-weeks)
    - [Phase 5: Refinement (3 weeks)](#phase-5-refinement-3-weeks)
    - [Phase 6: Testing and Deployment (2 weeks)](#phase-6-testing-and-deployment-2-weeks)

## Introduction

This document outlines the implementation plan for BicaraPintar's CMS Admin system, which will be built using Vue.js, Tailwind CSS, and TipTap Editor. The CMS will provide a comprehensive interface for managing all content on the BicaraPintar website, including products, services, solutions, case studies, news articles, and media items.

The implementation will follow a modular approach, with reusable components and a clear separation of concerns. The system will integrate with the backend API as specified in the backend implementation guide, providing a seamless experience for content managers and administrators.

## Architecture Overview

The CMS Admin will be implemented as a separate section within the existing Nuxt.js application, with its own routing, state management, and component structure. This approach allows for:

1. **Separation of concerns**: The public-facing website and admin interface are logically separated
2. **Code splitting**: Admin-specific code is only loaded when needed
3. **Security**: Admin routes can be protected with authentication middleware
4. **Maintainability**: Changes to the admin interface don't affect the public website

### High-Level Architecture

```
bicarapintar-frontend/
├── pages/
│   ├── admin/                  # Admin routes
│   │   ├── index.vue           # Admin dashboard
│   │   ├── login.vue           # Admin login
│   │   ├── products/           # Product management
│   │   ├── services/           # Service management
│   │   ├── solutions/          # Solution management
│   │   ├── case-studies/       # Case study management
│   │   ├── news/               # News management
│   │   ├── media/              # Media management
│   │   └── users/              # User management
├── components/
│   ├── admin/                  # Admin-specific components
│   │   ├── common/             # Reusable admin components
│   │   ├── dashboard/          # Dashboard components
│   │   ├── editor/             # TipTap editor components
│   │   ├── forms/              # Form components
│   │   ├── layout/             # Admin layout components
│   │   └── tables/             # Data table components
├── store/
│   ├── admin/                  # Admin-specific store modules
├── services/
│   ├── adminApi.js             # Admin API service
├── middleware/
│   ├── adminAuth.js            # Admin authentication middleware
└── layouts/
    └── admin.vue               # Admin layout
```

## Technical Requirements

### Dependencies

The following dependencies will be added to the project:

1. **TipTap Editor**:
   - @tiptap/vue-3
   - @tiptap/starter-kit
   - @tiptap/extension-image
   - @tiptap/extension-link
   - @tiptap/extension-table
   - @tiptap/extension-text-align
   - @tiptap/extension-underline
   - @tiptap/extension-placeholder

2. **UI Components**:
   - PrimeVue components (already in the project)
   - Tailwind CSS (already in the project)

3. **State Management**:
   - Vuex (already in the project)

4. **Authentication**:
   - JWT handling (using existing API service)

5. **Form Validation**:
   - Vuelidate or similar form validation library

### Browser Compatibility

The admin interface will support:
- Chrome (latest 2 versions)
- Firefox (latest 2 versions)
- Safari (latest 2 versions)
- Edge (latest 2 versions)

### Responsive Design

The admin interface will be responsive, with optimized layouts for:
- Desktop (1024px and above)
- Tablet (768px to 1023px)
- Mobile (below 768px)

## Module Implementation

### Authentication & Authorization

#### Login System

The login system will use JWT authentication, integrating with the backend's `/api/auth/login` endpoint. Features include:

- Login form with validation
- Remember me functionality
- Password reset flow
- Session management
- Automatic token refresh
- Secure token storage

#### Role-Based Access Control

The system will implement role-based access control based on the backend's permission system:

- Super Admin: Full access to all features
- Content Manager: Manage content but not users or settings
- Marketing Admin: Limited to news, media, and contact submissions

Implementation:
```javascript
// adminAuth.js middleware
export default function ({ store, redirect, route }) {
  // Check if user is authenticated
  if (!store.state.admin.auth.isAuthenticated) {
    return redirect('/admin/login')
  }
  
  // Check permissions for the route
  const requiredPermission = route.meta.permission
  if (requiredPermission && !store.getters['admin/auth/hasPermission'](requiredPermission)) {
    return redirect('/admin/unauthorized')
  }
}
```

### Dashboard

The dashboard will provide an overview of the system with:

- Key metrics (content counts, recent activity)
- Quick access to common tasks
- Recent content changes
- System notifications
- Performance indicators

Implementation approach:
- Modular dashboard widgets
- Data fetched from `/api/admin/dashboard/stats` endpoint
- Customizable layout based on user role
- Real-time updates for critical metrics

### Content Management

#### Common Features

All content types will share these features:

- List view with filtering, sorting, and pagination
- Detail view with form validation
- Rich text editing with TipTap
- Media selection and management
- Version history and comparison
- Preview functionality
- Bulk operations (publish/unpublish, delete)
- Status indicators (draft, published, scheduled)

#### Content Type-Specific Features

1. **Products Management**:
   - Feature management
   - Related services linking
   - Solution association

2. **Services Management**:
   - Benefit management
   - Related products linking
   - Solution association

3. **Solutions Management**:
   - Industry categorization
   - Challenge and results tracking
   - Case study association
   - Product and service linking

4. **Case Studies Management**:
   - Client information
   - Testimonial management
   - Gallery management
   - Product and service association

5. **News & Media Management**:
   - Article publishing with scheduling
   - Author assignment
   - Category management
   - Featured article selection
   - Media item cataloging

### Media Management

The media management system will provide:

- Upload interface (drag-and-drop, multi-file)
- Image editing (crop, resize, optimize)
- Media library with search and filtering
- Usage tracking (where media is used)
- Categorization and tagging
- Metadata editing
- Image optimization

Implementation:
```javascript
// Media upload service
export const mediaApi = {
  upload: (file, options = {}) => {
    const formData = new FormData()
    formData.append('file', file)
    
    if (options.alt) formData.append('alt', options.alt)
    if (options.category) formData.append('category', options.category)
    
    return api.post('/admin/media/upload', formData, {
      headers: {
        'Content-Type': 'multipart/form-data'
      }
    })
  },
  
  getAll: (params = {}) => api.get('/admin/media', { params }),
  
  delete: (id) => api.delete(`/admin/media/${id}`)
}
```

### User Management

The user management module will include:

- User listing with filtering and sorting
- User creation and editing
- Role assignment
- Permission management
- Activity logging
- Password management
- Account status control

### TipTap Editor Integration

The TipTap editor will be implemented as a reusable component with:

- Text formatting (bold, italic, underline, etc.)
- Headings and paragraph styles
- Lists (ordered and unordered)
- Tables with formatting
- Image insertion and alignment
- Link management
- Code blocks with syntax highlighting
- Custom components (callouts, product cards, etc.)

Implementation:
```javascript
// Basic TipTap editor component
import { Editor, EditorContent } from '@tiptap/vue-3'
import StarterKit from '@tiptap/starter-kit'
import Image from '@tiptap/extension-image'
import Link from '@tiptap/extension-link'
import Table from '@tiptap/extension-table'
import TableRow from '@tiptap/extension-table-row'
import TableCell from '@tiptap/extension-table-cell'
import TableHeader from '@tiptap/extension-table-header'
import TextAlign from '@tiptap/extension-text-align'
import Underline from '@tiptap/extension-underline'
import Placeholder from '@tiptap/extension-placeholder'

export default {
  components: {
    EditorContent,
  },
  props: {
    modelValue: {
      type: String,
      default: '',
    },
    placeholder: {
      type: String,
      default: 'Start writing...',
    },
  },
  emits: ['update:modelValue'],
  data() {
    return {
      editor: null,
    }
  },
  mounted() {
    this.editor = new Editor({
      extensions: [
        StarterKit,
        Image,
        Link.configure({
          openOnClick: false,
        }),
        Table.configure({
          resizable: true,
        }),
        TableRow,
        TableCell,
        TableHeader,
        TextAlign.configure({
          types: ['heading', 'paragraph'],
        }),
        Underline,
        Placeholder.configure({
          placeholder: this.placeholder,
        }),
      ],
      content: this.modelValue,
      onUpdate: () => {
        this.$emit('update:modelValue', this.editor.getJSON())
      },
    })
  },
  beforeUnmount() {
    this.editor.destroy()
  },
}
```

## Component Structure

### Reusable Components

1. **DataTable**: Configurable table with sorting, filtering, and pagination
2. **FormBuilder**: Dynamic form generation based on schema
3. **MediaSelector**: Media library browser and selector
4. **RichTextEditor**: TipTap editor wrapper with toolbar
5. **FilterPanel**: Advanced filtering interface
6. **StatusBadge**: Content status indicator
7. **ConfirmDialog**: Confirmation dialog for destructive actions
8. **Breadcrumbs**: Navigation breadcrumbs
9. **Notifications**: Toast notifications system
10. **ActionMenu**: Context menu for item actions

### Layout Components

1. **AdminLayout**: Main admin layout with navigation
2. **Sidebar**: Collapsible sidebar navigation
3. **TopBar**: Top navigation bar with user menu
4. **ContentHeader**: Page header with actions
5. **TabPanel**: Tabbed interface for content sections

## API Integration

### Admin API Service

A dedicated admin API service will be created to handle all admin-specific API calls:

```javascript
// adminApi.js
import api from './api'

// Products API
export const adminProductsApi = {
  getAll: (params = {}) => api.get('/admin/products', { params }),
  getById: (id) => api.get(`/admin/products/${id}`),
  create: (data) => api.post('/admin/products', data),
  update: (id, data) => api.put(`/admin/products/${id}`, data),
  delete: (id) => api.delete(`/admin/products/${id}`),
  bulkAction: (ids, action) => api.post('/admin/products/bulk', { ids, action }),
}

// Services API
export const adminServicesApi = {
  getAll: (params = {}) => api.get('/admin/services', { params }),
  getById: (id) => api.get(`/admin/services/${id}`),
  create: (data) => api.post('/admin/services', data),
  update: (id, data) => api.put(`/admin/services/${id}`, data),
  delete: (id) => api.delete(`/admin/services/${id}`),
  bulkAction: (ids, action) => api.post('/admin/services/bulk', { ids, action }),
}

// Similar patterns for other content types...

// Dashboard API
export const adminDashboardApi = {
  getStats: () => api.get('/admin/dashboard/stats'),
  getRecentActivity: () => api.get('/admin/dashboard/activity'),
}

// User management API
export const adminUsersApi = {
  getAll: (params = {}) => api.get('/admin/users', { params }),
  getById: (id) => api.get(`/admin/users/${id}`),
  create: (data) => api.post('/admin/users', data),
  update: (id, data) => api.put(`/admin/users/${id}`, data),
  delete: (id) => api.delete(`/admin/users/${id}`),
  getRoles: () => api.get('/admin/roles'),
  getPermissions: () => api.get('/admin/permissions'),
}
```

### State Management

Vuex store modules will be created for each content type:

```javascript
// store/admin/products.js
export const state = () => ({
  items: [],
  currentItem: null,
  loading: false,
  error: null,
  pagination: {
    page: 1,
    perPage: 10,
    total: 0,
  },
  filters: {},
})

export const mutations = {
  SET_ITEMS(state, items) {
    state.items = items
  },
  SET_CURRENT_ITEM(state, item) {
    state.currentItem = item
  },
  SET_LOADING(state, loading) {
    state.loading = loading
  },
  SET_ERROR(state, error) {
    state.error = error
  },
  SET_PAGINATION(state, pagination) {
    state.pagination = { ...state.pagination, ...pagination }
  },
  SET_FILTERS(state, filters) {
    state.filters = filters
  },
}

export const actions = {
  async fetchItems({ commit, state }) {
    commit('SET_LOADING', true)
    try {
      const params = {
        page: state.pagination.page,
        per_page: state.pagination.perPage,
        ...state.filters,
      }
      const response = await adminProductsApi.getAll(params)
      commit('SET_ITEMS', response.data.data)
      commit('SET_PAGINATION', {
        total: response.data.meta.total,
      })
    } catch (error) {
      commit('SET_ERROR', error.message)
    } finally {
      commit('SET_LOADING', false)
    }
  },
  
  async fetchItem({ commit }, id) {
    commit('SET_LOADING', true)
    try {
      const response = await adminProductsApi.getById(id)
      commit('SET_CURRENT_ITEM', response.data)
    } catch (error) {
      commit('SET_ERROR', error.message)
    } finally {
      commit('SET_LOADING', false)
    }
  },
  
  // Additional actions for create, update, delete, etc.
}
```

## Deployment Strategy

The admin interface will be included in the main application build but will be code-split to ensure optimal loading performance. The deployment process will include:

1. **Development Environment**:
   - Local development with mock API
   - Integration with development backend

2. **Staging Environment**:
   - Full integration testing
   - User acceptance testing
   - Performance testing

3. **Production Environment**:
   - Phased rollout
   - Monitoring and analytics
   - Backup and recovery procedures

## Testing Plan

### Unit Testing

- Component testing with Vue Test Utils
- Store module testing
- Service testing with mocked API responses

### Integration Testing

- Form submission flows
- Authentication and authorization
- API integration

### End-to-End Testing

- User journeys for common tasks
- Cross-browser compatibility
- Responsive design testing

## Timeline & Milestones

### Phase 1: Foundation (2 weeks)
- Project setup and dependency installation
- Admin layout and navigation
- Authentication system
- Basic routing and state management

### Phase 2: Core Components (3 weeks)
- TipTap editor integration
- Data table component
- Form builder component
- Media selector component
- Dashboard widgets

### Phase 3: Content Management (4 weeks)
- Products management
- Services management
- Solutions management
- Case studies management
- News and media management

### Phase 4: User Management (2 weeks)
- User listing and filtering
- User creation and editing
- Role and permission management
- Activity logging

### Phase 5: Refinement (3 weeks)
- Performance optimization
- Accessibility improvements
- Documentation
- User training materials

### Phase 6: Testing and Deployment (2 weeks)
- Comprehensive testing
- Bug fixing
- Deployment to staging
- Final deployment to production
