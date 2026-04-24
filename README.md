# Full-Stack Next.js Template with AI, Auth, and Database

A production-ready Next.js template with everything you need to build modern web applications: authentication, database, AI integration, and beautiful UI components.

## 🎯 What's Included

This template comes pre-configured with a carefully chosen default stack, **but every component is swappable**!

### Default Stack (All Components Swappable!)

#### Core Infrastructure
- ✅ **Next.js 16** - App Router with React Server Components
- ✅ **TypeScript** - Full type safety across the stack
- ✅ **PostgreSQL Database** - Configured with Drizzle ORM
  - 🔄 *Can swap to: Supabase, Prisma, MongoDB*
- ✅ **Better Auth** - Complete authentication system
  - Email/password authentication
  - Google OAuth integration
  - Session management
  - Email verification flows
  - 🔄 *Can swap to: Supabase Auth, Clerk, Auth.js*

#### AI Integration
- ✅ **Vercel AI SDK** - Streaming AI chat responses
- ✅ **Multiple AI Providers** - Pre-configured for:
  - Google AI (Gemini) - Free tier available
  - OpenRouter - Access to 100+ models
  - 🔄 *Can swap to: LangChain, OpenAI SDK, Anthropic SDK*

#### UI & Styling
- ✅ **shadcn/ui** - High-quality React components (New York style)
  - 🔄 *Can swap to: Material UI, Chakra UI, Headless UI*
- ✅ **Tailwind CSS v4** - Utility-first CSS framework
  - 🔄 *Can swap to: CSS Modules, styled-components, Emotion*
- ✅ **Dark Mode** - Built-in theme switching
- ✅ **Responsive Design** - Mobile-first approach

#### Deployment
- ✅ **Vercel** - Zero-config deployment with CI/CD
  - 🔄 *Can swap to: Netlify, Self-hosted (Docker), AWS/Azure/GCP*

> **📚 See [STACK_OPTIONS.md](./STACK_OPTIONS.md)** for detailed comparison of all alternatives and how to swap them.

### Pre-built Components
- Authentication UI (sign in, sign up, user menu)
- AI chat interface with streaming responses
- User dashboard
- Theme toggle
- Modal dialogs
- Forms and inputs
- Loading states and skeletons

### Developer Experience
- ESLint and Prettier configured
- Type checking scripts
- Database migration tools
- Hot reload with Turbopack
- Path aliases configured (`@/components`, `@/lib`, etc.)

---

## 🚀 Getting Started

### Prerequisites
- Node.js 18+ installed
- PostgreSQL database (local or hosted)
- Git

### 1. Clone This Template

```bash
# Clone the repository
git clone [YOUR_TEMPLATE_REPO_URL] my-new-project
cd my-new-project

# Remove the old git history and start fresh
rm -rf .git
git init
git add .
git commit -m "Initial commit from template"
```

### 2. Install Dependencies

```bash
npm install
# or
pnpm install
# or
yarn install
```

### 3. Configure Environment Variables

**IMPORTANT:** You MUST update these environment variables before running the app.

Copy the example file:
```bash
cp env.example .env
```

Then update `.env` with your own values:

#### Required Changes

