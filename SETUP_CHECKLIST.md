# Template Setup Checklist

Use this checklist to ensure your project is properly configured after cloning this template.

## ✅ Initial Setup

- [ ] Cloned/copied the template repository
- [ ] Removed old git history: `rm -rf .git && git init`
- [ ] Installed dependencies: `npm install` (or `pnpm install`/`yarn install`)
- [ ] Created `.env` file from `env.example`: `cp env.example .env`

## ✅ Environment Variables Configuration

### Required for Basic Functionality

- [ ] **POSTGRES_URL** - Updated with your PostgreSQL connection string
  - Local: `postgresql://user:password@localhost:5432/dbname`
  - Vercel: Get from Vercel Storage dashboard
  
- [ ] **BETTER_AUTH_SECRET** - Generated new secret
  - Run: `openssl rand -base64 32`
  - Must be different from the example value
  
- [ ] **NEXT_PUBLIC_APP_URL** - Set to your app URL
  - Development: `http://localhost:3000`
  - Production: Your actual domain

### Required for Google OAuth

- [ ] **GOOGLE_CLIENT_ID** - Created OAuth credentials
  - Get from: [Google Cloud Console](https://console.cloud.google.com/apis/credentials)
  
- [ ] **GOOGLE_CLIENT_SECRET** - From same OAuth credentials
  
- [ ] Added redirect URIs in Google Cloud Console:
  - [ ] `http://localhost:3000/api/auth/callback/google` (dev)
  - [ ] `https://your-domain.com/api/auth/callback/google` (prod, when ready)

### Required for AI Features

Choose at least one AI provider:

#### Option 1: Google AI (Recommended - Free Tier)
- [ ] **GOOGLE_AI_API_KEY** - Created API key
  - Get from: [Google AI Studio](https://aistudio.google.com/app/apikey)
  
- [ ] **GOOGLE_AI_MODEL** - Set model (default: `gemini-1.5-flash`)

#### Option 2: OpenRouter (Alternative)
- [ ] **OPENROUTER_API_KEY** - Created API key
  - Get from: [OpenRouter Keys](https://openrouter.ai/settings/keys)
  
- [ ] **OPENROUTER_MODEL** - Chose model
  - Browse: [OpenRouter Models](https://openrouter.ai/models)

### Optional (Can Configure Later)

- [ ] **BLOB_READ_WRITE_TOKEN** - For file uploads (Vercel Blob)
  - Only needed if implementing file upload features
  
- [ ] **POLAR_WEBHOOK_SECRET** - For payment processing
  - Only if using Polar for payments
  
- [ ] **POLAR_ACCESS_TOKEN** - For payment processing
  - Only if using Polar for payments

## ✅ Database Setup

- [ ] Database migrations generated: `npm run db:generate` (if schema changed)
- [ ] Database migrations applied: `npm run db:migrate`
- [ ] Verified database connection works
- [ ] (Optional) Opened Drizzle Studio to view tables: `npm run db:studio`

## ✅ Verification

- [ ] Started development server: `npm run dev`
- [ ] App loads at `http://localhost:3000`
- [ ] No console errors on page load
- [ ] Can view sign-up page
- [ ] Can create an account
- [ ] Can sign in successfully
- [ ] Can access dashboard at `/dashboard`
- [ ] Can access chat at `/chat` (if using AI features)

## ✅ Code Quality Checks

- [ ] Linting passes: `npm run lint`
- [ ] Type checking passes: `npm run typecheck`
- [ ] Full check passes: `npm run check`

## ✅ Git Repository Setup

- [ ] Created new repository on GitHub/GitLab
- [ ] Added remote: `git remote add origin <your-repo-url>`
- [ ] Updated README.md with your project details (optional)
- [ ] Made initial commit
- [ ] Pushed to remote: `git push -u origin main`

## ✅ Claude Code Integration

- [ ] Opened project in Claude Code CLI or IDE extension
- [ ] Verified Claude can read TEMPLATE.md and AGENTS.md
- [ ] Completed onboarding conversation with Claude
- [ ] Created feature plan for first custom features

## 🚀 Ready to Build!

Once all required items are checked, you're ready to start building your application!

**Next Steps:**
1. Open Claude Code
2. Describe what you want to build
3. Let Claude guide you through planning and implementation

## ⚠️ Production Deployment Checklist

When you're ready to deploy:

- [ ] All environment variables set in hosting provider
- [ ] Production database created and migrated
- [ ] **BETTER_AUTH_SECRET** is different from development
- [ ] Google OAuth redirect URI includes production domain
- [ ] **NEXT_PUBLIC_APP_URL** set to production domain
- [ ] Email integration configured (not console logging)
- [ ] Error monitoring added (Sentry, etc.)
- [ ] Analytics configured (optional)
- [ ] Rate limiting implemented for API routes
- [ ] Security headers configured
- [ ] SSL/TLS certificate configured
- [ ] Domain DNS configured
- [ ] Build succeeds: `npm run build`
- [ ] Tested in production environment

## 📝 Notes

Add any project-specific setup notes here:

```
- 
- 
- 
```

---

**Last Updated:** [Add date when you complete setup]
**Completed By:** [Your name]
