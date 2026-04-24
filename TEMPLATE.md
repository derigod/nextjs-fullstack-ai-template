# Template Onboarding Instructions for Claude

**Context:** This is a Next.js full-stack template repository. When a developer uses this template for a new project, they will:
1. Clone/copy this repository
2. Set up their environment variables
3. Open the project in Claude Code for the first time

**Your Role:** When you detect this is a fresh template initialization (first conversation in a new project), you should guide the developer through understanding what they have and planning what to build next.

---

## Template Stack Overview

### What's Pre-Configured

**Backend:**
- Next.js 16 with App Router and React Server Components
- PostgreSQL database via Drizzle ORM
- Better Auth (email/password + Google OAuth)
- TypeScript with full type safety

**AI Integration:**
- Vercel AI SDK configured
- Google AI (Gemini) provider ready
- OpenRouter as alternative provider
- Streaming chat API endpoint

**Frontend:**
- shadcn/ui component library (New York style)
- Tailwind CSS v4
- Dark mode support
- Responsive design patterns

**Developer Tools:**
- ESLint + Prettier configured
- TypeScript strict mode
- Database migration scripts
- Path aliases set up

### Example Features Included

These are demonstration features that users will likely want to modify or remove:

1. **Todo System** - A simple CRUD example showing:
   - Database schema definition
   - API route patterns
   - Component structure
   
2. **Chat Interface** - AI chat example at `/chat` showing:
   - Vercel AI SDK integration
   - Streaming responses
   - Authentication checks
   - UI patterns for chat

3. **Dashboard** - Basic user dashboard at `/dashboard` showing:
   - Protected routes
   - User session data
   - Layout patterns

---

## Template Detection

This is a **new template project** if:
- No custom features beyond the included examples (todo, chat, dashboard)
- Environment variables are still using example values
- No git history beyond "Initial commit from template" or similar
- User mentions "template" or "starting a new project"

---

## Onboarding Flow

When you detect this is a new template initialization, follow this flow:

### Step 1: Welcome and Verify Setup

Start with:
```
Welcome! I can see you're starting a new project with this template. Let me help you get going.

First, let me verify your setup is complete:
- Have you updated all the required environment variables in .env?
- Have you run the database migrations (npm run db:migrate)?
- Is the app running successfully on localhost:3000?
```

If setup is incomplete, guide them to complete it before proceeding.

### Step 2: Understand Their Project

Ask clarifying questions to understand what they want to build:

```
Great! Now let's talk about what you want to build.

To help me suggest the best approach, can you tell me:

1. **What type of application are you building?**
   (e.g., SaaS product, internal tool, marketplace, social app, etc.)

2. **Who are the main users?**
   (e.g., end consumers, businesses, internal team, etc.)

3. **What are the core features you need?**
   (e.g., user profiles, payments, real-time updates, file uploads, etc.)

4. **Will you need the AI chat functionality?**
   - Keep it as-is
   - Modify it for a specific use case
   - Remove it entirely

5. **Any specific integrations or third-party services?**
   (e.g., Stripe, Twilio, email service, analytics, etc.)
```

### Step 3: Analyze Current Template vs. Needs

Based on their answers, create a mental model of:

**Keep:**
- Features from the template they'll use
- Example code that matches their patterns

**Modify:**
- Features that need customization
- Schema changes needed
- UI/UX adjustments

**Add:**
- New features not in the template
- Integrations needed
- Additional infrastructure

**Remove:**
- Example features they don't need
- Unused providers or configurations

### Step 4: Present Recommendation Plan

Provide a clear, prioritized plan:

```
Based on what you want to build, here's my recommendation:

## Phase 1: Cleanup (Quick wins)
- Remove the todo example feature
- [Any other cleanup items]

## Phase 2: Core Data Model
- Modify database schema for [their domain]
- Add tables for [specific entities]
- Update relationships

## Phase 3: Essential Features
1. [Most important feature]
2. [Second priority feature]
3. [Third priority feature]

## Phase 4: Nice-to-Haves
- [Additional features]
- [Optimizations]

Would you like me to start with Phase 1, or would you prefer to jump straight into a specific feature?
```

### Step 5: Execution Mode

Once they choose a starting point:
- Use the `/create-spec` skill for any non-trivial feature
- Follow the AGENTS.md workflow for feature development
- Maintain template quality standards (type safety, error handling, etc.)

---

## Key Template Patterns to Maintain

When building on this template, maintain these established patterns:

### Database Patterns
```typescript
// Use UUIDs for non-auth tables
import { uuid } from "drizzle-orm/pg-core";
export const myTable = pgTable("my_table", {
  id: uuid("id").defaultRandom().primaryKey(),
  // ... other fields
});

// Better Auth tables use text IDs (don't change)
```

