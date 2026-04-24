# 🚀 Production Deployment Guide

This guide walks you through deploying your application to production on Vercel with a production database.

## ⚠️ Prerequisites & Dependencies

### Required Software & Accounts

**Before you start deployment, verify you have ALL of these:**

#### 1. Local Development Setup ✅
- [ ] **Node.js 18+** installed
  - Check: `node --version`
  - Install: https://nodejs.org/
- [ ] **npm/pnpm/yarn** installed
  - Check: `npm --version`
  - Included with Node.js
- [ ] **Git** installed
  - Check: `git --version`
  - Install: https://git-scm.com/
- [ ] **Docker Desktop** installed and running (for local database)
  - Check: `docker --version`
  - Install: https://www.docker.com/products/docker-desktop/
- [ ] Application running successfully locally (`npm run dev`)
- [ ] All environment variables configured in `.env`
- [ ] Database migrations working (`npm run db:migrate`)

#### 2. Online Accounts ✅
- [ ] **GitHub account** created
  - Sign up: https://github.com/join
  - Code already pushed to GitHub repository
- [ ] **Vercel account** created (free tier is fine)
  - Sign up: https://vercel.com/signup
  - Connected to your GitHub account
- [ ] **Google Cloud account** with OAuth credentials setup
  - Console: https://console.cloud.google.com/
  - OAuth 2.0 Client ID created
  - Production callback URL ready to add
- [ ] **AI Provider account** (choose one):
  - **Google AI Studio** account (recommended - free tier)
    - Sign up: https://aistudio.google.com/
    - API key generated
  - OR **OpenRouter** account
    - Sign up: https://openrouter.ai/
    - API key generated

#### 3. Environment Variables ✅
- [ ] All environment variables from `.env` documented
- [ ] Google OAuth Client ID and Secret ready
- [ ] AI provider API key ready
- [ ] New production `BETTER_AUTH_SECRET` generated

**⚠️ STOP HERE if any dependency is missing!**

If you're missing any of the above:
1. **Install required software** - Follow the links above
2. **Create required accounts** - All have free tiers
3. **Complete local setup** - Follow [QUICKSTART.md](./QUICKSTART.md) first
4. **Ask Claude for help** - Claude can guide you through installing missing dependencies

**Important:** Gather ALL your environment variables from `.env` before starting. You'll need them in step 4.

---

## 📋 Deployment Steps

### Step 1: Prepare Environment Variables

**Copy your `.env` file for reference:**

```bash
# Create a production env reference file (don't commit this!)
cp .env .env.production.reference
```

**Open `.env.production.reference` and prepare to change:**
- `POSTGRES_URL` - Will be replaced with Vercel/Neon production database
- `BETTER_AUTH_SECRET` - Generate a NEW secret for production (different from dev)
- `NEXT_PUBLIC_APP_URL` - Will be your Vercel domain
- Keep everything else the same (Google OAuth, AI keys, etc.)

**Generate a new production auth secret:**
```bash
openssl rand -base64 32
```
Copy this somewhere safe - you'll use it as `BETTER_AUTH_SECRET` in production.

---

### Step 2: Update Google OAuth for Production

Before deploying, add your production callback URL to Google OAuth:

