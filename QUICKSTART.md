# ⚡ Quick Start Guide

Get up and running in 5 minutes.

## 1. Clone & Install (1 min)

```bash
# Clone this template
git clone [YOUR_TEMPLATE_REPO_URL] my-app
cd my-app

# Remove old git history
rm -rf .git && git init

# Install dependencies
npm install  # or pnpm install / yarn install
```

## 2. Start Local Database (30 sec)

```bash
# Start PostgreSQL with Docker (easiest option)
docker compose up -d
```

> **Don't have Docker?** You can use a hosted database like [Vercel Postgres](https://vercel.com/storage/postgres) or install PostgreSQL locally.

## 3. Environment Setup (2 min)

```bash
# Copy environment template
cp env.example .env
```

Open `.env` and update these **required** values:

```bash
# 1. Database - If using Docker Compose, this is already correct!
POSTGRES_URL="postgresql://dev_user:dev_password@localhost:5432/postgres_dev"

# 2. Auth Secret - Generate a new one
BETTER_AUTH_SECRET="run: openssl rand -base64 32"

# 3. Google OAuth - Get from: https://console.cloud.google.com/apis/credentials
GOOGLE_CLIENT_ID="your-id.apps.googleusercontent.com"
GOOGLE_CLIENT_SECRET="your-secret"

# 4. AI Provider - Get from: https://aistudio.google.com/app/apikey
GOOGLE_AI_API_KEY="your-key-here"

# 5. App URL
NEXT_PUBLIC_APP_URL="http://localhost:3000"
```

## 4. Database Setup (1 min)

```bash
# Run migrations
npm run db:migrate
```

## 5. Start Development (30 sec)

```bash
# Start the server
npm run dev
```

Open **http://localhost:3000** 🎉

## 6. Verify It Works (30 sec)

✅ Create an account at `/`  
✅ Sign in  
✅ Visit `/dashboard` to see your profile  
✅ Try `/chat` for AI chat

---

## Next Steps

### Option A: Guided Setup with Claude Code
```bash
# Open in Claude Code
claude
```
Say: _"I want to build [describe your app]"_

Claude will:
- Guide you through planning
- Help customize the template
- Implement features for you

### Option B: Manual Customization

1. **Remove example features** you don't need:
   - `src/app/chat/` (AI chat)
   - Todo-related code in schema
   
2. **Update branding**:
   - Site name in `src/app/layout.tsx`
   - Favicon in `public/`
   
3. **Modify database schema**:
   - Edit `src/lib/schema.ts`
   - Run `npm run db:generate`
   - Run `npm run db:migrate`

---

## Troubleshooting

### "Database connection error"
- Make sure PostgreSQL is running: `docker compose up -d`
- Check if running: `docker compose ps`
- Verify `POSTGRES_URL` in `.env` is correct
- Check database credentials match docker-compose.yml

### "Authentication error"
- Verify `BETTER_AUTH_SECRET` is set
- Make sure it's at least 32 characters

### "Google OAuth not working"
- Check `GOOGLE_CLIENT_ID` and `GOOGLE_CLIENT_SECRET`
- Verify redirect URI in Google Console includes `http://localhost:3000/api/auth/callback/google`

### "AI chat not working"
- Verify `GOOGLE_AI_API_KEY` is set
- Or set up OpenRouter with `OPENROUTER_API_KEY`

### "Build errors"
```bash
# Clear cache and rebuild
rm -rf .next
npm run build
```

### "Type errors"
```bash
# Check types
npm run typecheck
```

---

## Useful Commands

```bash
# Development
npm run dev              # Start dev server
npm run build            # Build for production
npm run start            # Run production build

# Database
npm run db:studio        # Open database GUI
npm run db:push          # Quick schema push (dev)
npm run db:migrate       # Run migrations (prod)
npm run db:generate      # Generate new migration

# Quality
npm run lint             # Run linter
npm run typecheck        # Type check
npm run check            # Run both
```

---

## Production Deployment

### Vercel (Recommended)

```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel --prod
```

Add all environment variables in Vercel dashboard:
- All variables from `.env`
- Use **production values** for:
  - `POSTGRES_URL` (production database)
  - `BETTER_AUTH_SECRET` (new secret)
  - `NEXT_PUBLIC_APP_URL` (your domain)

Don't forget to add your production domain to Google OAuth redirect URIs!

---

## Resources

📖 **Detailed docs**: See [README.md](./README.md)  
✅ **Full setup checklist**: See [SETUP_CHECKLIST.md](./SETUP_CHECKLIST.md)  
🔧 **What's missing**: See [WHATS_MISSING.md](./WHATS_MISSING.md)  
🤖 **Template instructions**: See [TEMPLATE.md](./TEMPLATE.md) (for Claude)

---

**Need help?** Ask Claude Code or open an issue!

Happy coding! 🚀
