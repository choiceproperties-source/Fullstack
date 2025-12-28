# Choice Properties - Real Estate Rental Platform

## Overview

Choice Properties is a full-stack real estate rental platform that connects landlords with renters. The application enables property listing, browsing, rental applications, lease management, and payment processing. It follows a domain-based modular architecture on the backend with a React frontend.

**Core Functionality:**
- Property listing and search for renters
- Application submission and processing workflow
- Lease creation, signing, and management
- Payment processing with audit trails
- Role-based dashboards for renters, landlords, property managers, and admins

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture
- **Framework:** React 18 with Vite for build tooling
- **Routing:** Wouter (lightweight router)
- **State Management:** React Query for server state, Context API for auth state
- **UI Components:** Shadcn UI with Radix primitives, styled with TailwindCSS
- **Forms:** React Hook Form with Zod validation

### Backend Architecture
- **Framework:** Express.js on Node.js
- **Pattern:** Domain-based modular structure with thin routes, service layer, and repository layer
- **Modules:** Located in `server/modules/` organized by domain (properties, applications, payments, leases, admin, auth)
- **Legacy Routes:** `server/routes.ts` contains older monolithic routes being migrated to modular structure
- **API Versioning:** New endpoints use `/api/v2/*` prefix, legacy routes remain at `/api/*`

### Data Layer
- **ORM:** Drizzle ORM with PostgreSQL dialect
- **Schema:** Defined in `shared/schema.ts` with Zod validation schemas
- **Database:** PostgreSQL (via Supabase or direct connection using DATABASE_URL)

### Authentication & Authorization
- **Provider:** Supabase Auth handles user authentication
- **Tokens:** JWT-based session management
- **Roles:** renter, landlord, property_manager, agent, admin
- **Middleware:** `server/auth-middleware.ts` handles token validation and role checks
- **Row Level Security:** Enabled on database tables via Supabase

### Key Design Decisions

**Modular Backend Migration:**
The backend is transitioning from a monolithic `routes.ts` file to domain-based modules. Each module contains routes (thin handlers), services (business logic), and repositories (database queries). Migration order is strictly defined in `MIGRATION.md`.

**Image Handling:**
ImageKit is used for image optimization and CDN delivery. Images are uploaded to ImageKit, and URLs are stored in the database. The system includes upload limits and audit logging.

**Payment Audit Trail:**
All payment actions are logged and non-deletable for financial accountability. The payments module includes verification workflows and receipt generation.

## External Dependencies

### Required Services
- **Supabase:** PostgreSQL database, authentication, and storage
  - `SUPABASE_URL` - Project URL
  - `SUPABASE_SERVICE_ROLE_KEY` - Backend service key
  - `VITE_SUPABASE_URL` - Frontend project URL
  - `VITE_SUPABASE_ANON_KEY` - Frontend public key

- **PostgreSQL:** Database connection
  - `DATABASE_URL` - Direct database connection string for Drizzle

### Optional Services
- **ImageKit:** Image optimization and CDN
  - `IMAGEKIT_PUBLIC_KEY`
  - `IMAGEKIT_PRIVATE_KEY`
  - `IMAGEKIT_URL_ENDPOINT`

- **SendGrid:** Email notifications (via Replit connector)
  - Configured through Replit's connector system

### NPM Dependencies
- **Frontend:** React, React Query, Wouter, Radix UI, TailwindCSS, React Hook Form, Zod
- **Backend:** Express, Drizzle ORM, Supabase JS client, ImageKit SDK, SendGrid
- **Shared:** Zod schemas used by both frontend and backend