# Next.js Interview Questions — Basic to Expert

## Basic Level

1. What is Next.js?
2. Why would you use Next.js instead of React alone?
3. What are the main features of Next.js?
4. How is Next.js different from React?
5. What is file-system based routing in Next.js?
6. What is the difference between the App Router and Pages Router?
7. What is the purpose of the `app` directory?
8. What is the purpose of the `pages` directory?
9. What is a Server Component in Next.js?
10. What is a Client Component in Next.js?
11. What does the `"use client"` directive do?
12. When should you use a Client Component?
13. What is SSR in Next.js?
14. What is CSR in Next.js?
15. What is SSG in Next.js?
16. What is ISR in Next.js?
17. What is pre-rendering in Next.js?
18. What is hydration?
19. What is the difference between server-side rendering and client-side rendering?
20. What is a dynamic route in Next.js?
21. How do you create a dynamic route in the App Router?
22. What are route parameters in Next.js?
23. What are query/search parameters?
24. What is `Link` in Next.js?
25. Why should you use `next/link` instead of a normal `<a>` tag?
26. What is `next/image`?
27. What are the benefits of using `next/image`?
28. What is `next/font`?
29. How do you create a layout in the App Router?
30. What is the purpose of `layout.tsx`?
31. What is `page.tsx`?
32. What is `loading.tsx`?
33. What is `error.tsx`?
34. What is `not-found.tsx`?
35. What is `route.ts`?
36. What are route handlers in Next.js?
37. How do you create an API endpoint in the App Router?
38. How do environment variables work in Next.js?
39. What is the difference between server-only and client-exposed environment variables?
40. What is `NEXT_PUBLIC_` used for?

## Intermediate Level

41. Explain the rendering lifecycle of a Next.js App Router application.
42. How does the App Router decide whether a component runs on the server or client?
43. Can a Server Component import a Client Component?

A Server Component can import a Client Component.
Why this is allowed

Server Components run on the server.
When a Server Component imports a Client Component, Next.js:
Keeps the Server Component on the server.
Creates a client-side JavaScript boundary for the Client Component.
Sends the Client Component’s code to the browser so it can become interactive.

The Server Component itself never runs in the browser — only the Client Component does.

44. Can a Client Component import a Server Component?

A Client Component cannot import a Server Component.
Why not?

Client Components are bundled and run in the browser.
Server Components are meant to stay on the server (they can use server-only APIs, database access, secrets, etc.).
If a Client Component imported a Server Component, Next.js would try to include that server-only code in the client bundle → this is not allowed and will throw an error.

Instead of importing the Server Component, you pass it as a child (or as a prop) from a parent Server Component:

export default function Page() {
return (
<ClientWrapper>
<ServerComponent />
</ClientWrapper>
)
}
You are not giving the Client Component the actual Server Component code.
You are giving it the already-rendered result of that Server Component.

45. Why can't Server Components use browser-only APIs?

Because Server Components run on the server, not in the browser.
Simple explanation
Server Components execute in a Node.js (or Edge) environment on the server.

In that environment there is:

No window
No document
No localStorage / sessionStorage
No navigator
No browser events (click, scroll, etc.)

These APIs only exist in the browser. If a Server Component tried to use them, the code would crash on the server before any HTML is even sent to the client.

46. Why can't Server Components directly use hooks such as `useState` and `useEffect`?

You cannot use React hooks (useState, useEffect, useRef, useContext, etc.) in a Server Component.

Hooks are designed for Client Components because they need:

A component that stays alive in the browser
The ability to re-render when state changes
Access to browser APIs and the React client runtime

Server Components:

Run once on the server
Produce HTML / RSC payload
Do not re-render in the browser
Have no access to the client-side React runtime

47. What are the advantages of Server Components?
1. Zero JS bundle size
   Server Component ka code kabhi browser ko nahi jaata — sirf uska rendered output (HTML/RSC payload) jaata hai. Isse client-side JS bundle chhota rehta hai, jo faster load aur faster Time-to-Interactive deta hai.

1. Direct backend access — bina API layer ke
   Server Component seedha database, file system, ya internal services access kar sakta hai — koi separate REST/GraphQL API banane ki zaroorat nahi.
