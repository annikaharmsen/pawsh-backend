# Pawsh E-Commerce API

A RESTful API for an e-commerce platform built with Laravel 12, featuring shopping cart management, Stripe payment integration, order processing, and an admin dashboard with analytics.

## Overview

Pawsh E-Commerce API provides a complete backend solution for online retail operations. The API handles user authentication, product catalog management, shopping cart functionality, secure payment processing through Stripe, and comprehensive order tracking. An admin dashboard offers insights into customer behavior, sales metrics, and inventory status.

## Features

- **User Authentication**: Secure registration and login using Laravel Sanctum token-based authentication
- **Product Management**: Browse product catalog with detailed product information and images
- **Shopping Cart**: Add, update, and remove items from cart with real-time quantity management
- **Address Management**: Store and manage multiple shipping addresses per user
- **Checkout Process**: Streamlined checkout flow with Stripe payment integration
- **Order Tracking**: Complete order history with status tracking and payment confirmation
- **Admin Dashboard**: Analytics and reports for customers, orders, products, and revenue
- **Stripe Webhooks**: Automatic order status updates based on payment events

## Tech Stack

- **Backend**: PHP 8.2, Laravel 12.0
- **Authentication**: Laravel Sanctum 4.0
- **Payment Processing**: Stripe PHP SDK 17.4
- **Database**: SQLite
- **Development Tools**: Laravel Pint, Laravel Sail, PHPUnit

## Installation

### Prerequisites

- PHP 8.2 or higher
- Composer
- Node.js and npm
- SQLite

### Setup

1. Clone the repository:
```bash
git clone https://github.com/annikaharmsen/pawsh-backend
cd pawsh-backend
```

2. Install PHP dependencies:
```bash
composer install
```

3. Install Node dependencies:
```bash
npm install
```

4. Create environment file:
```bash
cp .env.example .env
```

5. Generate application key:
```bash
php artisan key:generate
```

6. Configure your environment variables in `.env`:
```env
APP_NAME="Pawsh E-Commerce API"
APP_URL=http://localhost:8000

DB_CONNECTION=sqlite

STRIPE_API_KEY=your_stripe_secret_key
```

7. Create SQLite database:
```bash
touch database/database.sqlite
```

8. Run migrations:
```bash
php artisan migrate
```

9. (Optional) Seed the database with sample data:
```bash
php artisan db:seed
```

## Usage

### Starting the Development Server

Run all services concurrently (server, queue, logs, and Vite):
```bash
composer dev
```

Or run services individually:
```bash
php artisan serve
php artisan queue:listen
npm run dev
```

The API will be available at `http://localhost:8000`.

### API Endpoints

#### Authentication
```http
POST /api/register        # Register new user
POST /api/login           # Login user
DELETE /api/delete-account/{user}  # Delete user account
```

#### Products
```http
GET /api/products         # List all products
GET /api/products/{id}    # Get single product
```

#### Shopping Cart
```http
GET /api/cart             # Get cart items
POST /api/cart            # Add item to cart
PUT /api/cart/{id}        # Update cart item quantity
DELETE /api/cart/{id}     # Remove item from cart
```

#### Orders
```http
GET /api/orders           # Get user's orders
POST /api/orders          # Create new order
GET /api/orders/{id}      # Get order details
```

#### Addresses
```http
GET /api/addresses        # List user's addresses
POST /api/addresses       # Create new address
PUT /api/addresses/{id}   # Update address
DELETE /api/addresses/{id} # Delete address
```

#### Checkout
```http
POST /api/checkout/shipping/{order}  # Set shipping address
POST /api/checkout/session/{order}   # Create Stripe session
GET /api/checkout/status/{session}   # Check payment status
POST /api/checkout/stripe-webhook    # Handle Stripe webhooks
```

#### Admin Dashboard
```http
GET /api/admin/dashboard/overview          # Dashboard overview
GET /api/admin/dashboard/customers-report  # Customer analytics
GET /api/admin/dashboard/orders-report     # Order analytics
GET /api/admin/dashboard/products-report   # Product analytics
```

### Authentication

Most endpoints require authentication using Laravel Sanctum. Include the bearer token in the Authorization header:

```http
Authorization: Bearer {token}
```

Obtain a token by logging in via `/api/login`.

### Stripe Webhook Configuration

To receive real-time payment updates, configure your Stripe webhook:

1. In your Stripe dashboard, add a webhook endpoint: `https://yourdomain.com/api/checkout/stripe-webhook`
2. Subscribe to these events:
   - `checkout.session.completed`
   - `checkout.session.async_payment_succeeded`
   - `checkout.session.async_payment_failed`

## Database Schema

The application uses the following core models:

- **User**: Customer accounts with authentication
- **Product**: Product catalog items
- **CartItem**: Shopping cart entries
- **Address**: Customer shipping/billing addresses
- **Order**: Order records with status tracking
- **OrderItem**: Line items for each order
- **Payment**: Payment transaction records
- **PaymentMethod**: Stored payment methods

## Testing

Run the test suite:
```bash
composer test
```

Or use PHPUnit directly:
```bash
php artisan test
```

## Code Style

This project uses Laravel Pint for code formatting:
```bash
./vendor/bin/pint
```

## License

This project is licensed under the MIT License.
