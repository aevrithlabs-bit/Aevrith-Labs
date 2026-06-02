# Aevrith Labs - System Architecture

## Project Overview
Aevrith Labs is a student networking and mentorship platform connecting students with seniors in their dream colleges.

## System Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                        CLIENT LAYER                              │
├─────────────────────────────────────────────────────────────────┤
│  Next.js Frontend (React + TypeScript)                           │
│  - SSR/SSG for SEO                                               │
│  - Mobile-First Responsive Design                                │
│  - Real-time Socket.io Client                                    │
└──────────────────────────┬──────────────────────────────────────┘
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
┌───────▼────────┐ ┌──────▼──────┐ ┌────────▼──────┐
│  REST API      │ │  WebSocket  │ │  Static CDN   │
│  Gateway       │ │  (Socket.io)│ │  (Cloudinary/ │
│                │ │             │ │   AWS S3)     │
└───────┬────────┘ └──────┬──────┘ └────────┬──────┘
        │                 │                 │
        └─────────────────┼─────────────────┘
                          │
        ┌─────────────────▼─────────────────┐
        │      API GATEWAY / MIDDLEWARE     │
        ├─────────────────────────────────┤
        │ - Authentication (JWT + Firebase)│
        │ - Rate Limiting                  │
        │ - Request Validation             │
        │ - CORS                           │
        └─────────────────┬─────────────────┘
                          │
        ┌─────────────────▼─────────────────┐
        │    APPLICATION LAYER              │
        ├─────────────────────────────────┤
        │ Node.js + Express Backend        │
        │ - Authentication Service         │
        │ - User Management                │
        │ - Mentorship Service             │
        │ - Chat Service                   │
        │ - Feed Service                   │
        │ - Moderation Service             │
        │ - Admin Service                  │
        │ - Notification Service           │
        └─────────────────┬─────────────────┘
                          │
        ┌─────────────────▼─────────────────┐
        │     DATA PERSISTENCE LAYER        │
        ├─────────────────────────────────┤
        │ PostgreSQL (Primary DB)          │
        │ Redis (Cache & Sessions)         │
        │ Message Queue (Bull/Redis)       │
        └─────────────────┬─────────────────┘
                          │
        ┌─────────────────▼─────────────────┐
        │    EXTERNAL SERVICES              │
        ├─────────────────────────────────┤
        │ - Firebase Auth                  │
        │ - Cloudinary (Media)             │
        │ - AWS S3 (Backups)               │
        │ - SendGrid (Emails)              │
        │ - Twilio (OTP/SMS)               │
        └─────────────────────────────────┘
```

## Technology Stack

### Frontend
- **Framework**: Next.js 14+ (React 18 + TypeScript)
- **Styling**: Tailwind CSS + Shadcn UI Components
- **State Management**: Zustand / React Query
- **Real-time**: Socket.io Client
- **Forms**: React Hook Form + Zod
- **API Client**: Axios / TanStack Query
- **Authentication**: NextAuth.js + Firebase

### Backend
- **Runtime**: Node.js 18+
- **Framework**: Express.js
- **Language**: TypeScript
- **Database**: PostgreSQL 14+
- **Cache**: Redis
- **Message Queue**: Bull (Redis-backed)
- **Authentication**: JWT + Firebase Admin SDK
- **Validation**: Joi / Zod
- **Logging**: Winston
- **Error Tracking**: Sentry

### Database
- **Primary**: PostgreSQL
- **Cache**: Redis
- **Sessions**: Redis

### DevOps & Deployment
- **Containerization**: Docker & Docker Compose
- **CI/CD**: GitHub Actions
- **Deployment**: AWS EC2 / Railway / Vercel
- **Monitoring**: PM2 / New Relic
- **Reverse Proxy**: Nginx

## MVP Priority Services

### Phase 1 (Core)
1. **Authentication Service** - User signup/login
2. **User Management** - Profiles & verification
3. **Discovery Service** - Find students
4. **Real-time Chat** - WebSocket messaging
5. **Admin Dashboard** - Verification & moderation

### Phase 2 (Enhancement)
6. Mentorship System
7. Community Feed
8. Notifications
9. Report & Block System
10. Advanced Search

### Phase 3 (Scaling)
11. AI Features
12. Advanced Analytics
13. Mobile App
14. Marketplace Integration

## Security Architecture

```
┌──────────────────────────────────────────┐
│        SECURITY LAYERS                    │
├──────────────────────────────────────────┤
│ 1. TLS/SSL (HTTPS only)                   │
│ 2. API Rate Limiting                      │
│ 3. CORS Policy                            │
│ 4. JWT Token Validation                   │
│ 5. Firebase Authentication                │
│ 6. Input Validation & Sanitization        │
│ 7. SQL Injection Prevention (Parameterized)|
│ 8. XSS Protection                         │
│ 9. CSRF Protection                        │
│ 10. Role-Based Access Control (RBAC)     │
│ 11. Encrypted Document Storage            │
│ 12. Audit Logging                         │
└──────────────────────────────────────────┘
```

## Scalability Strategy

### Horizontal Scaling
- Stateless API servers (load balanced with Nginx)
- Separate read replicas for PostgreSQL
- Redis cluster for caching

### Vertical Scaling
- Database indexing strategy
- Query optimization
- Connection pooling

### Performance Optimization
- CDN for static assets
- Image optimization (WebP, responsive sizes)
- API response caching
- Database query caching
- Socket.io rooms for chat scaling

## Data Flow Diagrams

### Authentication Flow
```
User → Sign Up Form → Firebase Auth → JWT Generation → 
Create User in DB → Return Token → Store in Client
```

### Real-time Chat Flow
```
User A → Socket.io Client → Express Server → 
Redis Pub/Sub → Socket.io Server → User B Socket Client
```

### Mentorship Request Flow
```
Student → Send Request → Store in DB → 
Notify Mentor (Real-time) → Mentor Accept/Reject → 
Update Status → Notify Student
```

## File Structure Overview
```
aevrith-labs/
├── frontend/          # Next.js application
├── backend/           # Node.js/Express application
├── database/          # PostgreSQL migrations & seeds
├── docker-compose.yml # Local development setup
├── docs/              # Documentation
└── deployment/        # Deployment configurations
```

## API Rate Limits
- Public endpoints: 100 req/min
- Authenticated endpoints: 1000 req/min
- Admin endpoints: 5000 req/min

## Session Management
- JWT expiry: 7 days
- Refresh token expiry: 30 days
- Session stored in Redis
- Automatic cleanup after expiry

## Error Handling
- Standardized error response format
- Error logging & tracking
- Graceful degradation
- User-friendly error messages

## Monitoring & Logging
- Application logs (Winston)
- Error tracking (Sentry)
- Performance monitoring
- API usage analytics
- User activity audit logs

---

This architecture supports:
- **Initial Users**: 1,000 concurrent users
- **Scalable to**: 100,000+ concurrent users
- **Data retention**: 7-year compliance
- **Uptime SLA**: 99.5%
