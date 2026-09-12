# Next.js Interview Questions — Basic to Expert

## Basic Level

**1. What is Next.js?**

Next.js is a React framework built on top of React that adds features like routing, server-side rendering (SSR), static site generation (SSG), and API routes out of the box. It's built by Vercel.

**2. Why would you use Next.js instead of React alone?**

- React alone is just a UI library — it doesn't include routing, SSR, or build tooling.
- Next.js gives you routing, SEO-friendly rendering (SSR/SSG), image optimization, API routes, and performance optimizations without manually configuring Webpack/Babel.
- Better SEO since pages can be pre-rendered on the server (plain React apps are client-rendered by default, which is bad for SEO).

**3. What are the main features of Next.js?**

- File-system based routing
- Server-Side Rendering (SSR)
- Static Site Generation (SSG)
- Incremental Static Regeneration (ISR)
- API routes (backend endpoints inside the same project)
- Image optimization (`next/image`)
- Built-in CSS/Sass support
- Fast Refresh
- Middleware support
- App Router with React Server Components (Next.js 13+)

**4. How is Next.js different from React?**

| React | Next.js |
|---|---|
| Library for building UI | Full framework built on React |
| No built-in routing | File-system based routing included |
| Client-side rendering by default | Supports SSR, SSG, ISR, CSR |
| Needs manual setup (Webpack, Babel, routing library) | Comes pre-configured |
| No built-in SEO advantage | SEO-friendly due to server rendering |

**5. What is file-system based routing in Next.js?**

Instead of manually defining routes (like with `react-router`), Next.js automatically creates routes based on the file/folder structure inside the `pages` (or `app`) directory.

You just create a file, and Next.js turns it into a route automatically — no extra routing config needed.

**6. What is the difference between the App Router and Pages Router?**

| Pages Router (old) | App Router (new, Next.js 13+) |
|---|---|
| Uses `pages/` directory | Uses `app/` directory |
| No built-in support for Server Components | Built on React Server Components (RSC) by default |
| Data fetching via `getServerSideProps`, `getStaticProps` | Data fetching directly in components using `async/await` |
| Layouts done manually with `_app.js` / `_document.js` | Built-in nested `layout.js` support |
| File = page (`about.js` → `/about`) | Folder + `page.js` = route (`about/page.js` → `/about`) |
| Client-rendered by default (unless configured) | Server Components by default (more performant) |

**7. What is the purpose of the `app` directory?**

The `app` directory is used by the App Router (Next.js 13+). Each folder inside it represents a route, and a `page.js` file inside that folder makes it a page. It also supports:

- Nested `layout.js` files (shared UI across routes)
- `loading.js` (loading UI)
- `error.js` (error boundaries)
- Server Components by default
- Co-located route-level logic (route handlers, metadata, etc.)

**8. What is the purpose of the `pages` directory?**

The `pages` directory is used by the Pages Router (the older, original routing system). Every file inside it automatically becomes a route based on its file name.

It also supports `getStaticProps`, `getServerSideProps`, and `getInitialProps` for data fetching, and `_app.js` / `_document.js` for global layout/config.

**9. What is a Server Component in Next.js?**

A Server Component is a component that renders only on the server — its code never gets sent to the browser as JS. Benefits:

- Smaller client-side bundle size
- Can directly access backend resources (database, file system, secrets) without an API layer
- Faster initial page load

**10. What is a Client Component in Next.js?**

A Client Component runs in the browser and supports interactivity — things like `useState`, `useEffect`, event handlers (`onClick`, etc.). You opt into it by adding `"use client"` at the top of the file.

**11. What does the `"use client"` directive do?**

It marks a file (and everything imported into it) as a Client Component, telling Next.js to send that component's JS to the browser and hydrate it there instead of rendering it only on the server.

**12. When should you use a Client Component?**

Use it when you need:

- Interactivity — `onClick`, `onChange`, form handling
- React hooks — `useState`, `useEffect`, `useContext`, `useReducer`
- Browser-only APIs — `window`, `localStorage`, `navigator`
- Third-party libraries that rely on hooks/browser APIs (e.g., some charting or animation libs)