1. Secrets/sensitive data safe rehte hain
   API keys, DB credentials, tokens — sab server pe hi rehte hain, browser ko kabhi expose nahi hote. Client Component me agar galti se secret use ho jaye, to wo JS bundle me chala jaata hai (security risk).

1. Faster initial page load
   Poora HTML server pe hi ban ke aata hai (pre-rendered) — user ko turant content dikh jaata hai, JS load hone ka wait nahi karna padta.

1. Better SEO
   Search engine crawlers ko complete HTML milta hai (client-side rendering ki tarah khaali shell nahi), isliye indexing better hoti hai.

1. What are the disadvantages or limitations of Client Components?
   --Larger JS bundle size
   Client Component ka poora code (React + logic + libraries) browser ko bhejna padta hai — jitne zyada Client Components, utna bada bundle, utna slow load.
   --No direct backend/DB access
   Client Component seedha database ya file system access nahi kar sakta — API/Route Handler banana hi padega, jo extra layer aur extra network request add karta hai.
   --Secrets expose hone ka risk
   Agar galti se koi secret/API key Client Component ke andar use ho jaye, to wo JS bundle ke through browser me expose ho jaata hai — security risk.
   --Extra network round-trip for data
   Data fetch karne ke liye useEffect use karna padta hai, jo component mount hone ke baad chalta hai — matlab pehle HTML aata hai (khaali/loading state ke saath), fir JS load hota hai, fir data fetch hota hai. Isse user ko loading spinners zyada dikhte hain.
   --Hydration overhead
   Client Component ko browser me "hydrate" hona padta hai (event listeners attach karna, React tree match karna) — ye extra CPU work hai jo Server Component ko nahi karna padta.
   --Poor SEO (agar SSR na ho)
   Agar data client-side fetch ho raha hai (jaise useEffect se), to initial HTML me wo data nahi hota — search engine crawler ko khaali content mil sakta hai.
   --Hydration mismatch errors ka risk
   Agar server aur client ka rendered output match nahi karta (jaise Date.now(), Math.random(), ya window check), to React hydration error deta hai.
   --Slower on weak devices/networks
   Chunki JS download + parse + execute browser (user ke device) pe hota hai, kamzor mobile/slow network pe ye noticeably slow ho sakta hai — jabki Server Component ka kaam powerful server pe hota hai.

1. What is component composition in Server and Client Components?
   Component composition ka matlab hai — Server aur Client Components ko is tarah arrange karna ki har component apna sahi kaam kare, aur interactivity sirf wahi jaaye jaha zaroorat ho. Ye pattern App Router ka core design principle hai.

Basic idea:

Server Component "shell" banata hai — layout, static content, data fetching
Client Component sirf wahi specific part handle karta hai jaha interactivity chahiye (button, form, toggle)
Poora page Client Component banane ke bajaye, sirf chhote interactive "islands" Client Component rakhte ho, baaki sab Server Component hi rehta hai

1. How do you pass data from a Server Component to a Client Component?

1. What kinds of props can be passed from Server Components to Client Components?

1. What is streaming in Next.js?
   In Next.js, streaming is a rendering technique that allows you to progressively send HTML content from the server to the client in chunks, rather than waiting for the entire page to be ready before sending anything.

How Streaming Works
Instead of the traditional approach where the server renders the entire page and then sends it all at once, streaming breaks the page into smaller pieces that can be:

Rendered independently
Sent as soon as they're ready
Displayed progressively in the browser

1. How does `loading.tsx` work with streaming?

1. What is React Suspense and how does Next.js use it?

1. What is partial rendering in the App Router?
1. What is the difference between static and dynamic rendering?

Static Rendering: Content is generated at build time and served as pre-rendered HTML to all users.

Dynamic Rendering: Content is generated at request time for each user request.

