# Next.js Features Explained with Examples

This document provides an in-depth look at 26 key Next.js features, with explanations and code examples for each.

## 1. Server-Side Rendering (SSR)

**Explanation:** SSR improves performance and SEO by rendering pages on the server for each request. This ensures that the initial HTML content is fully rendered, which is beneficial for search engines and provides a faster first contentful paint.

**Example:**

```javascript
// pages/ssr-example.js
export async function getServerSideProps(context) {
  const res = await fetch(`https://api.example.com/data`);
  const data = await res.json();

  return {
    props: { data }, // will be passed to the page component as props
  };
}

function SSRPage({ data }) {
  return <div>Server-side rendered data: {JSON.stringify(data)}</div>;
}

export default SSRPage;
```

## 2. Static Site Generation (SSG)

**Explanation:** SSG generates static HTML at build time for faster page loads. This is ideal for pages with content that doesn't change frequently, as it can be served directly from a CDN.

**Example:**

```javascript
// pages/ssg-example.js
export async function getStaticProps() {
  const res = await fetch('https://api.example.com/data');
  const data = await res.json();

  return {
    props: { data },
  };
}

function SSGPage({ data }) {
  return <div>Statically generated data: {JSON.stringify(data)}</div>;
}

export default SSGPage;
```

## 3. Incremental Static Regeneration (ISR)

**Explanation:** ISR allows updating static pages without rebuilding the entire site. It combines the benefits of static generation with the ability to update content periodically.

**Example:**

```javascript
// pages/isr-example.js
export async function getStaticProps() {
  const res = await fetch('https://api.example.com/data');
  const data = await res.json();

  return {
    props: { data },
    revalidate: 60, // regenerate page every 60 seconds
  };
}

function ISRPage({ data }) {
  return <div>ISR data (updates every 60 seconds): {JSON.stringify(data)}</div>;
}

export default ISRPage;
```

## 4. API Routes

**Explanation:** API Routes enable creating serverless API endpoints within Next.js, allowing you to build full-stack applications without needing a separate backend server.

**Example:**

```javascript
// pages/api/hello.js
export default function handler(req, res) {
  res.status(200).json({ message: 'Hello from Next.js API route!' });
}
```

## 5. File-based Routing

**Explanation:** Next.js automatically creates routes based on the file structure in the `pages` directory, simplifying the routing process.

**Example file structure:**
```
pages/
├── index.js         // Routes to /
├── about.js         // Routes to /about
└── blog/
    ├── index.js     // Routes to /blog
    └── [slug].js    // Routes to /blog/:slug
```

## 6. Dynamic Routing

**Explanation:** Dynamic routing supports parameter-based routes for dynamic content, allowing you to create pages that can handle variable parameters in the URL.

**Example:**

```javascript
// pages/posts/[id].js
import { useRouter } from 'next/router';

export default function Post() {
  const router = useRouter();
  const { id } = router.query;

  return <p>Post: {id}</p>;
}
```

## 7. Image Optimization

**Explanation:** Next.js provides automatic image optimization and responsive image generation, improving loading times and visual stability.

**Example:**

```jsx
import Image from 'next/image';

function OptimizedImage() {
  return (
    <Image
      src="/images/profile.jpg"
      alt="Profile picture"
      width={500}
      height={500}
      layout="responsive"
    />
  );
}
```

## 8. Font Optimization

**Explanation:** Next.js automatically optimizes and loads custom fonts, ensuring they are loaded efficiently and reducing layout shifts.

**Example:**

```javascript
// pages/_document.js
import Document, { Html, Head, Main, NextScript } from 'next/document';

class MyDocument extends Document {
  render() {
    return (
      <Html>
        <Head>
          <link
            href="https://fonts.googleapis.com/css2?family=Inter&display=optional"
            rel="stylesheet"
          />
        </Head>
        <body>
          <Main />
          <NextScript />
        </body>
      </Html>
    );
  }
}

export default MyDocument;
```

## 9. Code Splitting

**Explanation:** Next.js automatically splits code into smaller chunks, loading only the necessary JavaScript for each page. This improves initial load times and overall performance.

**Note:** This feature is built-in and doesn't require specific code examples.

## 10. Fast Refresh

**Explanation:** Fast Refresh provides instant feedback during development without losing component state, significantly improving the developer experience.

**Note:** This feature is built-in and doesn't require specific code examples.

## 11. TypeScript Support

**Explanation:** Next.js includes built-in TypeScript support without additional configuration, enabling type-safe development out of the box.

**Example:**

```typescript
// pages/typescript-example.tsx
import { GetServerSideProps, NextPage } from 'next';

interface Props {
  message: string;
}

const TypeScriptPage: NextPage<Props> = ({ message }) => {
  return <h1>{message}</h1>;
};

export const getServerSideProps: GetServerSideProps<Props> = async () => {
  return {
    props: {
      message: 'Hello from TypeScript!',
    },
  };
};

export default TypeScriptPage;
```

## 12. CSS Support

**Explanation:** Next.js supports various styling options out of the box, including CSS Modules, Sass, and other styling solutions.

**Example (CSS Modules):**

```jsx
// components/Button.js
import styles from './Button.module.css';

function Button({ children }) {
  return <button className={styles.button}>{children}</button>;
}