**13. What is SSG in Next.js?**

Trick: Same Server Component, bas `cache: "force-cache"` (ya kuch mat likho — ye default hai).

```
npm run build → HTML ban gaya → Site live ho gayi
```

Ab agar tumhara data (jaise product ka price) database me change ho jaye, to bhi website pe purana hi price dikhega — kyunki HTML file ek baar hi banti hai. Jab tak tum khud dobara `npm run build` + deploy nahi karoge, wo update nahi hoga.

Matlab: Update karne ke liye tumhe manually dobara build+deploy karna padega.

**14. What is CSR in Next.js?**

Trick: `"use client"` lagao + `useEffect`/`useState` se data fetch karo.

Kya hota hai:

- Server sirf ek khaali sa HTML bhejta hai (jaise ek khaali dabba) + ek bada JavaScript file. Fir browser us JS ko chalake khud poora page banata hai.

Beneficial kab hai:

- Jab page bahut interactive ho (dashboard, admin panel — jaha SEO ki zaroorat nahi)
- Server ka load kam hota hai kyunki wo sirf JS bhejta hai, kuch banata nahi
- Nuksan: bad SEO

**15. What is SSR in Next.js?**

Trick: Server Component (default) me fetch karo with `cache: "no-store"`.

Kya hota hai:

- Har baar jab user page maangta hai, server turant us waqt HTML bana ke deta hai — poora bhara hua, ready-made.
- Flow: User request karta hai → Server data fetch karta hai + HTML banata hai (usi waqt) → Ready HTML browser ko milta hai

Beneficial kab hai:

- Jab data baar-baar change hota ho (user-specific data, live dashboard)
- SEO chahiye + fresh data bhi chahiye

**16. What is ISR in Next.js?**

Yahi SSG jaisa hai, bas ek cheez extra add karte ho: `revalidate: 60` jaisa time.

```jsx
fetch("...", { next: { revalidate: 60 } })
```

Iska matlab: "Ye page 60 second ke liye static rahega. Uske baad, agar koi ise request kare, to Next.js background me khud naya HTML bana lega — bina mujhe (developer) dobara deploy kiye."

Trick: SSG jaisa hi, bas revalidate time add karo.

```jsx
// app/products/page.js

async function getProducts() {
  const res = await fetch("https://api.example.com/products", {
    next: { revalidate: 60 }, // 60 seconds baad background me refresh hoga
  });
  return res.json();
}
```

**17. What is pre-rendering in Next.js?**

Pre-rendering ka matlab hai — Next.js page ka HTML pehle se (server pe, build time ya request time pe) bana leta hai, instead of sab kuch browser pe chhod dena (jaise plain React/CSR karta hai).

- SSG aur SSR dono pre-rendering ke types hain
- Fayda: user ko content turant dikh jata hai, aur search engines ko bhi complete HTML milta hai (SEO achha hota hai)
- CSR isse alag hai — usme kuch bhi pre-rendered nahi hota, browser khud sab kuch banata hai JS chalake

**18. What is hydration?**

Hydration wo process hai jisme React, server se aayi static HTML ke upar apna JavaScript "chadha" ke use interactive banata hai.

- HTML dikhne me complete lagti hai, par click/interaction abhi kaam nahi karta
- Browser JS load karta hai background me
- Hydration hone ke baad event listeners (`onClick`, `onChange`) attach ho jate hain, page fully interactive ho jata hai

**19. What is a dynamic route in Next.js?**

Dynamic route wo route hai jiska URL fix nahi hota — usme ek variable part hota hai (jaise ID, slug), jo square brackets `[ ]` se define hota hai file/folder naming me.

**20. How do you create a dynamic route in the App Router?**

Folder ka naam square brackets `[ ]` me rakho, aur uske andar `page.tsx` banao:

```
app/
  products/
    [id]/
      page.tsx    → matches "/products/1", "/products/99", etc.
```

```jsx
// app/products/[id]/page.tsx
export default function ProductPage({ params }) {
  return <h1>Product ID: {params.id}</h1>;
}
```

`params.id` automatically URL se mil jata hai — agar user `/products/42` pe jaye, to `params.id = "42"`.