1. What causes a route to become dynamically rendered?
1. What is dynamic rendering based on request-time data?
1. How does Next.js cache `fetch` requests?
1. What is the difference between cached and uncached data fetching?
1. How do you disable caching for a `fetch` request?
1. How do you configure revalidation for a `fetch` request?
1. What is time-based revalidation?
1. What is on-demand revalidation?
1. What is `revalidatePath`?
1. What is `revalidateTag`?
1. What are cache tags?
1. What is `unstable_cache`?
1. How do you fetch data in a Server Component?
1. What is the recommended approach for data fetching in the App Router?
1. How do you handle loading states during data fetching?
1. How do you handle errors during server-side data fetching?
1. What is the difference between `redirect()` and `notFound()`?
1. How does `redirect()` work in the App Router?
1. How do nested layouts work?
1. What are route groups?
1. Why would you use route groups?
1. What are private folders in the App Router?
1. What are parallel routes?
1. What are intercepting routes?
1. What are catch-all routes?
1. What are optional catch-all routes?
1. What are middleware/proxy capabilities in Next.js?
1. Where can middleware run?
1. What are common use cases for middleware?
1. How would you protect routes in Next.js?
1. How would you implement authentication in Next.js?
1. What is the difference between authentication and authorization?
1. How do cookies work in Next.js?
1. How do you read cookies on the server?
1. How do you set cookies in a Route Handler?
1. How do you access request headers in Server Components?
1. How do you access search parameters in the App Router?
1. What is `useRouter()`?
1. What is `usePathname()`?
1. What is `useSearchParams()`?
1. What is `useParams()`?
1. What is the difference between `useRouter()` and `<Link>`?
1. What is shallow routing, and how does routing behavior differ in the App Router?
1. How do you programmatically navigate in Next.js?

## Advanced Level

101. Explain the Next.js App Router rendering model in detail.
102. Explain the difference between the Data Cache, Full Route Cache, and client-side Router Cache.
103. How are the different Next.js caches related?
104. What invalidates the Full Route Cache?
105. What invalidates the Router Cache?
106. How does revalidation affect cached data and rendered HTML/RSC payloads?
107. What is the relationship between React Server Components and the React Server Components Payload?
108. What is the Flight protocol in the context of React Server Components?
109. How does Next.js send Server Component output to the browser?
110. Why does Next.js use the React Server Components Payload?
111. Explain static generation for dynamic routes.
112. What is `generateStaticParams()`?
113. When should you use `generateStaticParams()`?
114. Does `generateStaticParams()` run again during ISR?
115. How do you generate metadata dynamically?
116. What is the Metadata API?
117. What is `generateMetadata()`?
118. How does metadata inheritance work across nested layouts?
119. How do you generate dynamic Open Graph images?
120. What is `robots.txt` support in Next.js?
121. What is `sitemap.xml` support in Next.js?
122. What is the difference between `generateMetadata()` and static metadata?
123. How do you optimize SEO in a Next.js application?
124. How does Next.js optimize images?
125. Explain image sizing, responsive images, and lazy loading in `next/image`.
126. What is the difference between `priority`/eager loading and lazy loading for images?
127. How does Next.js optimize fonts?
128. How do you reduce JavaScript sent to the client?
129. How does Server Component usage affect bundle size?
130. How would you identify unnecessary Client Components?
131. How do you optimize a slow Next.js page?
132. How do you diagnose unnecessary client-side JavaScript?
133. How do you avoid waterfalls during server-side data fetching?
134. How does parallel data fetching work in Server Components?
135. How do you implement sequential data fetching when dependencies exist?
136. What is request memoization?
137. How does React's `cache()` relate to data fetching?
138. How do you avoid duplicate data requests?
139. What are Server Actions?
140. How do Server Actions work?
141. What is the `"use server"` directive?
142. What are the security considerations for Server Actions?
143. How do you validate Server Action input?
144. How do Server Actions differ from Route Handlers?
145. When would you choose a Server Action over a Route Handler?
146. How do you perform mutations with Server Actions?
147. How do you revalidate cached data after a mutation?
148. How do you redirect after a Server Action?
149. How do you handle optimistic UI with Server Actions?
150. What is `useFormStatus()`?
151. What is `useActionState()`?
152. How do Server Actions interact with forms?
153. How do you handle authentication inside Server Actions?
154. How do you prevent unauthorized Server Action execution?
155. How do you validate authorization on the server even when the UI hides an action?
156. How do Route Handlers differ from traditional Express API routes?
157. What runtime options are available for Route Handlers?
158. What is the Edge Runtime?
159. What is the Node.js Runtime?
160. When should you choose the Node.js runtime over the Edge Runtime?
161. What limitations can the Edge Runtime have?
162. How does middleware differ from Route Handlers?
163. How would you implement rate limiting in a Next.js application?
164. How would you securely handle JWTs in Next.js?
165. Where should access tokens and refresh tokens be stored?
166. What are HttpOnly, Secure, and SameSite cookies?
167. How would you implement role-based authorization?
168. How would you protect Server Components from unauthorized data access?
169. How would you protect API/Route Handler endpoints?
170. How would you handle CORS in Next.js?

