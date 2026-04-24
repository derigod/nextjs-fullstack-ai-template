# 🔧 Stack Options & Alternatives

This template comes with a default tech stack, but **every component is swappable**. Before building your application, you can choose alternative technologies that better fit your needs.

## 📋 Default Stack Overview

| Component | Default Choice | Why It's Default |
|-----------|---------------|------------------|
| **Framework** | Next.js 16 (App Router) | Industry standard, great DX, RSC support |
| **Language** | TypeScript | Type safety, better tooling |
| **Database** | PostgreSQL + Drizzle ORM | Powerful, SQL, type-safe queries |
| **Authentication** | Better Auth | Modern, flexible, self-hosted |
| **AI Integration** | Vercel AI SDK | Streaming, multi-provider, React hooks |
| **UI Components** | shadcn/ui | Copy-paste, customizable, accessible |
| **Styling** | Tailwind CSS v4 | Utility-first, fast, modern |
| **Deployment** | Vercel | Zero-config, edge network, CI/CD |

---

## 🔄 Swappable Components

### 1. Authentication

#### Default: Better Auth
✅ **Pros:**
- Self-hosted (you control the data)
- Flexible and modern
- Built-in email/password + OAuth
- Session management included
- No monthly costs

❌ **Cons:**
- More setup required
- Need to configure email service
- Less features than managed services

#### Alternative A: **Supabase Auth**

**When to choose:**
- Want managed authentication
- Need built-in email templates
- Want real-time subscriptions
- Planning to use Supabase database

**How to swap:**

1. **Install Supabase:**
   ```bash
   npm install @supabase/supabase-js @supabase/auth-helpers-nextjs
   ```

2. **Create Supabase project:**
   - Go to https://supabase.com/
   - Create new project
   - Copy project URL and anon key

3. **Update environment variables:**
   ```env
   NEXT_PUBLIC_SUPABASE_URL=your-project-url
   NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
   ```

4. **Replace auth files:**
   - Delete `src/lib/auth.ts` and `src/lib/auth-client.ts`
   - Create `src/lib/supabase.ts`:
   ```typescript
   import { createClient } from '@supabase/supabase-js'
   
   export const supabase = createClient(
     process.env.NEXT_PUBLIC_SUPABASE_URL!,
     process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!
   )
   ```

5. **Update auth components:**
   - Modify `src/components/auth/*` to use Supabase methods
   - Replace Better Auth hooks with Supabase hooks

6. **Update schema:**
   - Remove Better Auth tables from `src/lib/schema.ts`
   - Supabase manages auth tables for you

**Claude can help:** Say "I want to swap Better Auth for Supabase Auth"

#### Alternative B: **Clerk**

**When to choose:**
- Want the easiest setup
- Need pre-built UI components
- Want organizations/teams built-in
- Don't mind SaaS pricing

**How to swap:**

1. **Install Clerk:**
   ```bash
   npm install @clerk/nextjs
   ```

2. **Create Clerk account:**
   - Go to https://clerk.com/
   - Create application
   - Copy publishable and secret keys

3. **Update environment variables:**
   ```env
   NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=pk_test_...
   CLERK_SECRET_KEY=sk_test_...
   ```

4. **Wrap app with ClerkProvider:**
   - Update `src/app/layout.tsx`
   - Remove Better Auth session handling

5. **Update components:**
   - Replace auth components with Clerk components
   - Use Clerk hooks (`useUser`, `useAuth`)

**Claude can help:** Say "I want to swap Better Auth for Clerk"

#### Alternative C: **Auth.js (NextAuth.js)**

**When to choose:**
- Need many OAuth providers
- Want open-source solution
- Familiar with NextAuth
- Need adapter flexibility

**How to swap:**

1. **Install Auth.js:**
   ```bash
   npm install next-auth@beta @auth/drizzle-adapter
   ```

2. **Configure Auth.js:**
   - Create `auth.config.ts`
   - Set up providers
   - Configure Drizzle adapter

3. **Update components:**
   - Replace Better Auth hooks with Auth.js hooks

**Claude can help:** Say "I want to swap Better Auth for Auth.js"

---

### 2. Database & ORM

#### Default: PostgreSQL + Drizzle ORM
✅ **Pros:**
- Type-safe SQL queries
- Lightweight and fast
- Full SQL control
- Great migrations