**21. What are route parameters in Next.js?**

Route parameters wo dynamic values hain jo URL ke path ka hi hissa hote hain — square brackets `[ ]` se define karte hain folder/file naming me.

```jsx
// app/blog/[id]/page.js
export default function BlogPost({ params }) {
  return <h1>Post ID: {params.id}</h1>;
}
```

Agar URL `/blog/5` hai, to `params.id` = `"5"`.

**22. What are query/search parameters?**

Query parameters wo values hain jo URL me `?` ke baad aate hain (`key=value` format me) — path ka hissa nahi hote.

```
/products?category=shoes&sort=price
```

Component me `searchParams` prop se access karte ho:

```jsx
// app/products/page.js
export default function ProductsPage({ searchParams }) {
  const category = searchParams.category; // "shoes"
  const sort = searchParams.sort;         // "price"

  return (
    <div>
      <p>Category: {category}</p>
      <p>Sort by: {sort}</p>
    </div>
  );
}
```

**23. What is `Link` in Next.js?**

`Link` ek built-in Next.js component hai (`next/link` se import hota hai) jo pages ke beech navigation karne ke liye use hota hai — normal HTML ke `<a>` tag ki jagah.

```jsx
import Link from "next/link";

export default function Navbar() {
  return (
    <nav>
      <Link href="/">Home</Link>
      <Link href="/about">About</Link>
      <Link href="/blog/5">Blog Post 5</Link>
    </nav>
  );
}
```

**24. Why should you use `next/link` instead of a normal `<a>` tag?**

| `<a>` tag (normal HTML) | `next/link` |
|---|---|
| Poora page reload hota hai | Client-side navigation — page reload nahi hota |
| Poora HTML+CSS+JS dobara download hota hai | Sirf zaroori data fetch hota hai, JS already loaded rehta hai |
| Slow | Fast (SPA jaisa feel) |
| Koi prefetching nahi | Automatic prefetching — link jab screen pe visible hota hai, uska data pehle se background me load ho jata hai |

**25. What is `next/image`?**

Ye Next.js ka built-in `Image` component hai jo normal HTML `<img>` tag ki jagah use karte ho — automatic image optimization ke saath.

```jsx
import Image from "next/image";

export default function Profile() {
  return (
    <Image
      src="/profile.jpg"
      alt="Profile picture"
      width={200}
      height={200}
    />
  );
}
```

**26. What are the benefits of using `next/image`?**

1. **Automatic resizing (responsive images)** — Alag-alag device (mobile, tablet, desktop) ke liye alag size ki image serve karta hai — bade screen ko badi image, chhote screen (mobile) ko chhoti image.
2. **Modern format conversion** — Purane `.jpg`/`.png` ko automatically WebP (ya AVIF) format me convert karke deta hai — jo same quality me 30-50% chhota hota hai.
3. **Lazy loading (by default)** — Jo images screen pe abhi visible nahi hain (neeche scroll karne pe aayengi), wo turant load nahi hoti — jab user scroll karke unke paas pahunchta hai tabhi load hoti hain.
4. **Priority loading** — Agar koi image turant (above the fold) dikhni chahiye, to `priority` prop se lazy loading skip kar sakte ho.
5. **Layout shift prevent karta hai** — `width` aur `height` pehle se define hone ki wajah se browser pehle se jagah reserve kar leta hai, isliye CLS (Cumulative Layout Shift) kam hota hai.

**27. What is `next/font`?**

Ye Next.js ka built-in font optimization system hai — Google Fonts ya local fonts ko optimize tarike se load karta hai, taaki performance aur layout dono achhe rahein.

```jsx
import { Inter } from "next/font/google";

const inter = Inter({ subsets: ["latin"] });

export default function Layout({ children }) {
  return (
    <html lang="en" className={inter.className}>
      <body>{children}</body>
    </html>
  );
}
```

**28. How does `next/font` optimize fonts?**

