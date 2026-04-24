# What's Missing / Future Enhancements

This document tracks features and integrations that aren't included in the base template but are commonly needed for production applications.

## 🚨 Production-Critical Items

These should be addressed before deploying to production:

### 1. Email Service Integration
**Status:** ⚠️ Not Configured  
**Current State:** Email verification and password reset URLs are logged to console  
**Need:** Real email delivery

**Options:**
- [Resend](https://resend.com/) - Modern, developer-friendly (recommended)
- [SendGrid](https://sendgrid.com/) - Established, feature-rich
- [AWS SES](https://aws.amazon.com/ses/) - Cost-effective for high volume
- [Postmark](https://postmarkapp.com/) - Reliable transactional email

**Implementation Location:** `src/lib/auth.ts`
```typescript
// Update sendVerificationEmail and sendResetPassword functions
```

### 2. Error Monitoring
**Status:** ❌ Not Included  
**Need:** Track errors and exceptions in production

**Options:**
- [Sentry](https://sentry.io/) - Industry standard, excellent DX
- [LogRocket](https://logrocket.com/) - Includes session replay
- [Rollbar](https://rollbar.com/) - Good for smaller teams
- [BugSnag](https://www.bugsnag.com/) - Simple setup

**Quick Setup (Sentry):**
```bash
npm install @sentry/nextjs
npx @sentry/wizard@latest -i nextjs
```

### 3. Rate Limiting
**Status:** ❌ Not Included  
**Need:** Protect API routes from abuse

**Options:**
- [Upstash Rate Limit](https://upstash.com/docs/redis/sdks/ratelimit-ts/overview) - Edge-compatible
- [next-rate-limit](https://github.com/vercel/next.js/tree/canary/examples/api-routes-rate-limit)
- Vercel Edge Middleware rate limiting

**Example Implementation:**
```typescript
// middleware.ts
import { Ratelimit } from "@upstash/ratelimit";
import { Redis } from "@upstash/redis";
```

### 4. Security Headers
**Status:** ⚠️ Basic Only  
**Need:** Enhanced security headers (CSP, HSTS, etc.)

**Implementation:** `next.config.js`
```javascript
async headers() {
  return [
    {
      source: '/:path*',
      headers: [
        { key: 'X-Frame-Options', value: 'DENY' },
        { key: 'X-Content-Type-Options', value: 'nosniff' },
        { key: 'Referrer-Policy', value: 'strict-origin-when-cross-origin' },
        // Add CSP, HSTS, etc.
      ],
    },
  ];
}
```

---

## 📊 Observability & Analytics

### Analytics
**Status:** ❌ Not Included  
**Options:**
- [Vercel Analytics](https://vercel.com/analytics) - Built-in, zero config
- [Posthog](https://posthog.com/) - Product analytics + feature flags
- [Plausible](https://plausible.io/) - Privacy-focused
- [Google Analytics 4](https://analytics.google.com/) - Free, comprehensive

### Application Performance Monitoring (APM)
**Status:** ❌ Not Included  
**Options:**
- [Vercel Speed Insights](https://vercel.com/docs/speed-insights)
- [New Relic](https://newrelic.com/)
- [Datadog](https://www.datadoghq.com/)

### Logging
**Status:** ⚠️ Console Only  
**Options:**
- [Axiom](https://axiom.co/) - Modern, serverless-friendly
- [Better Stack](https://betterstack.com/) (formerly Logtail)
- [Datadog Logs](https://www.datadoghq.com/product/log-management/)

---

## 🧪 Testing & Quality

### Unit & Integration Testing
**Status:** ❌ Not Included  
**Recommended Setup:**
```bash
# Vitest (fast, modern)
npm install -D vitest @testing-library/react @testing-library/jest-dom

# Or Jest (more established)
npm install -D jest @testing-library/react @testing-library/jest-dom
```

**What to Test:**
- API route handlers
- Database queries
- Auth flows
- Utility functions

### End-to-End Testing
**Status:** ❌ Not Included  
**Recommended:**
```bash
# Playwright (recommended)
npm install -D @playwright/test
npx playwright install

# Or Cypress
npm install -D cypress
```

**What to Test:**
- Sign up / sign in flows
- Protected route access
- Critical user journeys
- AI chat functionality

### Type Safety Enhancements
**Status:** ⚠️ Basic  
**Potential Improvements:**
- [Zod](https://zod.dev/) for runtime validation (already included)
- [tRPC](https://trpc.io/) for end-to-end type safety
- Strict database type generation

---

## 💳 Payments & Billing

### Payment Processing
**Status:** ❌ Not Included (Polar scaffolded but not implemented)  
**Options:**
- [Stripe](https://stripe.com/) - Most comprehensive
- [Polar](https://polar.sh/) - Developer-focused (env vars present)
- [LemonSqueezy](https://www.lemonsqueezy.com/) - Merchant of record
- [Paddle](https://www.paddle.com/) - Merchant of record

**Features Needed:**
- Subscription management
- Webhook handlers
- Payment status tracking
- Invoice generation
- Tax handling

### Usage-Based Billing
**For AI features, file storage, etc.**
- Track API calls
- Monitor token usage
- Implement usage tiers
- Billing alerts

---

## 📁 File Management

### File Uploads
**Status:** ⚠️ Scaffolded (not fully implemented)  
**Current:** Basic Vercel Blob configuration
**Need:** Complete upload flow

**Features to Add:**
- File size validation
- File type restrictions
- Image optimization (Sharp, next/image)
- Progress indicators
- Multi-file upload
- Direct upload to storage (presigned URLs)

### Media Processing
**Status:** ❌ Not Included  
**Options:**
- Image optimization (Sharp, ImageMagick)
- Video transcoding (FFmpeg)
- PDF generation (Puppeteer, React-PDF)
- Document preview

---

## 🔐 Advanced Authentication

### Additional Auth Providers
**Status:** ⚠️ Only Google OAuth  
**Easy to Add:**
- GitHub OAuth
- Discord OAuth
- Apple Sign In
- Microsoft/Azure AD
- Facebook Login

### Advanced Auth Features
**Status:** ❌ Not Included  
**Consider Adding:**
- Two-factor authentication (2FA/TOTP)
- SMS authentication
- Passkeys/WebAuthn
- Magic link login
- Session management UI (active sessions, logout all)
- Account linking (multiple OAuth providers)

### Authorization
**Status:** ⚠️ Basic (authenticated vs. unauthenticated)  
**May Need:**
- Role-Based Access Control (RBAC)
- Permission system
- Team/organization support
- Resource-based permissions
- Admin panel

---

## 🌐 Internationalization (i18n)

**Status:** ❌ Not Included  
**If Needed:**
```bash
# next-intl (recommended for Next.js)
npm install next-intl

# Or next-i18next
npm install next-i18next react-i18next i18next
```

**Features:**
- Multi-language support
- Locale detection
- Translation management
- Date/number formatting

---

## 🔍 Search & Discovery

### Full-Text Search
**Status:** ❌ Not Included  
**Options:**
- [Algolia](https://www.algolia.com/) - Hosted, fast
- [Typesense](https://typesense.org/) - Open source, self-hosted option
- [Meilisearch](https://www.meilisearch.com/) - Open source, easy to use
- PostgreSQL full-text search (built-in)

### Vector Search (for AI features)
**Status:** ❌ Not Included  
**Options:**
- [Pinecone](https://www.pinecone.io/)
- [Qdrant](https://qdrant.tech/)
- [Weaviate](https://weaviate.io/)
- [pgvector](https://github.com/pgvector/pgvector) (PostgreSQL extension)

---

## 📱 Mobile & Real-Time

### Mobile App
**Status:** ❌ Not Included  
**Options:**
- React Native + Expo
- Capacitor (web → mobile)
- Progressive Web App (PWA)

### Real-Time Features
**Status:** ❌ Not Included  
**If Needed:**
- [Pusher](https://pusher.com/) - Hosted WebSockets
- [Ably](https://ably.com/) - Real-time messaging
- [Supabase Realtime](https://supabase.com/realtime) (if switching to Supabase)
- Socket.io (self-hosted)

**Use Cases:**
- Live notifications
- Collaborative editing
- Chat features
- Real-time dashboards

---

## 🤖 Advanced AI Features

### Enhanced AI Capabilities
**Status:** ⚠️ Basic Chat Only  
**Potential Additions:**
- Streaming with function calling
- Multi-modal inputs (images, audio)
- Embeddings generation
- RAG (Retrieval Augmented Generation)
- Prompt caching
- Fine-tuned models

### AI Agent Tools
**Options:**
- [LangChain](https://www.langchain.com/)
- [LlamaIndex](https://www.llamaindex.ai/)
- [AutoGPT](https://github.com/Significant-Gravitas/AutoGPT)

---

## 🚀 DevOps & Infrastructure

### CI/CD Pipeline
**Status:** ❌ Not Included  
**Platform:** Vercel has built-in CI/CD, but you might want:
- GitHub Actions workflow
- Automated testing on PR
- Staging environment
- Preview deployments
- Database migrations on deploy

### Database Management
**Status:** ⚠️ Manual Migrations  
**Improvements:**
- Automated backup strategy
- Point-in-time recovery
- Read replicas (for scale)
- Connection pooling (PgBouncer)
- Database monitoring

### Docker Setup
**Status:** ❌ Not Included  
**If Needed:**
```dockerfile
# Dockerfile for containerized deployment
# docker-compose.yml for local dev environment
```

---

## 📋 Admin & Operations

### Admin Dashboard
**Status:** ❌ Not Included  
**Features:**
- User management
- Content moderation
- Analytics overview
- System health monitoring
- Feature flags

### Content Management
**Status:** ❌ Not Included  
**Options:**
- [Sanity](https://www.sanity.io/)
- [Contentful](https://www.contentful.com/)
- [Strapi](https://strapi.io/)
- Custom admin UI

---

## 🎨 Design & UX

### Design System
**Status:** ⚠️ Basic shadcn/ui  
**Enhancements:**
- Custom design tokens
- Component documentation (Storybook)
- Accessibility audit
- Design system documentation

### Enhanced UX Features
**Status:** ❌ Not Included  
**Consider:**
- Skeleton loaders (included but limited)
- Optimistic UI updates
- Infinite scroll
- Virtual scrolling (large lists)
- Drag and drop
- Toast notifications (Sonner included)
- Command palette (⌘K menu)

---

## 📊 Data & Reporting

### Data Export
**Status:** ❌ Not Included  
**Features:**
- CSV export
- PDF reports
- Data backup tools
- API for data access

### Business Intelligence
**Status:** ❌ Not Included  
**Options:**
- [Metabase](https://www.metabase.com/)
- [Retool](https://retool.com/)
- [Grafana](https://grafana.com/)
- Custom dashboards

---

## 🔗 Integrations & Webhooks

### Webhook System
**Status:** ❌ Not Included  
**For:**
- Third-party service integration
- Event notifications
- Automation triggers

### Common Integrations
**Not Included:**
- Slack notifications
- Email marketing (Mailchimp, ConvertKit)
- CRM (Salesforce, HubSpot)
- Calendar (Google Calendar, Cal.com)
- SMS (Twilio)

---

## 📝 Documentation

### API Documentation
**Status:** ❌ Not Included  
**Tools:**
- OpenAPI/Swagger
- Postman collections
- API reference site

### User Documentation
**Status:** ❌ Not Included  
**Tools:**
- [Mintlify](https://mintlify.com/)
- [GitBook](https://www.gitbook.com/)
- [Docusaurus](https://docusaurus.io/)

---

## Priority Matrix

### Must-Have Before Production
1. ✅ Email service integration
2. ✅ Error monitoring
3. ✅ Rate limiting
4. ✅ Security headers
5. ✅ Environment-specific secrets

### Should Have Soon
1. Analytics
2. Logging
3. Basic testing
4. Performance monitoring
5. Database backups

### Nice to Have
1. Advanced auth features
2. Real-time capabilities
3. Advanced AI features
4. Mobile app
5. Admin dashboard

### Depends on Your Use Case
- Payments (if selling something)
- Search (if lots of content)
- i18n (if multi-language)
- File uploads (if user-generated content)
- CMS (if marketing site)

---

## How to Use This Document

1. **Review** the list based on your project requirements
2. **Prioritize** what you actually need
3. **Document** decisions (add notes below)
4. **Ask Claude** to help implement any of these features

**Your Notes:**
```
Feature: [Name]
Priority: [High/Medium/Low]
Reason: [Why you need it]
Timeline: [When to implement]
---
```

---

**Last Updated:** [Date]  
**Review Frequency:** Monthly or before major milestones