❌ **Cons:**
- More manual than Prisma
- Smaller ecosystem

#### Alternative A: **Supabase (PostgreSQL + Client)**

**When to choose:**
- Want managed PostgreSQL
- Need real-time subscriptions
- Want Row Level Security (RLS)
- Using Supabase Auth

**How to swap:**

1. **Already have Supabase client** (if using Supabase Auth)

2. **Use Supabase for data:**
   ```typescript
   // Instead of Drizzle queries
   const { data, error } = await supabase
     .from('todos')
     .select('*')
     .eq('user_id', userId)
   ```

3. **Remove Drizzle:**
   ```bash
   npm uninstall drizzle-orm drizzle-kit
   ```

4. **Update schema:**
   - Define tables in Supabase dashboard
   - Or use SQL migrations in Supabase

**Claude can help:** Say "I want to use Supabase instead of Drizzle"

#### Alternative B: **Prisma ORM**

**When to choose:**
- Want visual schema management (Prisma Studio)
- Prefer more abstraction
- Like generated types
- Need better tooling support

**How to swap:**

1. **Install Prisma:**
   ```bash
   npm install @prisma/client
   npm install -D prisma
   ```

2. **Initialize Prisma:**
   ```bash
   npx prisma init
   ```

3. **Define schema in `prisma/schema.prisma`:**
   ```prisma
   model User {
     id        String   @id @default(cuid())
     email     String   @unique
     name      String
     createdAt DateTime @default(now())
   }
   ```

4. **Generate client:**
   ```bash
   npx prisma generate
   npx prisma db push
   ```

5. **Replace Drizzle queries:**
   ```typescript
   // Old: drizzle query
   // New: prisma query
   const users = await prisma.user.findMany()
   ```

**Claude can help:** Say "I want to swap Drizzle for Prisma"

#### Alternative C: **MongoDB + Mongoose**

**When to choose:**
- Need document database
- Have unstructured data
- Want schemaless flexibility
- Familiar with NoSQL

**How to swap:**

1. **Install Mongoose:**
   ```bash
   npm install mongoose
   ```

2. **Update database URL:**
   ```env
   MONGODB_URL=mongodb+srv://...
   ```

3. **Define schemas:**
   ```typescript
   const UserSchema = new Schema({
     email: String,
     name: String,
   })
   ```

**Claude can help:** Say "I want to use MongoDB instead of PostgreSQL"

---

### 3. AI Integration

#### Default: Vercel AI SDK (Google AI + OpenRouter)
✅ **Pros:**
- Multi-provider support
- Streaming built-in
- React hooks included
- Easy to switch providers

❌ **Cons:**
- Opinionated structure
- Less control over raw requests

#### Alternative A: **LangChain**

**When to choose:**
- Building complex AI agents
- Need chains and tools
- Want embeddings and vector stores
- Building RAG applications

**How to swap:**

1. **Install LangChain:**
   ```bash
   npm install langchain @langchain/openai
   ```

2. **Replace Vercel AI SDK:**
   ```typescript
   import { ChatOpenAI } from "@langchain/openai"
   
   const model = new ChatOpenAI({
     modelName: "gpt-4",
   })
   ```

3. **Update chat route:**
   - Replace `streamText` with LangChain streaming
   - Use LangChain message formats

**Claude can help:** Say "I want to use LangChain instead of Vercel AI SDK"

#### Alternative B: **Raw OpenAI SDK**

**When to choose:**
- Only need OpenAI
- Want full control
- Don't need multi-provider
- Simpler implementation

**How to swap:**

1. **Install OpenAI SDK:**
   ```bash
   npm install openai
   ```

2. **Replace in chat route:**
   ```typescript
   import OpenAI from 'openai'
   
   const openai = new OpenAI({
     apiKey: process.env.OPENAI_API_KEY,
   })
   
   const stream = await openai.chat.completions.create({
     model: "gpt-4",
     messages: messages,
     stream: true,
   })
   ```

**Claude can help:** Say "I want to use raw OpenAI SDK"

#### Alternative C: **Anthropic SDK (Claude)**

**When to choose:**
- Want to use Claude models
- Need longer context windows
- Prefer Anthropic's approach

**How to swap:**

1. **Install Anthropic SDK:**
   ```bash
   npm install @anthropic-ai/sdk
   ```

