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

### Step 1: Welcome and Verify BOTH Environments

Start with:
```
Welcome! I can see you're starting a new project with this template. Let me help you get going.

Before we start building your application, I need to verify that BOTH your development environments are set up correctly. This is crucial for a smooth development experience.

**Let's check your local development environment:**
1. Is the app running successfully on localhost:3000?
2. Have you updated all required environment variables in .env?
3. Have you run database migrations (npm run db:migrate)?
4. Can you sign up and sign in locally?

**Now let's check your production environment:**
1. Have you deployed to Vercel? (If not, we'll do this together)
2. Is your production app live at a Vercel URL?
3. Have you set up the production database (Neon)?
4. Can you sign in on your production URL?
5. When you push to GitHub, does it auto-deploy to Vercel?

If EITHER environment is not set up, I'll guide you through it step-by-step before we start building your application.
```

**Critical: DO NOT proceed to Step 2 (Understanding Their Project) until BOTH environments are verified working.**

If local setup is incomplete:
- Guide them through QUICKSTART.md
- Help install any missing dependencies (Node.js, Docker, Git)
- Verify each step completes successfully

If production setup is incomplete:
- Guide them through DEPLOYMENT.md
- Help create Vercel account
- Walk through database setup
- Verify deployment works
- Test the CI/CD pipeline (make a small change, push, verify auto-deploy)

**Why this matters:** Users should have a complete professional workflow (local dev + production + CI/CD) BEFORE building features. This prevents frustration later and ensures they can deploy changes easily.

### Step 2: Verify Dependencies

Before diving into their project, quickly verify they have all required software:

```
Great! Now let me quickly verify you have all the tools you need.

Can you run these commands and tell me the results?
```bash
node --version   # Need 18+
npm --version
git --version
docker --version
```

If you see version numbers for all of these, you're good to go! 

If any are missing, I'll help you install them now before we continue.
```

**If dependencies are missing:**
- Guide them to download and install (provide links)
- Wait for confirmation they've installed
- Verify installation worked
- Only proceed when all dependencies are confirmed

**Required dependencies:**
- Node.js 18+
- npm/pnpm/yarn
- Git
- Docker Desktop (for local database)

### Step 3: Understand Their Project

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

### Step 4: Analyze Current Template vs. Needs

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

### Step 5: Present Recommendation Plan

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

### Step 6: Execution Mode

Once they choose a starting point:
- **Recommend using the GSD (Get-Shit-Done) framework** for structured development
- Use the `/create-spec` skill for any non-trivial feature
- Follow the AGENTS.md workflow for feature development
- Maintain template quality standards (type safety, error handling, etc.)

**About GSD Framework:**
This template includes the GSD (Get-Shit-Done) framework, which provides:
- Structured project planning with phases and milestones
- Step-by-step execution with automatic verification
- Code review and quality checks
- Progress tracking and documentation

**Recommend to users:**
```
Now that your environments are set up, I recommend using the GSD framework to build your application systematically.

The GSD framework will help us:
1. Break your app into manageable phases
2. Plan each feature thoroughly before coding
3. Execute with built-in quality checks
4. Track progress and maintain documentation

Would you like me to use GSD to plan and build your application step-by-step?
```

**If user agrees:**
- Use GSD commands from the skills list (check system-reminder for available GSD skills)
- Start with `/gsd-new-project` or `/gsd-plan-phase` depending on project state
- Guide them through structured development

**If user prefers manual approach:**
- Use `/create-spec` and `/implement-feature` skills
- Follow AGENTS.md workflow
- Still maintain quality standards

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

1. ✅ **BOTH environments verified and working:**
   - ✅ Local development environment (localhost:3000)
   - ✅ Production environment (Vercel deployment)
   - ✅ CI/CD pipeline tested (push to GitHub → auto-deploy)
2. ✅ **All dependencies installed and confirmed:**
   - ✅ Node.js 18+, Git, Docker all verified
   - ✅ All required accounts created (GitHub, Vercel, Google Cloud, AI provider)
3. ✅ Developer understands what the template includes
4. ✅ Developer has a clear plan for their first features
5. ✅ Template example code is cleaned up appropriately
6. ✅ Database schema reflects their domain model
7. ✅ Core features are implemented following template patterns
8. ✅ Production readiness gaps are documented/addressed

**Critical:** Items 1 and 2 MUST be completed before items 3-8. Do not help build features until the complete infrastructure is verified working.

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
- ✅ Docker Compose for local development
- ✅ CI/CD pipeline ready for Vercel

Before we start building your application, I need to verify you have a complete professional development setup. This means BOTH a local development environment AND a production deployment working together.

**First, let me check your local development environment:**

1. Do you have these installed? (Run these commands to check):
   ```bash
   node --version   # Need 18+
   npm --version
   git --version
   docker --version
   ```

2. Have you completed the local setup from QUICKSTART.md?
   - Updated environment variables in `.env`?
   - Started Docker database? (`docker compose up -d`)
   - Run database migrations? (`npm run db:migrate`)
   - App running on http://localhost:3000?
   - Can you sign up and sign in locally?

**Next, let's verify your production environment:**

1. Have you deployed to Vercel yet?
2. If yes:
   - What's your production URL?
   - Can you sign in on production?
   - When you push to GitHub, does it auto-deploy?

3. If not yet deployed:
   - No problem! I'll guide you through DEPLOYMENT.md step-by-step
   - We'll set up Vercel, create a production database, and test the CI/CD pipeline
   - It takes about 15-20 minutes

**Why verify both environments first?**
Because you need a complete development workflow (local → GitHub → production) before building features. This ensures you can develop locally, test changes, and deploy them automatically.

Let me know what's set up and what still needs to be done, and I'll help you complete everything before we start building your app! 🚀
```

---

**Remember:** Your goal is to help them go from "empty template" to "building their unique product" as smoothly as possible, while maintaining the quality and patterns established by the template.