| Variable | What to Change | Where to Get It |
|----------|---------------|-----------------|
| `POSTGRES_URL` | Replace with YOUR database connection string | Local PostgreSQL or [Vercel Postgres](https://vercel.com/docs/storage/vercel-postgres) |
| `BETTER_AUTH_SECRET` | Generate a NEW random 32+ character secret | Run: `openssl rand -base64 32` |
| `GOOGLE_CLIENT_ID` | Your Google OAuth credentials | [Google Cloud Console](https://console.cloud.google.com/apis/credentials) |
| `GOOGLE_CLIENT_SECRET` | Your Google OAuth credentials | [Google Cloud Console](https://console.cloud.google.com/apis/credentials) |
| `GOOGLE_AI_API_KEY` | Your Google AI API key (for Gemini) | [Google AI Studio](https://aistudio.google.com/app/apikey) |
| `NEXT_PUBLIC_APP_URL` | Your app URL | `http://localhost:3000` (dev) or your production URL |

#### Optional Variables

| Variable | Purpose | Required? |
|----------|---------|-----------|
| `OPENROUTER_API_KEY` | Alternative AI provider | No - only if using OpenRouter instead of Google AI |
| `OPENROUTER_MODEL` | OpenRouter model selection | No - defaults to Llama 3.2 free model |
| `BLOB_READ_WRITE_TOKEN` | Vercel Blob storage for file uploads | No - uses local storage if not set |
| `POLAR_WEBHOOK_SECRET` | Payment processing | No - only if implementing payments |
| `POLAR_ACCESS_TOKEN` | Payment processing | No - only if implementing payments |

#### Getting API Keys

**Google OAuth Setup:**
1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project or select existing
3. Enable Google+ API
4. Go to Credentials → Create OAuth 2.0 Client ID
5. Add authorized redirect URIs:
   - `http://localhost:3000/api/auth/callback/google` (development)
   - `https://yourdomain.com/api/auth/callback/google` (production)
6. Copy Client ID and Secret to `.env`

**Google AI (Gemini) Setup:**
1. Visit [Google AI Studio](https://aistudio.google.com/app/apikey)
2. Click "Get API key"
3. Copy the key to `GOOGLE_AI_API_KEY` in `.env`
4. Free tier includes generous limits

**OpenRouter Setup (Optional):**
1. Visit [OpenRouter](https://openrouter.ai/)
2. Sign up and go to [API Keys](https://openrouter.ai/settings/keys)
3. Create a new key
4. Browse [available models](https://openrouter.ai/models) and update `OPENROUTER_MODEL`

### 4. Set Up Database

Run database migrations:
```bash
npm run db:migrate
```

Or for development (pushes schema directly):
```bash
npm run db:push
```

### 5. Start Development

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

---

## 🤖 Working with Claude Code

This template is optimized for development with [Claude Code](https://claude.ai/code).

### Complete Setup Process with Claude

**Claude will guide you through THREE stages:**

#### Stage 1: Verify Dependencies ✅
Claude will check that you have installed:
- Node.js 18+
- Git
- Docker Desktop
- All required accounts (GitHub, Vercel, Google Cloud, AI provider)

If anything is missing, Claude will guide you to install it.

#### Stage 2: Set Up Both Environments ✅
Claude will help you configure:

**Local Development:**
- Environment variables
- Local PostgreSQL database (Docker)
- Google OAuth for localhost
- AI integration
- Test sign up/sign in locally

**Production Deployment:**
- Vercel project creation
- Production database (Neon)
- Environment variables in Vercel
- Google OAuth for production domain
- Deploy and test live app
- Verify CI/CD pipeline (GitHub → Vercel)

#### Stage 2.5: Choose Your Stack (Optional) 🔄

**Before building features, Claude will ask:**

```
Do you want to use the default stack or swap any components?
```

**You can swap:**
- 🔐 Auth: Better Auth → Supabase Auth, Clerk, Auth.js
- 🗄️ Database: Drizzle → Supabase, Prisma, MongoDB
- 🤖 AI: Vercel AI SDK → LangChain, OpenAI, Anthropic
- 🎨 UI: shadcn/ui → Material UI, Chakra UI
- 🚀 Deploy: Vercel → Netlify, Self-hosted

**Claude will:**
- Explain pros/cons of each option
- Recommend based on your use case
- Help you swap components safely
- Test both environments after swapping
- Update documentation with your choices

**📖 See [STACK_OPTIONS.md](./STACK_OPTIONS.md)** for all available alternatives

**⚠️ Important:** Claude will NOT help build features until:
- BOTH environments are verified working
- Stack choices are confirmed (default or swapped)
- Any swaps are tested in both environments

#### Stage 3: Build Your Application 🚀
Once both environments are working:
1. Claude asks what you want to build
2. Creates a plan tailored to your needs
3. Implements features following best practices
4. Tests locally first
5. Helps deploy to production

### Example First Conversation

```
You: [Open Claude Code in this project]

Claude: 👋 Welcome! I see you're using the Next.js template.
        Let's verify your setup before building...
        
        Do you have Node.js, Git, and Docker installed?
        Have you set up local development (QUICKSTART.md)?
        Have you deployed to production (DEPLOYMENT.md)?

You: I have Node and Git but haven't set up anything else yet.

Claude: Perfect! I'll guide you through:
        1. First, installing Docker
        2. Then setting up local development (5 min)
        3. Then deploying to production (20 min)
        4. THEN we'll build your app!
        
        Let's start - what operating system are you on?
```

### Why This Order Matters

Setting up BOTH environments first means:
- ✅ You can develop features locally (fast iteration)
- ✅ You can deploy to production easily (just `git push`)
- ✅ You won't hit surprises when deploying later
- ✅ You have a professional development workflow from day one

### Once Setup is Complete

After both environments work, Claude will ask:
- "What type of application are you building?"
- "Who are the main users?"
- "What are the core features you need?"

Then Claude will help you build using the **GSD (Get-Shit-Done) framework**.

### Building with GSD Framework

This template includes the **GSD framework** - a structured approach to building applications:

**What GSD Does:**
- 📋 Breaks your app into manageable phases
- 🎯 Plans each feature thoroughly before coding
- ✅ Executes with built-in quality checks
- 📊 Tracks progress and maintains documentation
- 🔄 Ensures code review between each phase
- 🚀 Deploys incrementally with verification

**Example GSD Workflow:**
```
You: "I want to build a task management app with teams"

Claude: I'll use the GSD framework to help you build this systematically.

Phase 1: Team Management
  → Plan data model
  → Implement team CRUD
  → Test and verify
  → Deploy to production

Phase 2: Task Management  
  → Plan task features
  → Implement task CRUD
  → Link to teams
  → Test and deploy

... and so on
```

**GSD Commands Claude Will Use:**
- `/gsd-new-project` - Start a new project with roadmap
- `/gsd-plan-phase` - Plan the next phase
- `/gsd-execute-phase` - Build the phase step-by-step
- `/gsd-verify-work` - Verify everything works
- `/gsd-ship` - Create PR and deploy

**Why GSD?**
- ✅ Systematic approach prevents missed requirements
- ✅ Built-in quality checks catch issues early
- ✅ Progress tracking shows what's done and what's next
- ✅ Incremental deployment reduces risk
- ✅ Documentation auto-generated as you build

---

**Ready to start?** Open this project in Claude Code and let Claude guide you through:
1. ✅ Complete setup verification (both environments)
2. ✅ Dependency checks
3. 🚀 Structured app development with GSD framework

---

## 📁 Project Structure

```
.
├── src/
│   ├── app/                    # Next.js App Router
│   │   ├── api/
│   │   │   ├── auth/          # Better Auth endpoints
│   │   │   └── chat/          # AI chat API route
│   │   ├── chat/              # AI chat page
│   │   ├── dashboard/         # User dashboard
│   │   └── page.tsx           # Home page
│   │
│   ├── components/            # React components
│   │   ├── auth/             # Authentication components
│   │   └── ui/               # shadcn/ui components
│   │
│   └── lib/                  # Core utilities
│       ├── auth.ts           # Better Auth config
│       ├── auth-client.ts    # Client-side auth
│       ├── db.ts             # Database connection
│       ├── schema.ts         # Drizzle schema
│       └── utils.ts          # Helper functions
│
├── drizzle/                   # Database migrations
├── .claude/                   # Claude Code configuration
└── public/                    # Static assets
```

---

## 🗄️ Database Schema

The template includes these pre-configured tables:

### Authentication Tables (Better Auth)
- `user` - User accounts
- `session` - Active sessions
- `account` - OAuth provider accounts
- `verification` - Email verification tokens

### Example Application Table
- `todo` - Sample todos (you can modify or remove this)

### Modifying the Schema

1. Edit `src/lib/schema.ts`
2. Generate migration: `npm run db:generate`
3. Apply migration: `npm run db:migrate`
4. Or push directly in dev: `npm run db:push`

---

## 🛠️ Available Scripts

| Command | Description |
|---------|-------------|
| `npm run dev` | Start development server with Turbopack |
| `npm run build` | Build for production |
| `npm run start` | Start production server |
| `npm run lint` | Run ESLint |
| `npm run typecheck` | Run TypeScript compiler check |
| `npm run check` | Run both lint and typecheck |
| `npm run db:generate` | Generate new migration from schema changes |
| `npm run db:migrate` | Run pending migrations |
| `npm run db:push` | Push schema directly (dev only) |
| `npm run db:studio` | Open Drizzle Studio (database GUI) |
| `npm run db:reset` | Drop all tables and recreate |

---

## 🚀 Deployment

**📘 Full deployment guide:** See [DEPLOYMENT.md](./DEPLOYMENT.md) for complete step-by-step instructions.

### Quick Deployment Summary

1. **Prepare:** Gather all environment variables from `.env`
2. **Create Vercel Project:** Import your GitHub repository
3. **Set Up Production Database:** Create Neon Postgres in Vercel Storage
4. **Configure Environment Variables:** Add all env vars in Vercel dashboard
5. **Deploy:** Vercel builds and deploys automatically
6. **Run Migrations:** Execute database migrations on production
7. **Update URLs:** Set production domain in `NEXT_PUBLIC_APP_URL` and Google OAuth

### Critical Production Setup

⚠️ **Before deploying, you MUST:**
- Create a production database (see Step 4 in DEPLOYMENT.md)
- Generate a NEW `BETTER_AUTH_SECRET` for production (different from dev)
- Add production callback URL to Google OAuth credentials
- Set up email service (currently logs to console - not suitable for production)

### Environment Variables for Production

| Variable | Where to Get | Notes |
|----------|--------------|-------|
| `POSTGRES_URL` | Vercel/Neon Database | **Different from local!** |
| `BETTER_AUTH_SECRET` | `openssl rand -base64 32` | **Must be different from dev!** |
| `GOOGLE_CLIENT_ID` | Google Cloud Console | Same as dev |
| `GOOGLE_CLIENT_SECRET` | Google Cloud Console | Same as dev |
| `GOOGLE_AI_API_KEY` | Google AI Studio | Same as dev |
| `NEXT_PUBLIC_APP_URL` | Vercel deployment URL | e.g., `https://your-app.vercel.app` |

**📖 Complete deployment guide:** [DEPLOYMENT.md](./DEPLOYMENT.md)

---

## ⚠️ Things You'll Want to Customize

### Before Production

1. **Email Integration** - Currently email verification and password reset URLs are logged to the console. You'll want to:
   - Add a transactional email service (Resend, SendGrid, etc.)
   - Update `src/lib/auth.ts` to send real emails

2. **Branding** - Update:
   - Site name in `src/app/layout.tsx`
   - Favicon in `public/`
   - Metadata and SEO tags

3. **Remove Example Features**
   - The `todo` table and related code is just an example
   - Delete or modify based on your needs

4. **Error Monitoring** - Consider adding:
   - Sentry for error tracking
   - Analytics (Vercel Analytics, Google Analytics, etc.)

5. **Review Security Settings**
   - Update CORS settings if needed
   - Review Better Auth configuration
   - Add rate limiting for API routes

---

## 📚 What's NOT Included (But Easy to Add)

This template provides the foundation, but you might want to add:

- 📧 Email service integration (Resend, SendGrid)
- 📊 Analytics (Vercel Analytics, Posthog)
- 🐛 Error monitoring (Sentry)
- 🧪 Testing setup (Vitest, Playwright)
- 💳 Payment processing (Stripe, Polar)
- 📁 Advanced file uploads
- 🔍 Full-text search
- 📱 Mobile app (React Native, Expo)
- 🌐 i18n (internationalization)

Ask Claude to help add any of these features!

---

## 🤝 Contributing

Found a bug or have a suggestion for the template?
1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

---

## 📝 License

This template is open source and available under the MIT License.

---

## 🆘 Need Help?

- Check the existing code and comments
- Ask Claude Code for help (it knows the entire template)
- Review the documentation for individual packages:
  - [Next.js Docs](https://nextjs.org/docs)
  - [Better Auth](https://better-auth.com)
  - [Drizzle ORM](https://orm.drizzle.team)
  - [Vercel AI SDK](https://sdk.vercel.ai)
  - [shadcn/ui](https://ui.shadcn.com)

---

**Happy Building! 🚀**

Built with ❤️ and Claude Code