## Expert Level

171. Explain the complete request-to-response lifecycle in a Next.js App Router application.
172. Explain how Server Components, Client Components, RSC Payload, streaming, and hydration work together.
173. Explain the complete Next.js caching architecture with a real-world example.
174. What happens when a cached `fetch` request is revalidated?
175. What happens to the Full Route Cache after data revalidation?
176. How does `revalidatePath()` differ conceptually from `revalidateTag()`?
177. How would you design a cache invalidation strategy for a large Next.js application?
178. How would you prevent stale data after a database mutation?
179. How would you design a Next.js application for high traffic?
180. How would you optimize a Next.js application with thousands of dynamic routes?
181. How would you design a multi-tenant Next.js application?
182. How would you implement tenant-aware routing?
183. How would you implement subdomain-based routing?
184. How would you handle authentication across multiple tenants?
185. How would you design authorization so users cannot bypass UI restrictions?
186. How would you secure Server Actions against malicious requests?
187. How would you protect against CSRF in a Next.js application?
188. How would you prevent XSS in a Next.js application?
189. What security risks exist when using `dangerouslySetInnerHTML`?
190. How would you securely render user-generated HTML?
191. How would you handle secrets in Server Components and Server Actions?
192. How can accidentally importing server-only code into client code create a security problem?
193. How would you structure a large-scale Next.js codebase?
194. How would you decide what belongs in Server Components versus Client Components?
195. How would you architect shared state in a Server Components application?
196. When should Redux Toolkit be used in a Next.js App Router application?
197. How do you safely use Redux with Server Components?
198. What problems can occur when creating a global Redux store on the server?
199. How would you avoid state leaking between users in a server-rendered application?
200. How would you handle browser-only state with SSR?
201. How would you solve hydration mismatch errors?
202. What causes hydration mismatches in Next.js?
203. How would you debug a hydration mismatch?
204. How would you handle third-party libraries that require `window` or `document`?
205. When would you use dynamic imports with `ssr: false`?
206. What are the trade-offs of disabling SSR for a component?
207. How does code splitting work in Next.js?
208. How does `next/dynamic` work?
209. How would you optimize a page containing a very large third-party library?
210. How would you analyze and reduce bundle size?
211. How would you optimize Core Web Vitals in Next.js?
212. How would you improve LCP in a Next.js application?
213. How would you improve INP in a Next.js application?
214. How would you reduce CLS in a Next.js application?
215. How would you diagnose a page that has excellent backend response time but poor user performance?
216. How would you handle large lists in Next.js?
217. How would you implement virtualization in a Next.js application?
218. How would you design pagination for a large dataset?
219. Offset pagination vs cursor pagination: which would you choose and why?
220. How would you implement infinite scrolling with Server Components?
221. How would you combine streaming with Suspense for a dashboard?
222. How would you design a dashboard with independently loading widgets?
223. How do parallel routes help with complex dashboards?
224. How do intercepting routes help implement modal routing?
225. How would you preserve modal state while navigating between routes?
226. How would you implement optimistic updates with Server Actions?
227. How would you handle race conditions between concurrent mutations?
228. How would you handle retries and idempotency for server mutations?
229. How would you design error handling across Server Components, Client Components, Route Handlers, and Server Actions?
230. What is the difference between expected errors and uncaught exceptions in modern Next.js patterns?
231. How do error boundaries work in the App Router?
232. How would you implement global error handling?
233. How would you log and monitor production errors?
234. How would you integrate observability into a Next.js application?
235. How would you trace a slow request across Next.js and a backend API?
236. How would you handle distributed caching with Redis?
237. How would you decide between Next.js caching and Redis caching?
238. How would you prevent cache stampedes?
239. How would you handle cache invalidation after database updates?
240. How would you design a Next.js application deployed on a CDN/serverless platform?
241. What changes when Next.js is deployed to a traditional Node.js server?
242. What changes when Next.js is deployed to a serverless environment?
243. What changes when Next.js is deployed to an edge environment?
244. How would you design a CI/CD pipeline for a Next.js application?
245. How would you manage environment variables across development, staging, and production?
246. How would you safely expose only required environment variables to the browser?
247. How would you handle database connections in serverless Next.js deployments?
248. How would you prevent connection exhaustion in serverless environments?
249. How would you implement health checks for a Next.js application?
250. How would you handle graceful degradation when a backend service is unavailable?