1. Go to [Google Cloud Console](https://console.cloud.google.com/apis/credentials)
2. Select your OAuth 2.0 Client ID
3. Under "Authorized redirect URIs", add:
   ```
   https://your-app-name.vercel.app/api/auth/callback/google
   ```
   (You'll update this with the real domain after deployment)
4. Click **Save**

> **Tip:** You can add multiple redirect URIs, so keep your localhost URL too for local development.

---

### Step 3: Create Vercel Project

#### Option A: Vercel Dashboard (Recommended)

1. **Go to Vercel:** https://vercel.com/new
2. **Import Git Repository:**
   - Click "Import Git Repository"
   - Select your GitHub account
   - Find and select your repository (e.g., `your-username/your-project-name`)
   - Click "Import"

3. **Configure Project:**
   - **Framework Preset:** Next.js (auto-detected)
   - **Root Directory:** `./` (leave as default)
   - **Build Command:** `npm run build` (auto-detected)
   - **Install Command:** `npm install` (auto-detected)

4. **DON'T CLICK DEPLOY YET!** We need to set up the database and environment variables first.

#### Option B: Vercel CLI

```bash
# Install Vercel CLI globally
npm install -g vercel

# Login to Vercel
vercel login

# Link to a new project (but don't deploy yet)
vercel link
```

---

### Step 4: Create Production Database (Vercel + Neon)

**Before adding environment variables, set up your production database:**

1. **In Vercel Dashboard:**
   - Go to your project → **Storage** tab
   - Click **Create Database**
   - Select **Neon Serverless Postgres**

2. **Configure Database:**
   - **Database Name:** Choose a name (e.g., `your-app-prod-db`)
   - **Region:** Choose closest to your users
   - Click **Create**

3. **Get Connection String:**
   - After creation, Vercel will show you the connection details
   - Copy the `POSTGRES_URL` connection string
   - It will look like: `postgresql://user:pass@ep-xxx.region.aws.neon.tech/dbname?sslmode=require`

4. **Vercel Auto-Adds to Environment:**
   - Vercel automatically adds `POSTGRES_URL` to your environment variables
   - You can verify in: Project Settings → Environment Variables

> **Important:** This is your PRODUCTION database. It's separate from your local Docker PostgreSQL database.

---

### Step 5: Configure Environment Variables

Now add all your environment variables in Vercel:

1. **Go to:** Project Settings → Environment Variables
2. **Add each variable** (one at a time):

#### Required Variables

| Variable | Value | Notes |
|----------|-------|-------|
| `POSTGRES_URL` | *(auto-added by Neon)* | Production database URL from Step 4 |
| `BETTER_AUTH_SECRET` | `<NEW-SECRET>` | The new secret you generated in Step 1 |
| `GOOGLE_CLIENT_ID` | `<same-as-local>` | Copy from your `.env` |
| `GOOGLE_CLIENT_SECRET` | `<same-as-local>` | Copy from your `.env` |
| `GOOGLE_AI_API_KEY` | `<same-as-local>` | Copy from your `.env` (if using Google AI) |
| `GOOGLE_AI_MODEL` | `gemini-1.5-flash` | Or your preferred model |
| `NEXT_PUBLIC_APP_URL` | `https://your-app.vercel.app` | Will update after first deploy |

#### Optional Variables

| Variable | Value | Notes |
|----------|-------|-------|
| `OPENROUTER_API_KEY` | `<same-as-local>` | Only if using OpenRouter |
| `OPENROUTER_MODEL` | `<model-name>` | Only if using OpenRouter |
| `BLOB_READ_WRITE_TOKEN` | *(create in Vercel Blob)* | Only if using file uploads |
| `POLAR_WEBHOOK_SECRET` | `<your-secret>` | Only if using Polar payments |
| `POLAR_ACCESS_TOKEN` | `<your-token>` | Only if using Polar payments |

3. **For each variable:**
   - Click **Add New**
   - Enter **Key** (variable name)
   - Enter **Value**
   - Select environments: ✅ Production, ✅ Preview, ✅ Development
   - Click **Save**

> **Tip:** You can bulk add variables by clicking "Add Multiple" and pasting in `.env` format.

---

### Step 6: Deploy to Vercel

Now you're ready to deploy!

#### From Vercel Dashboard:

1. Go to **Deployments** tab
2. Click **Deploy** (or it may auto-deploy after adding env vars)
3. Wait for build to complete (2-3 minutes)
4. You'll get a production URL like: `https://your-app-xyz.vercel.app`

#### From CLI:

```bash
vercel --prod
```

---

### Step 7: Run Database Migrations on Production

After deployment, you need to run migrations on your production database:

#### Option A: Vercel CLI (Recommended)

```bash
# Connect to production environment
vercel env pull .env.production

# Run migrations against production database
# IMPORTANT: Make sure POSTGRES_URL points to production
npx drizzle-kit migrate
```

#### Option B: Run Migrations Locally Against Production DB

1. **Temporarily update your `.env`:**
   ```bash
   # Save your local POSTGRES_URL
   # Then replace with production URL from Vercel
   POSTGRES_URL="postgresql://user:pass@ep-xxx.region.aws.neon.tech/dbname?sslmode=require"
   ```

2. **Run migrations:**
   ```bash
   npm run db:migrate
   ```

3. **Restore your local `.env`:**
   ```bash
   # Put back your local database URL
   POSTGRES_URL="postgresql://dev_user:dev_password@localhost:5432/postgres_dev"
   ```

> **Warning:** Be careful not to run migrations against the wrong database!

#### Option C: Add Migration to Build Process (Automatic)

The template already includes this in `package.json`:

```json
"build": "pnpm run db:migrate && next build"
```

This automatically runs migrations during Vercel deployment. However, be cautious with this approach in production - you may want to run migrations manually for more control.

---

### Step 8: Update Production URLs

Now that you have your Vercel domain, update these:

#### 1. Update `NEXT_PUBLIC_APP_URL` in Vercel

1. Go to: Project Settings → Environment Variables
2. Find `NEXT_PUBLIC_APP_URL`
3. Update value to: `https://your-actual-domain.vercel.app`
4. Click **Save**
5. Redeploy: Deployments → Click "..." → Redeploy

#### 2. Update Google OAuth Redirect URI

1. Go to [Google Cloud Console](https://console.cloud.google.com/apis/credentials)
2. Edit your OAuth 2.0 Client ID
3. Under "Authorized redirect URIs", update to your real domain:
   ```
   https://your-actual-domain.vercel.app/api/auth/callback/google
   ```
4. Click **Save**

#### 3. Add Custom Domain (Optional but Recommended)

You can use your Vercel domain (`your-app.vercel.app`) or register a custom domain.

**Option A: Use Vercel Domain**
- Your app is already live at: `https://your-app-xyz.vercel.app`
- Free and works immediately
- No additional setup needed
- Update `NEXT_PUBLIC_APP_URL` to this URL (already done in Step 8.1)

**Option B: Register and Use Custom Domain**

1. **Register a Domain** (if you don't have one):
   - [Namecheap](https://www.namecheap.com/)
   - [Google Domains](https://domains.google/)
   - [Cloudflare Registrar](https://www.cloudflare.com/products/registrar/)
   - Or any domain registrar

2. **Add Domain to Vercel:**
   - In Vercel: Project Settings → **Domains**
   - Click **Add Domain**
   - Enter your domain (e.g., `myapp.com`)
   - Click **Add**

3. **Configure DNS:**
   
   Vercel will show you DNS records to add. Add these to your domain registrar:
   
   **For apex domain (myapp.com):**
   ```
   Type: A
   Name: @
   Value: 76.76.21.21
   ```
   
   **For www subdomain (www.myapp.com):**
   ```
   Type: CNAME
   Name: www
   Value: cname.vercel-dns.com
   ```

4. **Wait for DNS Propagation:**
   - Can take 5 minutes to 48 hours (usually 5-15 minutes)
   - Vercel will show "Valid Configuration" when ready
   - SSL certificate is automatically provisioned

5. **Update Environment Variables:**
   - In Vercel: Project Settings → Environment Variables
   - Update `NEXT_PUBLIC_APP_URL` to: `https://myapp.com`
   - Click **Save**
   - Redeploy: Deployments → "..." → **Redeploy**

6. **Update Google OAuth:**
   - Go to [Google Cloud Console](https://console.cloud.google.com/apis/credentials)
   - Edit your OAuth 2.0 Client ID
   - Under "Authorized redirect URIs", add:
     ```
     https://myapp.com/api/auth/callback/google
     https://www.myapp.com/api/auth/callback/google
     ```
   - Click **Save**

7. **Set Primary Domain (Optional):**
   - In Vercel Domains settings
   - Click "..." next to your custom domain
   - Select **Set as Primary**
   - All Vercel URLs will redirect to your custom domain

**Domain Setup Checklist:**
- [ ] Domain registered (if using custom)
- [ ] DNS records added to registrar
- [ ] Domain verified in Vercel (shows "Valid Configuration")
- [ ] SSL certificate active (automatic, shows lock icon)
- [ ] `NEXT_PUBLIC_APP_URL` updated in Vercel
- [ ] Google OAuth redirect URIs updated
- [ ] Redeployed application
- [ ] Tested: `https://yourdomain.com` loads correctly
- [ ] Tested: Authentication works on custom domain

---

### Step 9: Verify Production Deployment

Test your production app:

- [ ] **Visit your Vercel URL** - Page loads without errors
- [ ] **Sign up with email/password** - Creates account
- [ ] **Sign in** - Authentication works
- [ ] **Try Google OAuth** - OAuth flow completes
- [ ] **Visit `/dashboard`** - Shows user info
- [ ] **Try `/chat`** - AI chat works (if configured)
- [ ] **Check Vercel Logs** - No errors in Function Logs
- [ ] **Check Database** - Data is being saved

**View Vercel Logs:**
- Dashboard → Deployments → Click on deployment → Function Logs
- Or use CLI: `vercel logs`

---

## 🔄 CI/CD: Automatic Deployments

Once set up, Vercel automatically deploys when you push to GitHub:

### Automatic Deployment Flow

```
git push origin main
    ↓
GitHub detects push
    ↓
Vercel webhook triggered
    ↓
Vercel builds and deploys
    ↓
Production updated automatically
```

### Branch Deployments

- **`main` branch** → Production deployment
- **Other branches** → Preview deployments
- **Pull Requests** → Automatic preview URLs

### Preview Deployments

Every PR gets a unique preview URL:
- `https://your-app-git-feature-branch.vercel.app`
- Test changes before merging to production
- Shares same environment variables as production

---

## 🛠️ Post-Deployment Setup

### 1. Set Up Email Service (Required for Production)

Currently, email verification and password reset URLs are logged to the console. For production, integrate a real email service:

**Recommended Services:**
- [Resend](https://resend.com/) - Modern, developer-friendly
- [SendGrid](https://sendgrid.com/) - Established, feature-rich
- [AWS SES](https://aws.amazon.com/ses/) - Cost-effective

**Update `src/lib/auth.ts`:**
```typescript
sendVerificationEmail: async ({ user, url }) => {
  // Replace console.log with actual email sending
  await sendEmail({
    to: user.email,
    subject: "Verify your email",
    html: `Click here to verify: ${url}`
  });
},
```

### 2. Set Up Error Monitoring

Add error tracking for production issues:

```bash
# Install Sentry
npm install @sentry/nextjs
npx @sentry/wizard@latest -i nextjs
```

Add `SENTRY_DSN` to Vercel environment variables.

### 3. Set Up Analytics (Optional)

Enable Vercel Analytics:
1. Vercel Dashboard → Analytics tab
2. Click **Enable Analytics**
3. Free for hobby projects

Or install other analytics:
```bash
npm install @vercel/analytics
```

### 4. Enable Vercel Protection (Optional)

Protect preview deployments:
1. Project Settings → Deployment Protection
2. Enable protection for preview deployments
3. Set password or restrict to team members

---

## 🔧 Environment Management

### Local vs. Production Environments

| Environment | Database | Auth Secret | App URL |
|-------------|----------|-------------|---------|
| **Local** | Docker PostgreSQL | Dev secret | localhost:3000 |
| **Production** | Vercel/Neon Postgres | Prod secret (different!) | your-app.vercel.app |

### Best Practices

1. **Never commit `.env` files** - They're in `.gitignore`
2. **Use different secrets for prod/dev** - Especially `BETTER_AUTH_SECRET`
3. **Keep API keys secure** - Use Vercel's encrypted environment variables
4. **Test in preview deployments** - Before merging to main
5. **Monitor production logs** - Check Vercel Function Logs regularly

### Managing Secrets

**Pull production environment locally (for debugging):**
```bash
vercel env pull .env.production
```

**Add new environment variable:**
```bash
vercel env add SECRET_NAME
```

---

## 🚨 Troubleshooting

### Build Fails

**Error: "Cannot connect to database"**
- ✅ Check `POSTGRES_URL` is set in Vercel
- ✅ Verify Neon database is active
- ✅ Check connection string is correct

**Error: "Missing environment variable"**
- ✅ Verify all required env vars are set in Vercel
- ✅ Check variable names match exactly (case-sensitive)
- ✅ Ensure variables are enabled for Production environment

### Authentication Issues

**Google OAuth not working:**
- ✅ Verify production callback URL is added to Google Console
- ✅ Check `NEXT_PUBLIC_APP_URL` matches your actual domain
- ✅ Ensure `GOOGLE_CLIENT_ID` and `GOOGLE_CLIENT_SECRET` are set

**Session issues:**
- ✅ Verify `BETTER_AUTH_SECRET` is set and is different from dev
- ✅ Check it's at least 32 characters long

### Database Issues

**Migrations not running:**
- ✅ Run migrations manually: `npx drizzle-kit migrate`
- ✅ Check `POSTGRES_URL` points to production database
- ✅ Verify database user has CREATE/ALTER permissions

**Connection pool errors:**
- ✅ Neon has connection limits on free tier
- ✅ Consider upgrading plan or implementing connection pooling

### AI Chat Issues

**Chat not working:**
- ✅ Verify `GOOGLE_AI_API_KEY` or `OPENROUTER_API_KEY` is set
- ✅ Check Vercel Function Logs for API errors
- ✅ Ensure API keys are valid and have quota remaining

---

## 📊 Monitoring Production

### Vercel Dashboard

**Key sections to monitor:**
1. **Deployments** - Build status and history
2. **Function Logs** - Runtime errors and console logs
3. **Analytics** - Traffic and performance
4. **Speed Insights** - Web vitals and performance metrics

### Database Monitoring

**Neon Dashboard:**
1. Go to [Neon Console](https://console.neon.tech/)
2. Select your database
3. Monitor:
   - Connection count
   - Query performance
   - Storage usage

---

## 🔄 Deployment Workflow

### Making Changes

```bash
# 1. Make changes locally
# 2. Test thoroughly
npm run dev

# 3. Run type checking and linting
npm run check

# 4. Commit and push
git add .
git commit -m "Your changes"
git push origin main

# 5. Vercel auto-deploys
# 6. Monitor deployment in Vercel dashboard
```

### Rollback if Needed

```bash
# From Vercel Dashboard:
# Deployments → Find previous working deployment → Promote to Production

# Or redeploy a specific commit:
vercel --prod --force
```

---

## ✅ Production Checklist

### Infrastructure Setup
- [ ] All environment variables configured in Vercel
- [ ] Production database created and migrated
- [ ] Google OAuth callback URLs updated for production domain
- [ ] SSL/HTTPS working (automatic with Vercel)
- [ ] Custom domain configured (if using)
- [ ] All URLs correctly set:
  - [ ] `NEXT_PUBLIC_APP_URL` matches actual domain
  - [ ] Google OAuth redirect URIs match actual domain
  - [ ] No `localhost` URLs in production config

### Application Testing
- [ ] Sign up with email/password works
- [ ] Sign in with email/password works
- [ ] Google OAuth sign in works
- [ ] Dashboard loads and shows user data
- [ ] AI chat works (if configured)
- [ ] No console errors in browser
- [ ] No errors in Vercel Function Logs
- [ ] Database writes working (check Neon dashboard)

### Production Readiness (Before Public Launch)
- [ ] Email service integrated (not console.log)
- [ ] Error monitoring set up (Sentry or similar)
- [ ] Analytics configured
- [ ] Rate limiting implemented (see WHATS_MISSING.md)
- [ ] Security headers configured (see WHATS_MISSING.md)
- [ ] Database backups enabled (Neon automatic)
- [ ] Monitoring and alerts set up

---

## 🎯 Complete Environment Verification

**At this point, you should have TWO fully functional environments:**

### ✅ Local Development Environment
```
✓ Code running on localhost:3000
✓ Docker PostgreSQL database
✓ All environment variables in .env
✓ Google OAuth working locally
✓ AI integration working
✓ Git repository with remote on GitHub
```

### ✅ Production Environment
```
✓ Code deployed to Vercel
✓ Neon PostgreSQL database
✓ All environment variables in Vercel
✓ Google OAuth working on production domain
✓ AI integration working
✓ Custom domain (optional)
✓ Automatic CI/CD from GitHub
```

### 🔄 Development Workflow Now Works Like This:

```bash
# 1. Make changes locally
npm run dev
# Test changes at localhost:3000

# 2. Commit and push
git add .
git commit -m "Add new feature"
git push origin main

# 3. Vercel automatically deploys
# No manual deployment needed!
# Changes live at your-app.vercel.app in ~2 minutes

# 4. Verify on production
# Test at https://your-app.vercel.app
```

---

## 🚀 Ready to Build Your Application!

**You now have a complete development infrastructure:**

✅ **Local Environment** - Fast iteration and testing  
✅ **Production Environment** - Live app with automatic deployments  
✅ **CI/CD Pipeline** - Push to GitHub → Auto-deploy to Vercel  
✅ **Production Database** - Separate from local development  
✅ **Authentication** - Working in both environments  
✅ **AI Integration** - Ready to use  

### What to Do Next

**Option 1: Let Claude Guide You with GSD Framework (Recommended)**

Open Claude Code and say:

```
"I have both local development and production environments set up. 
I want to build [describe your app idea]."
```

Claude will use the **GSD (Get-Shit-Done) framework** to help you:

1. ✅ Verify both environments are working
2. ✅ Create a structured project roadmap
3. ✅ Plan features in manageable phases
4. ✅ Implement each phase with quality checks
5. ✅ Test locally before deploying
6. ✅ Deploy to production incrementally
7. ✅ Verify each phase works end-to-end

**GSD Framework Benefits:**
- 📋 **Systematic Planning** - No missed requirements
- ✅ **Quality Checks** - Built-in code review and verification
- 📊 **Progress Tracking** - Always know what's done and what's next
- 🔄 **Incremental Deployment** - Deploy small changes frequently
- 📚 **Auto Documentation** - Documentation generated as you build

**Example GSD Session:**
```
You: "I want to build a SaaS product for project management"

Claude: I'll use GSD to help you build this systematically.
        
        Phase 1: Project Setup
        ✅ Already done - both environments working!
        
        Phase 2: User Workspace
        - Multi-tenant database schema
        - Workspace CRUD operations  
        - User invitations
        
        Phase 3: Project Management
        - Projects within workspaces
        - Task management
        - Assignment system
        
        Let's start with Phase 2. I'll create a detailed plan...
```

**GSD Commands You'll See:**
- `/gsd-new-project` - Creates project roadmap
- `/gsd-plan-phase` - Plans next phase in detail
- `/gsd-execute-phase` - Builds the phase step-by-step
- `/gsd-verify-work` - Verifies phase works correctly
- `/gsd-ship` - Creates PR and deploys to production

**Option 2: Start Building Manually**

1. Read [README.md](./README.md) for project structure
2. Review [WHATS_MISSING.md](./WHATS_MISSING.md) for additional features to add
3. Modify `src/lib/schema.ts` for your data model
4. Build your features in `src/app/`
5. Test locally, commit, and push (auto-deploys!)

---

## ⚠️ Important Notes

### Before Building Features:

**DO NOT start building your app until you have verified:**
- [ ] Local environment working 100%
- [ ] Production environment working 100%
- [ ] Can sign in on both local and production
- [ ] Changes pushed to GitHub deploy automatically to Vercel

**If anything isn't working:**
- Check the troubleshooting section in this guide
- Review Vercel Function Logs for errors
- Ask Claude for help debugging
- Verify all URLs and environment variables are correct

### Development Best Practices:

1. **Always test locally first** before pushing to production
2. **Run `npm run check`** before committing (lint + typecheck)
3. **Keep secrets different** between local and production
4. **Monitor production logs** after deployments
5. **Use preview deployments** for testing (create feature branches)

---

## 🎓 Understanding Your Setup

You now have a **professional-grade development workflow**:

**Local Development:**
- Fast iteration (hot reload)
- Full debugging capabilities
- Isolated database (Docker)
- No risk to production

**Production Deployment:**
- Automatic from GitHub
- Separate production database
- Real user data
- Automatic SSL/HTTPS
- Edge network (fast globally)

**CI/CD Pipeline:**
- Every `git push` triggers deployment
- Preview URLs for feature branches
- Automatic rollback if build fails
- Zero-downtime deployments

**This is the same setup used by professional development teams at startups and large companies.**

You're now ready to build any application you want! 🚀

---

## 🎯 Quick Reference

**Vercel Dashboard:** https://vercel.com/dashboard  
**Neon Console:** https://console.neon.tech/  
**Google Cloud Console:** https://console.cloud.google.com/  

**Useful Commands:**
```bash
# Deploy to production
vercel --prod

# View logs
vercel logs

# Pull environment variables
vercel env pull

# Run migrations on production
npx drizzle-kit migrate
```

---

## 📚 Additional Resources

- [Vercel Documentation](https://vercel.com/docs)
- [Neon Documentation](https://neon.tech/docs)
- [Next.js Deployment](https://nextjs.org/docs/deployment)
- [Drizzle ORM Migrations](https://orm.drizzle.team/kit-docs/overview)
- [Better Auth Production Guide](https://better-auth.com/docs/production)

---

**Need help?** Check the main [README.md](./README.md) or [WHATS_MISSING.md](./WHATS_MISSING.md) for additional production requirements.

**Last Updated:** 2026-04-23