2. **Update API route:**
   ```typescript
   import Anthropic from '@anthropic-ai/sdk'
   
   const anthropic = new Anthropic({
     apiKey: process.env.ANTHROPIC_API_KEY,
   })
   ```

**Claude can help:** Say "I want to use Anthropic's Claude API"

---

### 4. UI Component Library

#### Default: shadcn/ui
✅ **Pros:**
- Copy-paste components (you own the code)
- Highly customizable
- Accessible
- No bundle size overhead

❌ **Cons:**
- Manual component installation
- Less components than full libraries

#### Alternative A: **Material UI (MUI)**

**When to choose:**
- Need comprehensive component set
- Want Material Design
- Need advanced data grids
- Enterprise application

**How to swap:**

1. **Install MUI:**
   ```bash
   npm install @mui/material @emotion/react @emotion/styled
   ```

2. **Replace components:**
   - Remove `src/components/ui/*`
   - Import from `@mui/material`

3. **Update theme:**
   - Create MUI theme
   - Wrap app with ThemeProvider

**Claude can help:** Say "I want to use Material UI instead of shadcn/ui"

#### Alternative B: **Chakra UI**

**When to choose:**
- Want simpler API
- Need dark mode built-in
- Prefer component props over CSS
- Fast development

**How to swap:**

1. **Install Chakra UI:**
   ```bash
   npm install @chakra-ui/react @emotion/react @emotion/styled framer-motion
   ```

2. **Setup provider:**
   ```typescript
   import { ChakraProvider } from '@chakra-ui/react'
   ```

**Claude can help:** Say "I want to use Chakra UI"

#### Alternative C: **Headless UI + Custom Design**

**When to choose:**
- Building custom design system
- Want full control
- Need accessibility primitives
- Don't want pre-styled components

**How to swap:**

1. **Install Headless UI:**
   ```bash
   npm install @headlessui/react
   ```

2. **Build custom components:**
   - Use Headless UI primitives
   - Style with Tailwind
   - Full design control

**Claude can help:** Say "I want to use Headless UI with custom styling"

---

### 5. Styling Approach

#### Default: Tailwind CSS v4
✅ **Pros:**
- Utility-first (fast development)
- Small bundle size
- Great DX with autocomplete
- Modern and popular

❌ **Cons:**
- HTML can get verbose
- Learning curve for utilities

#### Alternative A: **CSS Modules**

**When to choose:**
- Prefer traditional CSS
- Want scoped styles
- Don't like utility classes

**How to swap:**

1. **Already supported in Next.js**
2. **Create `.module.css` files**
3. **Remove Tailwind:**
   ```bash
   npm uninstall tailwindcss
   ```

**Claude can help:** Say "I want to use CSS Modules instead of Tailwind"

#### Alternative B: **Styled Components / Emotion**

**When to choose:**
- Want CSS-in-JS
- Need dynamic styling
- Prefer component-scoped styles

**How to swap:**

1. **Install styled-components:**
   ```bash
   npm install styled-components
   ```

2. **Setup Next.js config for styled-components**

**Claude can help:** Say "I want to use styled-components"

---

### 6. Deployment Platform

#### Default: Vercel
✅ **Pros:**
- Zero-config for Next.js
- Automatic preview deployments
- Edge network
- Great DX

❌ **Cons:**
- Vendor lock-in
- Costs for high traffic

#### Alternative A: **Netlify**

**When to choose:**
- Want similar DX to Vercel
- Need Netlify-specific features
- Prefer Netlify pricing

**How to swap:**

1. **Create `netlify.toml`:**
   ```toml
   [build]
     command = "npm run build"
     publish = ".next"
   ```

2. **Deploy to Netlify**

**Claude can help:** Say "I want to deploy to Netlify instead of Vercel"

#### Alternative B: **Self-Hosted (Docker)**

**When to choose:**
- Want full control
- Have existing infrastructure
- Need to meet compliance requirements

**How to swap:**

1. **Create Dockerfile**
2. **Set up reverse proxy (nginx)**
3. **Configure environment variables**
4. **Set up CI/CD pipeline**

**Claude can help:** Say "I want to self-host with Docker"

#### Alternative C: **AWS Amplify / Azure / GCP**

**When to choose:**
- Already using cloud provider
- Need integration with other services
- Enterprise requirements

