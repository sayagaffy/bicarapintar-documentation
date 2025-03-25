# BicaraPintar Backend Implementation Guide

## Table of Contents
- [Introduction](#introduction)
- [Technical Specifications](#technical-specifications)
- [API Endpoints](#api-endpoints)
- [Entity Relationship Diagram](#entity-relationship-diagram)
- [Admin Panel Implementation](#admin-panel-implementation)
- [TipTap Integration](#tiptap-integration)
- [Database Structure](#database-structure)
- [Authentication Flow](#authentication-flow)
- [Performance Optimization](#performance-optimization)
- [Testing Strategy](#testing-strategy)
- [Deployment Considerations](#deployment-considerations)

## Introduction

This document provides comprehensive technical specifications for implementing the backend system to support BicaraPintar's frontend application. The backend will be built using Laravel, providing RESTful API endpoints, admin panel functionality, and database management to power the company's digital presence.

## Technical Specifications

### Laravel Framework Requirements

- **Laravel Version**: 10.x (latest stable release)
- **PHP Version**: 8.2 or higher
- **Database**: MySQL 8.0 or PostgreSQL 14+
- **Server Requirements**:
  - Nginx or Apache
  - PHP-FPM
  - Composer 2.x
  - Node.js 18+ (for asset compilation)
  - Redis (for caching and queue management)

### Core Packages

| Package | Purpose |
|---------|---------|
| Laravel Sanctum | API authentication with token support |
| Laravel Spatie Permission | Role and permission management |
| Laravel Media Library | File and media management |
| Laravel Scout | Full-text search capabilities |
| Laravel Horizon | Queue monitoring and management |
| Laravel Telescope | Debugging and monitoring in development |
| Laravel Fortify | Authentication scaffolding |
| Laravel Socialite | OAuth authentication (optional) |

## API Endpoints

Based on the frontend analysis, the following API endpoints are required:

### Authentication

```
POST   /api/auth/login                # User login
POST   /api/auth/register             # User registration
POST   /api/auth/logout               # User logout
GET    /api/auth/user                 # Get authenticated user
POST   /api/auth/password/email       # Send password reset link
POST   /api/auth/password/reset       # Reset password
```

### Products

```
GET    /api/products                  # List all products
GET    /api/products/{id}             # Get product by ID
GET    /api/products/slug/{slug}      # Get product by slug
GET    /api/products/featured         # Get featured products
POST   /api/products                  # Create product (admin)
PUT    /api/products/{id}             # Update product (admin)
DELETE /api/products/{id}             # Delete product (admin)
```

### Services

```
GET    /api/services                  # List all services
GET    /api/services/{id}             # Get service by ID
GET    /api/services/slug/{slug}      # Get service by slug
GET    /api/services/featured         # Get featured services
POST   /api/services                  # Create service (admin)
PUT    /api/services/{id}             # Update service (admin)
DELETE /api/services/{id}             # Delete service (admin)
```

### Solutions

```
GET    /api/solutions                 # List all solutions
GET    /api/solutions/{id}            # Get solution by ID
GET    /api/solutions/slug/{slug}     # Get solution by slug
GET    /api/solutions/industry/{industry} # Get solutions by industry
GET    /api/solutions/product/{productId} # Get solutions by product
GET    /api/solutions/service/{serviceId} # Get solutions by service
POST   /api/solutions                 # Create solution (admin)
PUT    /api/solutions/{id}            # Update solution (admin)
DELETE /api/solutions/{id}            # Delete solution (admin)
```

### Case Studies

```
GET    /api/case-studies              # List all case studies
GET    /api/case-studies/{id}         # Get case study by ID
GET    /api/case-studies/slug/{slug}  # Get case study by slug
POST   /api/case-studies              # Create case study (admin)
PUT    /api/case-studies/{id}         # Update case study (admin)
DELETE /api/case-studies/{id}         # Delete case study (admin)
```

### News & Media

```
GET    /api/news/articles             # List all news articles
GET    /api/news/articles/{slug}      # Get article by slug
GET    /api/news/articles/featured    # Get featured articles
GET    /api/news/media                # List all media items
POST   /api/news/articles             # Create article (admin)
PUT    /api/news/articles/{id}        # Update article (admin)
DELETE /api/news/articles/{id}        # Delete article (admin)
```

### Contact & Forms

```
POST   /api/contact                   # Submit contact form
POST   /api/careers/apply             # Submit job application
POST   /api/newsletter/subscribe      # Subscribe to newsletter
```

### Admin-specific Endpoints

```
GET    /api/admin/dashboard/stats     # Get dashboard statistics
GET    /api/admin/users               # List all users (admin)
POST   /api/admin/users               # Create user (admin)
PUT    /api/admin/users/{id}          # Update user (admin)
DELETE /api/admin/users/{id}          # Delete user (admin)
```

## Entity Relationship Diagram

Based on the frontend data structure, here's the proposed ERD for the backend database:

```
+----------------+       +----------------+       +----------------+
|    Products    |       |    Services    |       |   Solutions    |
+----------------+       +----------------+       +----------------+
| id             |       | id             |       | id             |
| title          |       | title          |       | title          |
| slug           |       | slug           |       | slug           |
| short_desc     |       | short_desc     |       | industry       |
| full_desc      |       | full_desc      |       | short_desc     |
| features       |       | benefits       |       | full_desc      |
| image          |       | image          |       | challenges     |
| featured       |       | created_at     |       | results        |
| created_at     |       | updated_at     |       | metrics        |
| updated_at     |       +-------+--------+       | image          |
+-------+--------+               |                | gallery        |
        |                        |                | created_at     |
        |                        |                | updated_at     |
        |                        |                +-------+--------+
        |                        |                        |
+-------v--------+       +-------v--------+       +-------v--------+
| Product_Service |       | Service_Solution |     | Product_Solution |
+----------------+       +----------------+       +----------------+
| product_id     |       | service_id     |       | product_id     |
| service_id     |       | solution_id    |       | solution_id    |
+----------------+       +----------------+       +----------------+

+----------------+       +----------------+       +----------------+
|  Case Studies  |       |  News Articles |       |  Media Items   |
+----------------+       +----------------+       +----------------+
| id             |       | id             |       | id             |
| title          |       | title          |       | title          |
| slug           |       | slug           |       | type           |
| client         |       | date           |       | date           |
| industry       |       | category       |       | source         |
| challenge      |       | summary        |       | summary        |
| solution       |       | content        |       | url            |
| results        |       | image          |       | created_at     |
| testimonial    |       | author_id      |       | updated_at     |
| images         |       | featured       |       +----------------+
| created_at     |       | created_at     |
| updated_at     |       | updated_at     |
+-------+--------+       +----------------+
        |
+-------v--------+       +----------------+       +----------------+
| CaseStudy_Rel  |       |     Users      |       |     Roles      |
+----------------+       +----------------+       +----------------+
| case_study_id  |       | id             |       | id             |
| product_id     |       | name           |       | name           |
| service_id     |       | email          |       | guard_name     |
+----------------+       | password       |       | created_at     |
                         | remember_token |       | updated_at     |
+----------------+       | role_id        |       +-------+--------+
|    Contacts    |       | created_at     |               |
+----------------+       | updated_at     |               |
| id             |       +-------+--------+               |
| first_name     |               |                        |
| last_name      |               +------------------------+
| email          |
| company        |       +----------------+       +----------------+
| subject        |       |  Permissions   |       | Role_Permission|
| message        |       +----------------+       +----------------+
| created_at     |       | id             |       | role_id        |
| updated_at     |       | name           |       | permission_id  |
+----------------+       | guard_name     |       +----------------+
                         | created_at     |
                         | updated_at     |
                         +----------------+
```

## Admin Panel Implementation

The admin panel will be implemented using Laravel's built-in authentication and authorization system, enhanced with Spatie Permission for role-based access control.

### Admin Roles and Permissions

1. **Super Admin**
   - Full access to all features
   - User management
   - Role and permission management

2. **Content Manager**
   - Manage products, services, solutions
   - Manage case studies and news articles
   - Upload and manage media

3. **Marketing Admin**
   - Manage news articles and media items
   - View contact submissions
   - Manage newsletter subscribers

### CRUD Operations

All content types (Products, Services, Solutions, Case Studies, News Articles) will have standard CRUD operations with the following features:

- List view with filtering, sorting, and pagination
- Detail view with related items
- Create/Edit forms with validation
- Soft delete with restore capability
- Bulk operations (delete, publish/unpublish)

### Permission Management

Implement granular permissions using Spatie Permission:

```php
// Example permissions
'view products'
'create products'
'edit products'
'delete products'
'view services'
'create services'
// etc.
```

### Admin Dashboard

The admin dashboard will include:

- Key metrics (visitors, page views, contact submissions)
- Recent content updates
- Latest contact form submissions
- Quick links to common tasks

## TipTap Integration

TipTap will be integrated as a rich text editor for content management, particularly for long-form content like product descriptions, service details, and news articles.

### Backend Implementation

1. **Controller Setup**:

```php
public function storeArticle(Request $request)
{
    $validated = $request->validate([
        'title' => 'required|string|max:255',
        'content' => 'required|string', // TipTap JSON content
        // other fields
    ]);
    
    $article = Article::create($validated);
    
    return response()->json($article, 201);
}
```

2. **Content Storage**:
   - Store TipTap content as JSON in a TEXT/LONGTEXT column
   - Implement HTML sanitization for security

3. **Image Handling**:
   - Create dedicated endpoint for image uploads
   - Integrate with Laravel Media Library for image management

```php
// Image upload endpoint for TipTap
public function uploadImage(Request $request)
{
    $request->validate([
        'image' => 'required|image|max:2048',
    ]);
    
    $path = $request->file('image')->store('tiptap-images', 'public');
    
    return response()->json([
        'url' => Storage::url($path)
    ]);
}
```

4. **Content Rendering**:
   - Create a helper to render TipTap JSON as HTML for non-SPA contexts

```php
// Helper function to render TipTap content as HTML
function renderTipTapContent($jsonContent)
{
    // Parse JSON and convert to HTML
    // Implementation depends on TipTap configuration
}
```

## Database Structure

### Migrations

Create the following migrations:

1. **Products Table**:

```php
Schema::create('products', function (Blueprint $table) {
    $table->id();
    $table->string('title');
    $table->string('slug')->unique();
    $table->text('short_description');
    $table->text('full_description');
    $table->json('features')->nullable();
    $table->string('image')->nullable();
    $table->boolean('featured')->default(false);
    $table->timestamps();
    $table->softDeletes();
});
```

2. **Services Table**:

```php
Schema::create('services', function (Blueprint $table) {
    $table->id();
    $table->string('title');
    $table->string('slug')->unique();
    $table->text('short_description');
    $table->text('full_description');
    $table->json('benefits')->nullable();
    $table->string('image')->nullable();
    $table->timestamps();
    $table->softDeletes();
});
```

3. **Solutions Table**:

```php
Schema::create('solutions', function (Blueprint $table) {
    $table->id();
    $table->string('title');
    $table->string('slug')->unique();
    $table->string('industry');
    $table->text('short_description');
    $table->text('full_description');
    $table->json('challenges')->nullable();
    $table->json('results')->nullable();
    $table->json('metrics')->nullable();
    $table->string('image')->nullable();
    $table->json('gallery')->nullable();
    $table->timestamps();
    $table->softDeletes();
});
```

4. **Case Studies Table**:

```php
Schema::create('case_studies', function (Blueprint $table) {
    $table->id();
    $table->string('title');
    $table->string('slug')->unique();
    $table->string('client');
    $table->string('industry');
    $table->text('challenge');
    $table->text('solution');
    $table->text('results');
    $table->json('testimonial')->nullable();
    $table->json('images')->nullable();
    $table->timestamps();
    $table->softDeletes();
});
```

5. **News Articles Table**:

```php
Schema::create('news_articles', function (Blueprint $table) {
    $table->id();
    $table->string('title');
    $table->string('slug')->unique();
    $table->date('date');
    $table->string('category');
    $table->text('summary');
    $table->longText('content');
    $table->string('image')->nullable();
    $table->foreignId('author_id')->nullable()->constrained('users');
    $table->boolean('featured')->default(false);
    $table->json('tags')->nullable();
    $table->timestamps();
    $table->softDeletes();
});
```

6. **Media Items Table**:

```php
Schema::create('media_items', function (Blueprint $table) {
    $table->id();
    $table->string('title');
    $table->string('type');
    $table->date('date');
    $table->string('source')->nullable();
    $table->text('summary');
    $table->string('url');
    $table->timestamps();
});
```

7. **Pivot Tables**:

```php
// Product-Service relationship
Schema::create('product_service', function (Blueprint $table) {
    $table->foreignId('product_id')->constrained()->onDelete('cascade');
    $table->foreignId('service_id')->constrained()->onDelete('cascade');
    $table->primary(['product_id', 'service_id']);
});

// Service-Solution relationship
Schema::create('service_solution', function (Blueprint $table) {
    $table->foreignId('service_id')->constrained()->onDelete('cascade');
    $table->foreignId('solution_id')->constrained()->onDelete('cascade');
    $table->primary(['service_id', 'solution_id']);
});

// Product-Solution relationship
Schema::create('product_solution', function (Blueprint $table) {
    $table->foreignId('product_id')->constrained()->onDelete('cascade');
    $table->foreignId('solution_id')->constrained()->onDelete('cascade');
    $table->primary(['product_id', 'solution_id']);
});

// Case Study relationships
Schema::create('case_study_product', function (Blueprint $table) {
    $table->foreignId('case_study_id')->constrained()->onDelete('cascade');
    $table->foreignId('product_id')->constrained()->onDelete('cascade');
    $table->primary(['case_study_id', 'product_id']);
});

Schema::create('case_study_service', function (Blueprint $table) {
    $table->foreignId('case_study_id')->constrained()->onDelete('cascade');
    $table->foreignId('service_id')->constrained()->onDelete('cascade');
    $table->primary(['case_study_id', 'service_id']);
});
```

### Seeders

Create seeders to populate the database with initial data:

1. **UserSeeder**: Create admin and test users
2. **RoleAndPermissionSeeder**: Set up roles and permissions
3. **ProductSeeder**: Seed products from frontend data
4. **ServiceSeeder**: Seed services from frontend data
5. **SolutionSeeder**: Seed solutions from frontend data
6. **CaseStudySeeder**: Seed case studies from frontend data
7. **NewsArticleSeeder**: Seed news articles from frontend data

### Factories

Create factories for testing and development:

```php
// Example Product Factory
class ProductFactory extends Factory
{
    protected $model = Product::class;

    public function definition()
    {
        return [
            'title' => $this->faker->company() . ' ' . $this->faker->word(),
            'slug' => $this->faker->slug(),
            'short_description' => $this->faker->sentence(15),
            'full_description' => $this->faker->paragraphs(3, true),
            'features' => json_encode($this->faker->sentences(5)),
            'image' => 'images/products/' . $this->faker->numberBetween(1, 5) . '.jpg',
            'featured' => $this->faker->boolean(20),
        ];
    }
}
```

## Authentication Flow

The authentication system will be implemented using Laravel Sanctum for API token authentication.

### Authentication Flow

1. **Registration**:
   - User submits registration form
   - Validate input and create user
   - Assign default role
   - Return user data with token

2. **Login**:
   - User submits credentials
   - Validate credentials
   - Generate Sanctum token
   - Return user data with token

3. **Authenticated Requests**:
   - Frontend includes token in Authorization header
   - Backend validates token
   - Process request if authenticated

4. **Logout**:
   - Revoke current token
   - Return success response

### Middleware Requirements

1. **Authentication Middleware**:
   - `auth:sanctum` for API routes requiring authentication

2. **Role-based Middleware**:
   - Using Spatie Permission's middleware
   - `role:admin` for admin-only routes
   - `permission:edit products` for specific permissions

3. **API Rate Limiting**:
   - Implement rate limiting for public endpoints
   - Higher limits for authenticated users

```php
// Example route definitions with middleware
Route::middleware('auth:sanctum')->group(function () {
    Route::get('/user', [UserController::class, 'show']);
    
    // Admin routes
    Route::middleware('role:admin')->prefix('admin')->group(function () {
        Route::apiResource('products', AdminProductController::class);
        Route::apiResource('services', AdminServiceController::class);
    });
    
    // Content manager routes
    Route::middleware('permission:edit content')->prefix('content')->group(function () {
        Route::post('upload-image', [ContentController::class, 'uploadImage']);
    });
});
```

## Performance Optimization

### Caching Strategy

1. **Response Caching**:
   - Cache API responses for public endpoints
   - Use Redis for cache storage
   - Implement cache tags for efficient invalidation

```php
// Example caching in a controller
public function index()
{
    return Cache::tags(['products'])->remember('products.all', 3600, function () {
        return Product::with('services')->get();
    });
}
```

2. **Model Caching**:
   - Cache frequently accessed models
   - Implement cache invalidation on model updates

3. **Query Caching**:
   - Cache complex queries
   - Use query builder's remember method

### Queue Implementation

1. **Queued Jobs**:
   - Process email sending in the background
   - Handle image processing and optimization
   - Generate reports and exports

```php
// Example queued job
public function store(Request $request)
{
    $contact = Contact::create($request->validated());
    
    // Queue email notification
    NotifyAdminJob::dispatch($contact);
    
    return response()->json($contact, 201);
}
```

2. **Queue Configuration**:
   - Use Redis as queue driver
   - Configure multiple queues for different priorities
   - Set up Horizon for monitoring

### Database Optimization

1. **Indexing**:
   - Add indexes to frequently queried columns
   - Composite indexes for common query patterns

2. **Eager Loading**:
   - Use eager loading to prevent N+1 query problems
   - Implement lazy loading where appropriate

```php
// Example of eager loading
public function show($slug)
{
    return Product::with(['services', 'solutions', 'caseStudies'])
        ->where('slug', $slug)
        ->firstOrFail();
}
```

3. **Query Optimization**:
   - Use chunking for large datasets
   - Implement pagination for list endpoints

## Testing Strategy

### Unit Testing

1. **Model Testing**:
   - Test relationships
   - Test scopes and custom methods
   - Test validation rules

2. **Service Testing**:
   - Test business logic in service classes
   - Test edge cases and error handling

### Feature Testing

1. **API Endpoint Testing**:
   - Test all API endpoints
   - Test authentication and authorization
   - Test validation and error responses

```php
// Example API test
public function test_can_get_products()
{
    $user = User::factory()->create();
    
    $response = $this->actingAs($user)
        ->getJson('/api/products');
    
    $response->assertStatus(200)
        ->assertJsonStructure([
            'data' => [
                '*' => ['id', 'title', 'slug', 'short_description']
            ]
        ]);
}
```

2. **Admin Panel Testing**:
   - Test CRUD operations
   - Test permission enforcement
   - Test form validation

### Integration Testing

1. **Frontend-Backend Integration**:
   - Test API responses match frontend expectations
   - Test data format consistency
   - Test error handling and recovery

2. **Third-party Service Integration**:
   - Test email sending
   - Test file storage
   - Test payment processing (if applicable)

### Continuous Integration

1. **CI Pipeline**:
   - Run tests on every pull request
   - Check code style and static analysis
   - Generate test coverage reports

2. **Deployment Testing**:
   - Test database migrations
   - Test seeding process
   - Test environment configuration

## Deployment Considerations

### Environment Setup

1. **Production Environment**:
   - PHP 8.2+ with OPcache enabled
   - Nginx with proper caching configuration
   - MySQL 8.0 or PostgreSQL 14+
   - Redis for caching and queues
   - Supervisor for queue workers

2. **Staging Environment**:
   - Mirror of production for testing
   - Separate database with anonymized data
   - Debugging tools enabled

3. **Development Environment**:
   - Local setup with Docker
   - Laravel Sail for easy setup
   - Telescope enabled for debugging

### Deployment Process

1. **Continuous Deployment**:
   - Automated deployment from main branch
   - Zero-downtime deployment
   - Rollback capability

2. **Database Migrations**:
   - Run migrations with --force flag
   - Backup database before migration
   - Test migrations in staging first

3. **Asset Compilation**:
   - Compile assets during deployment
   - Version assets for cache busting
   - Use CDN for asset delivery

### Monitoring and Maintenance

1. **Application Monitoring**:
   - Set up Laravel Telescope in production
   - Implement error logging with Sentry or Bugsnag
   - Monitor queue performance with Horizon

2. **Server Monitoring**:
   - CPU, memory, and disk usage
   - Database performance
   - Cache hit rates

3. **Backup Strategy**:
   - Daily database backups
   - File storage backups
   - Off-site backup storage

4. **Security Updates**:
   - Regular dependency updates
   - Security patch application
   - Vulnerability scanning