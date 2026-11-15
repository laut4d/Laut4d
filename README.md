# NEXPLAY — Full Starter Project (Vercel + Supabase)

Starter platform: landing page, signup/login, player dashboard, admin listing, game catalog, virtual wallet schema, responsive dark/gold UI.

> This is a functional starter foundation, not a finished commercial game platform. Game engines, moderation workflows, production-grade audit controls, and full admin CRUD are not included yet. Wallet is virtual/demo only; no cash deposit, withdrawal, or gambling functionality.

## 1. Supabase setup
1. Create a Supabase project.
2. Open **SQL Editor** and run all of `supabase/schema.sql`.
3. In Authentication → URL Configuration, add your local URL `http://localhost:3000` and your Vercel domain (e.g. `https://your-project.vercel.app`) to Site URL / Redirect URLs as appropriate.
4. Copy Project URL and Publishable key (not service-role secret).

## 2. Environment
Copy `.env.example` to `.env.local` and fill:
- `NEXT_PUBLIC_SUPABASE_URL`
- `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY`

Do not put service-role keys in client environment or Git.

## 3. Run locally (Termux or desktop)
```bash
npm install
npm run dev:webpack
```
Open `http://localhost:3000`.

## 4. Deploy to Vercel
### Recommended (Git integration)
1. Push project folder to a GitHub repository (exclude `.env.local`).
2. In Vercel, Import Git Repository.
3. Add the two Supabase environment variables in Project → Settings → Environment Variables.
4. Deploy. Add deployed URL to Supabase Auth redirect/site configuration.

### Vercel CLI
```bash
npm install
npm install -g vercel
vercel login
vercel
vercel --prod
```
Set environment variables in Vercel dashboard or CLI before testing auth.

## Routes
- `/` landing/catalog
- `/register` signup
- `/login` login
- `/dashboard` player area (client redirects unauthenticated users)
- `/admin` admin-only UI check (requires profile role `admin`)

## Promote an admin
After registering a trusted account, run in Supabase SQL Editor:
```sql
update public.profiles set role='admin' where username='YOUR_USERNAME';
```
Never expose role changes to public clients.

## Security notes / production checklist
- RLS is enabled for user/profile/wallet/transactions tables.
- Admin page is a basic UI check. For privileged mutations, implement server-side authorization (server actions/API) and stronger policies before production.
- Profile SELECT currently permits authenticated users to see profile records. Restrict columns/rows if usernames or profile metadata should be private.
- No secrets in repository; `.env.local` ignored.
- No real-money balance: virtual XP only.
- Add rate limiting, abuse prevention, email verification, password reset, logs/monitoring, and tests before a public production launch.
