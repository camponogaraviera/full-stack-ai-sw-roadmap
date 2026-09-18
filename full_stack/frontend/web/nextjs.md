<div align='center'>
  <h1> Frontend </h1>
  <h2> Web Application Frameworks </h2>
  <h3> Next.js </h3>
</div>

# Table of Contents

- [About](#about)
- [Creating a React Project Using Next](#creating-a-react-project-using-next)
- [Glossary](#glossary)

---

# About

Modern build tools and frameworks often utilize faster bundlers and offer better performance optimizations than CRA.

[Next.js](https://github.com/vercel/next.js) is a full-stack React framework that has built-in features such as server-side rendering (SSR), static site generation (SSG), file-based routing (replacing React-Router), and route handlers.

Instead of implementing a RESTful API using Express.js, Next.js provides its own internal `file-system routing` to create API endpoints. One can write standard HTTP methods (GET, POST, PUT, DELETE) directly inside Next.js using [Next.js Route Handlers](https://nextjs.org/blog/building-apis-with-nextjs). A Next.js [Route Handler](../../backend/api/restfull_api.md#route-handler--middleware) is a [callback function](https://github.com/camponogaraviera/javascript/blob/main/js-course/notebooks/asynchronous/callback.js) that handles incoming HTTP requests for a specific URL endpoint. The folder structure one creates in the Next.js App Router (e.g., `app/api/users/route.ts`) defines a public URL endpoint (`/api/users`) that external or internal clients can hit via `fetch`.

It is generally unwise to force an Express.js server inside Next.js because a custom server removes critical Next.js features, such as automatic static optimization.

---

# Creating a React Project Using Next

- Generating boilerplate code with NPM:

```bash
npx create-next-app@latest [project-name] [options]
```

- Using Yarn (alternative):

```bash
# Yarn Berry:
yarn dlx create-next-app@latest
```

```bash
# Yarn Classic (v1):
yarn create next-app [project-name] [options]
```

---

# Glossary

Server-side Rendering (SSR): Webpages are built on the server before they are sent back to the client.

Static Site Generation (SSG): HTML pages are generated at build time.
