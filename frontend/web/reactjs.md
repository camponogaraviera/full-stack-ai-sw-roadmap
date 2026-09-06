<div align='center'>
  <h1> 3.2 Frontend </h1>
  <h2> 3.2.3 Web Applications </h2>
  <h3> React.js </h3>
</div>

# Table of Contents

- [About](#about)
- [Creating a React Project](#creating-a-react-project)
  - [Using Create React App (deprecated)](#using-create-react-app-deprecated)
    - [React.js Files and Folders](#reactjs-files-and-folders)
  - [Using Vite](#using-vite)
  - [Using Next.js](#using-nextjs)
- [Glossary](#glossary)
- [Short Notes](#short-notes)

---

# About

[React.js](https://github.com/facebook/react) is a JavaScript library for building user interfaces. React.js (Web) and React Native (Mobile) primarily use [JavaScript](https://github.com/camponogaraviera/javascript). However, they also utilize JSX (JavaScript XML), a syntax extension for JavaScript that allows one to create declarative UI components using a syntax that closely resembles HTML or XML.

- Use plain React.js (e.g., with Vite) if you need to build large-scale web applications with client-side rendered functionality.
- Use React.js + Next.js if you need Server-Side Rendering (SSR), Static Site Generation (SSG), and automatic code splitting.

Note: Next.js has built-in features such as client-side rendering (CSR), server-side rendering (SSR), static site generation (SSG), file-based routing, code splitting, and optimization. SSR means that webpages are run on the server before they are sent back to the client.

---

# Creating a React Project

Let's look at popular frameworks to generate boilerplate code.

## Using [Create React App](https://create-react-app.dev/) (deprecated):

Note: The React team now recommends that developers use modern build tools (e.g., Vite) and frameworks (e.g., Next.js), instead of Create React App (CRA).

- Yarn:

```bash
yarn create react-app app-name
```

- NPM:

```bash
npx create-react-app app-name
```

- Start the development server:

```bash
cd app-name && yarn && yarn start
```

- Create a production build for optimization:

```bash
npm run build
```

### React.js Files and Folders

- Folders:
  - `build`: contains the optimized version of the project.
  - `node_modules`: contains all installed dependencies and its sub-dependencies.
  - `public`: static files (CSS, icons, manifest, index.html).
  - `src`: main App folder for tests, logic, routes, etc.

- Files:
  - `public/index.html`: the classic HTML file.
  - `public/manifest.json`: it makes sure the project is [progressive web compliant (PWA)](https://en.wikipedia.org/wiki/Progressive_web_app), providing essential metadata enabling the application to work on a mobile device.
  - [public/robots.txt](https://www.robotstxt.org/robotstxt.html): used to define which pages from your React APP search engine crawlers can or cannot request.
  - `src/index.js`: renders src/App.js into public/index.html.
  - `src/App.js`: the application itself.
  - `src/App.test.js`: test cases.
  - `src/reportWebVitals.js`: metrics for user experience.
  - `src/setupTests.js`: imports testing libraries.
  - `package.json`: list all the project's dependencies (packages, libraries) required to run the application.
- Build helpers:
  - `Babel`: converts React code into a version of JavaScript code the browser can understand. This happens after running `npm run build`.
  - `Webpack`: a module bundler used to make the code more performant (optimized) for the browser by dividing the project into chunks/portions of code that are located inside `build/static/js` after running `npm run build`.

## Using [Vite](https://vite.dev/guide/)

Vite has no opinion about your backend. It doesn't ship API routes, server-side rendering, or a data layer by default. Because of that, Express + Vite is a very common and well-supported pattern.

Use Vite with Yarn (recommended):

```bash
yarn create vite my-react-app --template react
```

Or use Vite with NPM 7+ (an extra double-dash is needed):

```bash
npm create vite@latest my-react-app -- --template react
```

## Using [Next.js](https://react.dev/learn/creating-a-react-app#nextjs-app-router)

Modern build tools and frameworks often utilize faster bundlers and offer better performance optimizations than CRA.

[Next.js](https://github.com/vercel/next.js) is a full-stack React framework that has built-in features such as server-side rendering (SSR), static site generation (SSG), file-based routing (replacing React-Router), and route handlers.

Instead of implementing a RESTful API using Express.js, Next.js provides its own internal `file-system routing` to create API endpoints. One can write standard HTTP methods (GET, POST, PUT, DELETE) directly inside Next.js using [Next.js Route Handlers](https://nextjs.org/blog/building-apis-with-nextjs). A Next.js [Route Handler](../../backend/api/restfull_api.md#route-handler--middleware) is a [callback function](https://github.com/camponogaraviera/javascript/blob/main/js-course/notebooks/asynchronous/callback.js) that handles incoming HTTP requests for a specific URL endpoint. The folder structure one creates in the Next.js App Router (e.g., `app/api/users/route.ts`) defines a public URL endpoint (`/api/users`) that external or internal clients can hit via `fetch`.

It is generally unwise to force an Express.js server inside Next.js because a custom server removes critical Next.js features, such as automatic static optimization.

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

JAMstack app: any app that uses JavaScript, API, and Markup.

SSR: Server-side rendering means that webpages are run on the server before they are sent back to the client.

SEM: Search engine marketing uses paid platforms to scale marketing visibility.

SEO: Search Engine Optimization is the process of improving a website so it ranks higher in search engine results (like Google), leading to more organic (non-paid) traffic. It involves a combination of technical, content, and off-site strategies.

- **On-Page SEO:** Use internal linking (a link to another page that is within the same website), URL structure, proper HTML tags (title, meta description, h1, etc.).
- **Technical SEO:** Use clean and crawlable code, secure site (HTTPS), `sitemap` (an XML file that lists website pages), and `robots.txt` file (which controls which parts of the website search engines can or cannot crawl).
- **Off-Page SEO:** Use backlinks (when another website links to yours).

---

# Short Notes

1. If you want to have more control over optimization, you can run [npm run eject](https://github.com/facebook/create-react-app/blob/main/packages/cra-template/template/README.md#npm-run-eject) (not recommended).
2. The browser does not understand `Sass` (extension .scss) and, therefore, React will convert `.scss` down to `.css`.
3. The `package-lock.json` is automatically generated when running `npm create-react-app`. It is a good practice NOT to include this file in `.gitignore`. This file is generally committed for version control.
4. When using `yarn.lock`, there is no need to keep `package-lock.json`.
5. Do not `.gitignore` the `yarn.lock` file, as it ensures that the exact versions of dependencies are installed, even if the versions in `package.json` are defined using version ranges (such as ^ or ~).
6. Babel (`.babelrc`) can also be used to transpile modern JavaScript (ES6+) code into an older JavaScript version (e.g., ES5) to ensure compatibility with environments (e.g., browsers, older Node.js versions) that do not support certain modern JavaScript syntax (e.g., `let`, `const`, `arrow functions`, `async/await`, `import/export`). Polyfills for missing built-in features (`Promise`, `fetch`, `Map`, `Set`, etc.) can be added by configuring the `@babel/preset-env` preset in the `.babelrc` file with `core-js` and the `useBuiltIns` option.