export default Button;
```

```css
/* Button.module.css */
.button {
  padding: 10px 20px;
  background-color: blue;
  color: white;
}
```

## 13. Environment Variables

**Explanation:** Next.js provides easy configuration and use of environment variables, allowing secure management of sensitive information.

**Example:**

```javascript
// pages/api/db-example.js
export default function handler(req, res) {
  res.status(200).json({ dbConnection: process.env.DATABASE_URL });
}
```

## 14. Automatic Prefetching

**Explanation:** Next.js automatically prefetches links in the viewport for faster page transitions, improving the perceived performance of your application.

**Example:**

```jsx
import Link from 'next/link';

function NavBar() {
  return (
    <nav>
      <Link href="/about">
        <a>About</a>
      </Link>
      <Link href="/contact">
        <a>Contact</a>
      </Link>
    </nav>
  );
}
```

## 15. Internationalization (i18n)

**Explanation:** Next.js provides built-in support for multiple languages and locales, making it easier to create multilingual websites.

**Example:**

```javascript
// next.config.js
module.exports = {
  i18n: {
    locales: ['en', 'fr', 'de'],
    defaultLocale: 'en',
  },
}

// pages/index.js
import { useRouter } from 'next/router';

export default function Home() {
  const router = useRouter();
  const { locale } = router;

  return <h1>{locale === 'en' ? 'Welcome' : locale === 'fr' ? 'Bienvenue' : 'Willkommen'}</h1>;
}
```

## 16. AMP Support

**Explanation:** Next.js enables creating Accelerated Mobile Pages (AMP) for improved mobile performance and visibility in search results.

**Example:**

```javascript
// pages/amp-example.js
export const config = { amp: true };

function AmpPage() {
  return <h1>This is an AMP page</h1>;
}

export default AmpPage;
```

## 17. Custom App and Document

**Explanation:** Next.js allows customizing the application initialization and HTML document structure through custom `_app.js` and `_document.js` files.

**Example (_app.js):**

```javascript
// pages/_app.js
import '../styles/globals.css';

function MyApp({ Component, pageProps }) {
  return (
    <Layout>
      <Component {...pageProps} />
    </Layout>
  );
}

export default MyApp;
```

## 18. Middleware

**Explanation:** Middleware enables running code before a request is completed, allowing for request modification, redirects, or additional processing.

**Example:**

```javascript
// middleware.js
export function middleware(req) {
  const { pathname } = req.nextUrl;
  if (pathname.startsWith('/api/')) {
    console.log('API route accessed:', pathname);
  }
}
```

## 19. Edge Runtime

**Explanation:** Next.js supports running code at the edge for improved performance, leveraging edge computing capabilities.

**Example:**

```javascript
// pages/api/edge.js
export const config = {
  runtime: 'edge',
};

export default function handler(req) {
  return new Response(JSON.stringify({ message: 'Hello from the edge!' }), {
    status: 200,
    headers: {
      'Content-Type': 'application/json',
    },
  });
}
```

## 20. React 18 Features

**Explanation:** Next.js supports React 18 features like Streaming SSR and Suspense, enabling more efficient rendering and improved user experiences.

**Example (using Suspense):**

```jsx
import { Suspense } from 'react';
import SlowComponent from '../components/SlowComponent';

function Page() {
  return (
    <div>
      <h1>My Page</h1>
      <Suspense fallback={<div>Loading...</div>}>
        <SlowComponent />
      </Suspense>
    </div>
  );
}

export default Page;
```

## 21. Next.js Analytics

**Explanation:** Next.js provides built-in performance analytics, helping developers identify and resolve performance issues.

**Note:** This feature is built-in and typically accessed through the Next.js dashboard or CLI.

## 22. Next.js Compiler

**Explanation:** The Rust-based compiler in Next.js enables faster builds and optimized output, improving development and build times.

**Note:** This feature is built-in and doesn't require specific code examples.

## 23. ESLint Integration

**Explanation:** Next.js includes built-in ESLint configuration for code quality, helping maintain consistent coding standards.

**Example (.eslintrc.json):**

```json
{
  "extends": "next/core-web-vitals"
}
```

## 24. Dynamic Imports

**Explanation:** Next.js allows importing JavaScript modules dynamically, enabling code-splitting and lazy-loading of components.

**Example:**

```javascript
import dynamic from 'next/dynamic';

const DynamicComponent = dynamic(() => import('../components/hello'));

function Home() {
  return (
    <div>
      <h1>Dynamic Import Example</h1>
      <DynamicComponent />
    </div>
  );
}

export default Home;
```

## 25. Export Static HTML

**Explanation:** Next.js can export the app as static HTML for deployment on any static hosting, providing flexibility in deployment options.

**Example (next.config.js):**

```javascript
module.exports = {
  output: 'export',
}
```

## 26. Zero Config

**Explanation:** Next.js works out of the box with sensible defaults but is also fully configurable, allowing for easy setup and customization as needed.

**Note:** This feature is inherent to Next.js and doesn't require specific code examples. Developers can start using Next.js with minimal configuration and customize as their project grows.

These examples and explanations cover the 26 key features of Next.js, demonstrating its power and flexibility in building modern web applications.