**Claude can help:** Say "I want to deploy to AWS/Azure/GCP"

---

## 🎯 When to Swap Components

### Before Building Features
**Best time to swap** - Clean slate, no existing code to migrate.

### After Prototype
**Harder but possible** - Some code to refactor, but doable with Claude's help.

### In Production
**Most difficult** - Requires careful migration strategy, but Claude can guide you.

---

## 💡 How Claude Can Help You Swap

### Just Ask!

```
"I want to use [ALTERNATIVE] instead of [DEFAULT]"
```

**Examples:**
- "I want to use Supabase instead of Better Auth"
- "I want to use Prisma instead of Drizzle"
- "I want to use Chakra UI instead of shadcn/ui"
- "I want to use MongoDB instead of PostgreSQL"

### Claude Will:

1. ✅ **Verify it's a good fit** for your use case
2. ✅ **Guide installation** step-by-step
3. ✅ **Help remove old code** (Better Auth, Drizzle, etc.)
4. ✅ **Install new dependencies**
5. ✅ **Update configuration files**
6. ✅ **Migrate existing code** to new library
7. ✅ **Test both local and production** environments
8. ✅ **Update documentation** for future reference

### Claude Won't:
❌ Swap without asking why  
❌ Swap if it's not a good fit for your use case  
❌ Break your existing working code  

---

## 📊 Decision Matrix

### Choose Supabase If:
- [ ] Want managed database + auth + storage all-in-one
- [ ] Need real-time subscriptions
- [ ] Want minimal backend setup
- [ ] Okay with vendor lock-in

### Choose Better Auth (Default) If:
- [ ] Want self-hosted solution
- [ ] Need flexibility and control
- [ ] Want to avoid vendor lock-in
- [ ] Have time for more setup

### Choose Clerk If:
- [ ] Want fastest auth setup
- [ ] Need pre-built UI components
- [ ] Want organizations/teams built-in
- [ ] Budget allows for SaaS pricing

### Choose Prisma If:
- [ ] Want better tooling (Prisma Studio)
- [ ] Prefer generated types
- [ ] Like visual schema management
- [ ] Want larger community

### Choose Drizzle (Default) If:
- [ ] Want lightweight ORM
- [ ] Prefer writing SQL
- [ ] Need full control over queries
- [ ] Want better performance

---

## ⚠️ Important Notes

### Before Swapping:

1. **Understand the trade-offs** - Every choice has pros/cons
2. **Check pricing** - Some alternatives have usage costs
3. **Consider vendor lock-in** - Can you migrate away later?
4. **Verify compatibility** - Does it work with your other choices?
5. **Ask Claude first** - Get guidance before making changes

### After Swapping:

1. **Test thoroughly** - Local and production environments
2. **Update documentation** - Document your stack choices
3. **Check dependencies** - Remove unused packages
4. **Update README** - Note what you changed from default

---

## 🚀 Quick Swap Checklist

When swapping any component:

- [ ] Discussed with Claude (get recommendation)
- [ ] Understand why you're swapping
- [ ] Have accounts/API keys for new service (if needed)
- [ ] Backup current working code
- [ ] Install new dependencies
- [ ] Update environment variables
- [ ] Remove old dependencies
- [ ] Update configuration files
- [ ] Migrate existing code
- [ ] Test locally
- [ ] Test on production
- [ ] Update documentation
- [ ] Commit changes

---

## 📚 Resources

**Authentication:**
- Supabase Auth: https://supabase.com/docs/guides/auth
- Clerk: https://clerk.com/docs
- Auth.js: https://authjs.dev/
- Better Auth: https://better-auth.com/

**Database & ORM:**
- Prisma: https://www.prisma.io/docs
- Supabase: https://supabase.com/docs
- Drizzle: https://orm.drizzle.team/

**AI Integration:**
- LangChain: https://js.langchain.com/
- OpenAI: https://platform.openai.com/docs
- Anthropic: https://docs.anthropic.com/

**UI Libraries:**
- Material UI: https://mui.com/
- Chakra UI: https://chakra-ui.com/
- Headless UI: https://headlessui.com/

---

**Remember:** The default stack is carefully chosen for a good balance of features, DX, and cost. Only swap if you have a specific reason!

**Need help deciding?** Ask Claude: "What stack should I use for [your use case]?"