1. **Self-hosting automatic** — Normally Google Fonts use karne ke liye tumhe `<link>` tag se Google ke server se font fetch karna padta hai, jisse extra network request Google ke server ko jaani padti hai. `next/font` build time pe font file ko download karke tumhare khud ke server pe host kar deta hai — Google ko koi extra request nahi jaati.
2. **Zero layout shift (CLS prevent)** — Font load hone se pehle browser fallback font dikhata hai, aur jab actual font load hoti hai to text size/spacing badal jata hai — isse layout "jump" karta hai. `next/font` ye automatically prevent karta hai size-matching ke through.
3. **No extra network request during runtime** — Chunki font already build time pe optimize ho ke self-hosted hai, browser ko turant mil jati hai.

**29. How do you create a layout in the App Router?**

`app` directory ke andar kisi bhi folder me `layout.js`/`layout.tsx` file banao. Ye us folder aur uske andar ke saare routes ke liye shared UI wrap kar deta hai.

```jsx
// app/layout.js — Root layout (poori app ke liye zaroori)
export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>
        <header>My Website Header</header>
        {children} {/* yaha page ka content aayega */}
        <footer>My Website Footer</footer>
      </body>
    </html>
  );
}
```

**30. What is the purpose of `layout.tsx`?**

`layout.tsx` shared UI define karta hai jo multiple pages ke beech common rehti hai (header, footer, sidebar, navigation) — aur navigation ke time re-render nahi hota, sirf `children` (page content) badalta hai.

Key points:

- Root `layout.tsx` mandatory hai (`app/layout.tsx`) — isme `<html>` aur `<body>` tags hone chahiye
- Nested layouts optional hain, jitni chaho utni levels me bana sakte ho
- Layout state preserve rakhta hai navigation ke beech (jaise agar sidebar me kuch expand kiya hai, wo collapse nahi hoga page change karne pe)

**31. What is `page.tsx`?**

`page.tsx` wo file hai jo kisi folder ko actual route/URL banati hai — bina is file ke, folder sirf ek normal folder hai, route nahi banta.

```jsx
// app/about/page.tsx
export default function AboutPage() {
  return <h1>About Us</h1>;
}
```

Agar `page.tsx` nahi hai, to wo route accessible hi nahi hoga (404 aayega), chahe folder me aur files ho (jaise sirf `layout.tsx`).

**32. What is `loading.tsx`?**

`loading.tsx` ek automatic loading UI hai jo Next.js khud dikhata hai jab tak us route ka `page.tsx` (aur uska data) load ho raha hota hai — bina tumhe manually `useState`/`isLoading` likhe.

```jsx
// app/dashboard/loading.tsx
export default function Loading() {
  return <p>Loading dashboard...</p>;
}
```

Kaise kaam karta hai: Ye React `Suspense` ka use karta hai internally. Jab tak `app/dashboard/page.tsx` ka data (async fetch) resolve nahi hota, `loading.tsx` dikhta rehta hai — resolve hote hi actual page dikh jata hai.

**33. What is `error.tsx`?**

`error.tsx` ek error boundary hai — agar us route ke andar (`page.tsx` ya kisi child component) me koi runtime error aati hai, to poori app crash hone ke bajaye sirf `error.tsx` wala fallback UI dikhta hai.

```jsx
// app/dashboard/error.tsx
"use client"; // error.tsx hamesha Client Component hona chahiye

export default function Error({ error, reset }) {
  return (
    <div>
      <h2>Something went wrong!</h2>
      <p>{error.message}</p>
      <button onClick={() => reset()}>Try again</button>
    </div>
  );
}
```

**34. What is `not-found.tsx`?**

`not-found.tsx` custom 404 page dikhane ke liye use hota hai — jab koi route exist nahi karta, ya tum manually `notFound()` function call karte ho (jaise agar database me record nahi mila).

```jsx
// app/not-found.tsx
export default function NotFound() {
  return (
    <div>
      <h2>404 - Page Not Found</h2>
      <p>Ye page exist nahi karta.</p>
    </div>
  );
}
```

**35. What is `route.ts`?**

`route.ts` (App Router me API routes banane ka tarika) — kisi folder me ye file dalne se wo folder ek backend API endpoint ban jata hai (page nahi banta).