## Scenario-Based Interview Questions

251. A page is slow only on the first request but fast afterward. How would you investigate it?
252. A page is showing stale data after a database update. What would you check?
253. A Client Component is unnecessarily making a database/API request. How would you redesign it?
254. A Server Component needs interactive behavior. How would you structure the components?
255. A third-party chart library crashes during SSR. How would you fix it?
256. Users sometimes see another user's data after deployment. What server-state mistake could cause this?
257. A page has a hydration mismatch only in production. How would you debug it?
258. Your Next.js application has a very large JavaScript bundle. What would you investigate first?
259. A dashboard has five independent API calls and the page is slow. How would you optimize it?
260. A mutation succeeds but the UI still shows old data. How would you fix the cache/update flow?
261. A protected page is accessible briefly before redirecting unauthenticated users. How would you improve the architecture?
262. A user can manually call a hidden admin endpoint. How would you secure it?
263. You need real-time notifications in a Next.js application. What architecture would you choose?
264. You need to upload large files from the browser. How would you design the upload flow?
265. You need a multi-step form that survives navigation. Where would you keep its state?
266. You need SEO-friendly pages for millions of products. How would you design rendering and caching?
267. You need a highly personalized dashboard. Would you use static or dynamic rendering, and why?
268. You need a public marketing site and authenticated application in the same Next.js project. How would you structure it?
269. You need different layouts for different sections of an application. How would you design the route tree?
270. You need modal URLs that users can share and navigate with browser back/forward. Which Next.js routing features would you use?

## Practical Coding / Architecture Questions

271. Create a dynamic product route using the App Router.
272. Fetch product data in a Server Component and display it.
273. Implement loading and error states for a server-rendered page.
274. Implement a protected dashboard route.
275. Create a Route Handler for CRUD operations.
276. Create a Server Action that inserts a record into a database.
277. Revalidate a product page after updating product data.
278. Implement pagination in a Next.js application.
279. Implement infinite scrolling with a Client Component and server-side data fetching.
280. Implement authentication using secure cookies.
281. Implement role-based access control.
282. Implement middleware/proxy-based route protection.
283. Implement dynamic metadata for product pages.
284. Implement a sitemap for a dynamic website.
285. Implement a responsive optimized image using `next/image`.
286. Implement a dashboard using nested layouts.
287. Implement parallel routes for dashboard sections.
288. Implement an intercepting route for a modal.
289. Implement optimistic updates for a form mutation.
290. Implement a reusable server-side data-fetching layer.
291. Design a folder structure for a large enterprise Next.js application.
292. Design a Next.js frontend architecture for a MERN application.
293. Design a Next.js application that consumes a separate Node.js/Express backend.
294. Design authentication and refresh-token handling for Next.js + Node.js.
295. Design caching and revalidation for a product catalog.
296. Design a scalable Next.js application with CDN, database, Redis, and API services.
297. Explain how you would migrate a Pages Router application to the App Router.
298. Explain how you would migrate a client-heavy React application to Server Components incrementally.
299. Explain how you would debug a production-only Next.js performance problem.
300. Explain how you would review and improve an existing enterprise Next.js codebase.