### API Route Patterns
```typescript
// Always check authentication
const session = await auth.api.getSession({ headers: await headers() });
if (!session) {
  return new Response(JSON.stringify({ error: "Unauthorized" }), {
    status: 401,
  });
}

// Validate input
// Handle errors properly
// Return consistent JSON responses
```

### Component Patterns
```typescript
// Use shadcn/ui components from @/components/ui
// Follow the existing structure in components/
// Maintain TypeScript strict typing
// Use server components by default, add "use client" only when needed
```

### Auth Patterns
```typescript
// Server-side: use auth.api methods
// Client-side: use hooks from auth-client
// Protect routes in layout.tsx or page.tsx
```

---

## Things to Explicitly Mention

### Production Readiness Gaps

Always mention these items that need attention before production:

1. **Email Integration**
   - "Currently, password reset and verification emails just log to the console. You'll need to integrate a real email service (Resend, SendGrid, etc.) before going to production."

2. **Error Monitoring**
   - "Consider adding Sentry or another error monitoring solution."

3. **Analytics**
   - "You may want to add analytics (Vercel Analytics, Posthog, etc.)"

4. **Security Hardening**
   - "Before production, review: rate limiting, CORS settings, CSP headers, etc."

5. **Environment Secrets**
   - "Use different BETTER_AUTH_SECRET values for dev/staging/production"
   - "Never commit .env to version control"

### Common Modifications

Be ready to help with these frequent customization requests:

- Removing example features (todo, chat)
- Adding new database tables
- Integrating payment providers (Stripe, Polar)
- Setting up email services
- Adding new OAuth providers
- Customizing UI theme and branding
- Adding file upload functionality
- Implementing role-based access control
- Adding API rate limiting
- Setting up webhooks

---

## Template-Specific Commands

Guide users to leverage the included skills:

- `/create-spec` - For planning any non-trivial feature
- `/implement-feature` - For orchestrated implementation
- `shadcn` skill - For adding UI components
- `nextjs` skill - For Next.js-specific patterns
- `better-auth-best-practices` - For auth-related features

---

## Success Criteria

A successful onboarding conversation should result in:

1. ✅ Developer understands what the template includes
2. ✅ Developer has a clear plan for their first features
3. ✅ Template example code is cleaned up appropriately
4. ✅ Database schema reflects their domain model
5. ✅ Core features are implemented following template patterns
6. ✅ Production readiness gaps are documented/addressed

---

## Example Onboarding Scenarios

### Scenario 1: SaaS Product
User: "I want to build a project management tool with teams and tasks"

**Your approach:**
- Keep: Auth system, database setup, AI (might use for smart features)
- Remove: Todo example (will build custom task system)
- Add: Teams/organizations, projects, tasks, assignments, permissions
- Suggest: Role-based access, team invites, Stripe for billing

### Scenario 2: Internal Tool
User: "Building an internal admin panel to manage customer data"

**Your approach:**
- Keep: Auth, database, basic UI
- Remove: Google OAuth (internal only), AI chat
- Add: RBAC, data tables, bulk operations, audit logs
- Suggest: CSV export, advanced filtering, admin-only access

### Scenario 3: AI-Powered App
User: "Creating an AI writing assistant"

**Your approach:**
- Keep: Everything, especially AI integration
- Modify: Chat UI for writing-specific use case
- Add: Document storage, templates, export functionality
- Suggest: Prompt engineering, streaming improvements, usage tracking

---

## Anti-Patterns to Avoid

❌ **Don't** start coding immediately without understanding their project
❌ **Don't** assume they need all template features
❌ **Don't** make breaking changes without explaining the impact
❌ **Don't** add features without following the established patterns
❌ **Don't** skip the planning phase for complex features

✅ **Do** ask clarifying questions
✅ **Do** explain trade-offs when suggesting approaches
✅ **Do** maintain code quality standards from the template
✅ **Do** use the `/create-spec` workflow for substantial work
✅ **Do** document changes and decisions

---

## Your First Message Template

When you detect a new template project, start with something like:

```
👋 Welcome to your new project! I can see you're starting from the full-stack Next.js template.

This template gives you a production-ready foundation with:
- ✅ Authentication (Better Auth with email/password + Google OAuth)
- ✅ PostgreSQL database (Drizzle ORM)
- ✅ AI integration (Vercel AI SDK with Google AI/Gemini)
- ✅ UI components (shadcn/ui + Tailwind CSS)
- ✅ Example features (todos, chat interface)

Before we start building, let me make sure everything is set up correctly.

**Quick Setup Check:**
1. Have you updated all environment variables in `.env`? (especially POSTGRES_URL, BETTER_AUTH_SECRET, Google credentials)
2. Have you run database migrations? (`npm run db:migrate`)
3. Is the app running on http://localhost:3000?

Once that's confirmed, I'll help you plan what to build next! 🚀
```

---

**Remember:** Your goal is to help them go from "empty template" to "building their unique product" as smoothly as possible, while maintaining the quality and patterns established by the template.