```ts
// app/api/users/route.ts
import { NextResponse } from "next/server";

export async function GET() {
  const users = [{ id: 1, name: "Mayur" }, { id: 2, name: "Rahul" }];
  return NextResponse.json(users);
}

export async function POST(request) {
  const body = await request.json();
  // database me save karo
  return NextResponse.json({ message: "User created", data: body });
}
```

**36. What are route handlers in Next.js?**

Route handlers wo functions hain jo App Router me API endpoints banate hain — `route.ts`/`route.js` file me define karte ho, aur ye standard HTTP methods (`GET`, `POST`, `PUT`, `DELETE`, etc.) ke functions export karte hain. Ye Pages Router ke `pages/api/` ka App Router replacement hain.

```ts
// app/api/products/route.ts
import { NextResponse } from "next/server";

export async function GET() {
  const products = [{ id: 1, name: "Shirt" }, { id: 2, name: "Shoes" }];
  return NextResponse.json(products);
}
```

Ye function ek server pe chalne wala backend code hai — browser ko sirf response milta hai, function ka code nahi.

**37. How do you create an API endpoint in the App Router?**

`app` directory ke andar kisi bhi folder me `route.ts` file banao, aur usme HTTP method ke naam se function export karo.

```ts
// app/api/users/route.ts
import { NextResponse } from "next/server";

// GET /api/users
export async function GET() {
  const users = [{ id: 1, name: "Mayur" }];
  return NextResponse.json(users);
}

// POST /api/users
export async function POST(request: Request) {
  const body = await request.json();
  return NextResponse.json({ message: "Created", data: body }, { status: 201 });
}
```

Dynamic API route (jaise `/api/users/5`):

```ts
// app/api/users/[id]/route.ts
import { NextResponse } from "next/server";

export async function GET(request: Request, { params }: { params: { id: string } }) {
  return NextResponse.json({ id: params.id, name: "Mayur" });
}
```

**38. How do environment variables work in Next.js?**

Environment variables secret ya config values (API keys, database URLs) store karne ke liye use hoti hain, bina unhe code me hardcode kiye. Ye `.env.local` (ya `.env`, `.env.production`) file me define karte ho.

```
# .env.local
DATABASE_URL=mongodb://localhost:27017/mydb
API_SECRET_KEY=abc123xyz
NEXT_PUBLIC_API_URL=https://api.example.com
```

Important rules:

- `.env.local` file kabhi Git me commit nahi karni chahiye (`.gitignore` me hona chahiye)
- Env variable change karne ke baad server restart karna padta hai (`npm run dev` dobara)

**39. What is the difference between server-only and client-exposed environment variables?**

| Server-only variable | Client-exposed variable (`NEXT_PUBLIC_`) |
|---|---|
| Sirf server pe accessible (Server Components, Route Handlers) | Server aur browser dono me accessible |
| `process.env.SECRET_KEY` | `process.env.NEXT_PUBLIC_API_URL` |
| Browser me kabhi expose nahi hota, JS bundle me nahi jata | Browser ke JS bundle me chala jata hai (koi bhi dekh sakta hai) |
| Database URL, API secret keys, private tokens ke liye | Public API URL, analytics ID, jo values secret nahi hain |

**40. What is `NEXT_PUBLIC_` used for?**

`NEXT_PUBLIC_` ek special prefix hai jo Next.js ko batata hai — "is variable ko browser ke JS bundle me bhi include karo, taaki Client Components me bhi ye accessible ho jaye."

Normally, `.env` file ki saari variables sirf server pe accessible hoti hain (security ke liye). Par kabhi-kabhi tumhe kuch values browser me bhi chahiye hoti hain (jaise public API URL) — tab tum `NEXT_PUBLIC_` prefix lagate ho.

```
# .env.local
NEXT_PUBLIC_API_URL=https://api.example.com
NEXT_PUBLIC_GOOGLE_ANALYTICS_ID=G-XXXXXXX
```

Kab use karo `NEXT_PUBLIC_`:

- Public API URLs (jo already public hain)
- Analytics/tracking IDs (Google Analytics, etc.)
- Firebase config (public keys jo client SDK ke liye zaroori hain)

Kab NAHI use karo:

- Database credentials
- API secret keys, private tokens
- Payment gateway secret keys
