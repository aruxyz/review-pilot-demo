<div align="center">

# ReviewPilot Demo

A minimal Next.js 14 app with **5 intentional UX bugs** for [ReviewPilot](https://github.com/aruxyz/reviewpilot-browser-use) to detect.

ReviewPilot is an AI browser QA tool that opens your deployed preview, explores your app like a real user, and posts a human-readable review comment with screenshots and suggested fixes.

[Deploy to Vercel](#deploy-to-vercel) | [Run ReviewPilot against this demo](#run-reviewpilot-against-this-demo) | [The 5 Bugs](#the-5-intentional-bugs)

</div>

---

## What This Repo Is

This is the **demo target** for ReviewPilot. It is a small Next.js app with five planted UX bugs. When you deploy this repo to Vercel and run [ReviewPilot](https://github.com/aruxyz/reviewpilot-browser-use) against the deploy URL, ReviewPilot should detect several of these bugs and produce a report with screenshots.

> **Do NOT fix these bugs.** They are planted on purpose. Every bug is marked with a `BUG #N (intentional)` comment in the source.

---

## Deploy to Vercel

### Option A: One-click deploy

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/aruxyz/review-pilot-demo)

This clones the repo to your GitHub account and deploys it to Vercel in one step. You get a production URL like `https://review-pilot-demo.vercel.app`.

### Option B: Vercel CLI

```bash
git clone https://github.com/aruxyz/review-pilot-demo.git
cd review-pilot-demo
npm install
vercel --prod
```

### Option C: Vercel Dashboard

1. Open [vercel.com/new](https://vercel.com/new)
2. Import this repo
3. Framework preset: Next.js (auto-detected)
4. Deploy

After deploy, copy the production URL. You will use it in the next step.

---

## Run ReviewPilot Against This Demo

Install ReviewPilot on your machine:

```bash
pip install reviewpilot-browser-use
playwright install chromium
```

Set an LLM API key:

```bash
export OPENAI_API_KEY=sk-...
```

Run ReviewPilot against your deployed demo URL:

```bash
reviewpilot run --url https://YOUR-DEMO-URL.vercel.app
```

Open the report:

```bash
reviewpilot report
```

You should see findings for several of the five bugs below, complete with screenshots, reproduction steps, and suggested fixes.

---

## Run Locally

```bash
git clone https://github.com/aruxyz/review-pilot-demo.git
cd review-pilot-demo
npm install
npm run dev      # starts on http://localhost:3000
```

Or build and run the production bundle:

```bash
npm install
npm run build
npm run start    # starts on http://localhost:3000
```

Requires Node 18 or newer (developed and verified on Node 22).

---

## Pages

| Route | Description |
|---|---|
| `/` | Homepage: hero section, pricing CTA, footer |
| `/login` | Login form (broken) |
| `/pricing` | Pricing tiers with a "Get started" CTA |
| `/_not-found` | Next.js default 404 page, triggered by the broken footer link |

---

## The 5 Intentional Bugs

### Bug 1: Mobile navbar overflow

- **File:** `components/Navbar.tsx`
- **Symptom:** On viewports 390px or narrower the navbar links overflow horizontally. There is no hamburger menu and no responsive collapse. The links use a single `flex` row with `whitespace-nowrap`, so they get cut off and cause horizontal page scroll.
- **How to detect:** Set viewport to 390x844 or smaller. Observe horizontal scroll or clipped nav links.

### Bug 2: Login button stays disabled

- **File:** `app/login/page.tsx`
- **Symptom:** The "Sign In" button is hardcoded `disabled={true}` and never becomes enabled, even when both the email and password fields are filled. The disabled state is not wired to form validation.
- **How to detect:** Open `/login`, type into both fields, and observe the button remains greyed out and non-clickable.

### Bug 3: Pricing CTA hidden below fold on mobile

- **File:** `app/page.tsx`
- **Symptom:** The homepage hero section uses `py-96` (384px) vertical padding on mobile, pushing the "View Pricing Plans" CTA below the fold on viewports 390px or narrower. The CTA is present in the DOM but requires scrolling to reach. On desktop the padding shrinks to `py-16` so the CTA is visible.
- **How to detect:** Load `/` at 390x844 and take a screenshot of the initial viewport. The CTA is not visible. Scroll down to find it.

### Bug 4: Broken footer link (404)

- **File:** `components/Footer.tsx`
- **Symptom:** The "Privacy Policy" and "Terms of Service" footer links point to `/nonexistent-page`, a route that does not exist. Clicking either link returns a 404.
- **How to detect:** Click either footer link and observe the Next.js 404 page.

### Bug 5: Unclear form validation

- **File:** `app/login/page.tsx`
- **Symptom:** The login form shows a generic error banner ("Error 001: Invalid input") with no field-level indicators. There is no red border, no `aria-invalid`, and no helper text under the inputs. The user cannot tell which field is wrong or what the problem is.
- **How to detect:** Open `/login`, type into either field, and observe the generic error with no per-field cues. The inputs keep their neutral grey border.

---

## File Tree

```
review-pilot-demo/
├── README.md
├── package.json
├── tsconfig.json
├── next.config.mjs
├── postcss.config.mjs
├── tailwind.config.ts
├── next-env.d.ts
├── .gitignore
├── app/
│   ├── globals.css
│   ├── layout.tsx
│   ├── page.tsx              # Bug 3
│   ├── login/
│   │   └── page.tsx          # Bugs 2 + 5
│   └── pricing/
│       └── page.tsx
└── components/
    ├── Navbar.tsx            # Bug 1
    └── Footer.tsx            # Bug 4
```

---

## Tech Stack

- Next.js 14 (App Router)
- React 18
- TypeScript 5
- Tailwind CSS 3

---

## Notes

- There is no backend, database, or real authentication. All pages are static.
- `/nonexistent-page` is intentionally not implemented so that the footer links produce a real 404 (Next.js default 404 page).
- The app is deliberately plain-looking. It is a bug-demo app, not a portfolio piece.

---

## Related

- **ReviewPilot (the tool):** [aruxyz/reviewpilot-browser-use](https://github.com/aruxyz/reviewpilot-browser-use)
- **Browser Use (the engine):** [browser-use/browser-use](https://github.com/browser-use/browser-use)

---

## License

MIT


## Contributing

Contributions welcome. Please open an issue first to discuss what you would like to change.
