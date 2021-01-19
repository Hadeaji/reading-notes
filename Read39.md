# Next.js

## Create a Next.js App

To build a complete web application with React from scratch, there are many important details you need to consider:

- Code has to be bundled using a bundler like webpack and transformed using a compiler like Babel.
- You need to do production optimizations such as code splitting.
- You might want to statically pre-render some pages for performance and SEO. You might also want to use server-side rendering or client-side rendering.
- You might have to write some server-side code to connect your React app to your data store.

A framework can solve these problems. But such a framework must have the right level of abstraction — otherwise it won’t be very useful. It also needs to have great "Developer Experience", ensuring you and your team have an amazing experience while writing code.

**Next.js: The React Framework** Enter Next.js, the React Framework. Next.js provides a solution to all of the above problems. But more importantly, it puts you and your team in the pit of success when building React applications.

### Setup

First, let’s make sure that your development environment is ready.

Create a Next.js app
To create a Next.js app, open your terminal, cd into the directory you’d like to create the app in, and run the following command:

```
npx create-next-app nextjs-blog --use-npm --example "https://github.com/vercel/next-learn-starter/tree/master/learn-starter"
```

**Run the development server:**

You now have a new directory called nextjs-blog. Let’s cd into it:

`cd nextjs-blog`

Then, run the following command:

`npm run dev`

## Navigate Between Pages

> **Download Starter Code (Optional)**

```
npx create-next-app nextjs-blog --use-npm --example "https://github.com/vercel/next-learn-starter/tree/master/navigate-between-pages-starter"
```

### Pages in Next.js

In Next.js, a page is a React Component exported from a file in the `pages directory`.

Pages are associated with a route based on their file name. For example, in development:

- pages/index.js is associated with the / route.
- pages/posts/first-post.js is associated with the /posts/first-post route.

We already have the pages/index.js file, so let’s create pages/posts/first-post.js to see how it works.

**Create a New Page**
 Create the posts directory under pages.

Create a file called first-post.js inside the posts directory with the following content:

```
export default function FirstPost() {
  return <h1>First Post</h1>
}
```

The component can have any name, but you must export it as a default export.

This is how you can create different pages in Next.js.

Simply create a JS file under the pages directory, and the path to the file becomes the URL path.

In a way, this is similar to building websites using HTML or PHP files. Instead of writing HTML you write JSX and use React Components.

### Link Component

When linking between pages on websites, you use the `<a>` HTML tag.

In Next.js, you use the Link Component from next/link to wrap the `<a>` tag. `<Link>` allows you to do client-side navigation to a different page in the application.

Using `<Link>` First, open pages/index.js, and import the Link component from next/link by adding this line at the top:

```
import Link from 'next/link'
```

Then find the h1 tag that looks like this:

```
<h1 className="title">
  Learn <a href="https://nextjs.org">Next.js!</a>
</h1>
```

And change it to:

```
<h1 className="title">
  Read{' '}
  <Link href="/posts/first-post">
    <a>this page!</a>
  </Link>
</h1>
```

> {' '} adds an empty space, which is used to divide text over multiple lines.

Next, open pages/posts/first-post.js and replace its content with the following:

```
import Link from 'next/link'

export default function FirstPost() {
  return (
    <>
      <h1>First Post</h1>
      <h2>
        <Link href="/">
          <a>Back to home</a>
        </Link>
      </h2>
    </>
  )
}
```

As you can see, the Link component is similar to using `<a>` tags, but instead of `<a href="…">`, you use `<Link href="…">` and put an `<a>` tag inside.

### Client-Side Navigation

The Link component enables client-side navigation between two pages in the same Next.js app.

Client-side navigation means that the page transition happens using JavaScript, which is faster than the default navigation done by the browser.

Here’s a simple way you can verify it:

- Use the browser’s developer tools to change the background CSS property of `<html>` to `yellow`.
- Click on the links to go back and forth between the two pages.
- You’ll see that the yellow background persists between page transitions.

If you’ve used `<a href="…">` instead of `<Link href="…">` and did this, the background color will be cleared on link clicks because the browser does a full refresh.

**Code splitting and prefetching**

Next.js does code splitting automatically, so each page only loads what’s necessary for that page. That means when the homepage is rendered, the code for other pages is not served initially.

This ensures that the homepage loads quickly even if you have hundreds of pages.

Only loading the code for the page you request also means that pages become isolated. If a certain page throws an error, the rest of the application would still work.

## Assets, Metadata, and CSS

> **Download Starter Code (Optional)**

```
npx create-next-app nextjs-blog --use-npm --example "https://github.com/vercel/next-learn-starter/tree/master/assets-metadata-css-starter"
```

### Metadata

What if we wanted to modify the metadata of the page, such as the `<title>` HTML tag?

`<title>` is part of the `<head>` HTML tag, so let's dive into how we can modify the `<head>` tag in a Next.js page.

Open pages/index.js in your editor and find the following lines:

```
<Head>
  <title>Create Next App</title>
  <link rel="icon" href="/favicon.ico" />
</Head>
```

Notice that `<Head>` is used instead of the lowercase `<head>`. `<Head>` is a React Component that is built into Next.js. It allows you to modify the `<head>` of a page.

**Adding Head to first-post.js**

We haven't added a `<title>` to our /posts/first-post route. Let's add one.

Open the pages/posts/first-post.js file and add an import for Head from next/head at the beginning of the file:

`import Head from 'next/head'`

Then, update the exported FirstPost component to include the Head component. For now, we‘ll add just the title tag:

```
export default function FirstPost() {
  return (
    <>
      <Head>
        <title>First Post</title>
      </Head>
      <h1>First Post</h1>
      <h2>
        <Link href="/">
          <a>Back to home</a>
        </Link>
      </h2>
    </>
  )
}
```

### CSS Styling

Let’s now talk about CSS styling.

As you can see, our index page `(http://localhost:3000)` already has some styles. If you take a look at pages/index.js, you should see code like this:

```
<style jsx>{`
  …
`}</style>
```

This page is using a library called styled-jsx. It’s a “CSS-in-JS” library — it lets you write CSS within a React component, and the CSS styles will be scoped (other components won’t be affected).

Next.js has built-in support for styled-jsx, but you can also use other popular CSS-in-JS libraries such as styled-components or emotion.

**Writing and Importing CSS**

Next.js has built-in support for CSS and Sass which allows you to import .css and .scss files.

### Layout Component

First, Let’s create a Layout component which will be shared across all pages.

Create a top-level directory called components.
Inside components, create a file called layout.js with the following content:

```
export default function Layout({ children }) {
  return <div>{children}</div>
}
```

Then, open pages/posts/first-post.js, add an import for the Layout component, and make it the outermost component:

```
import Head from 'next/head'
import Link from 'next/link'
import Layout from '../../components/layout'

export default function FirstPost() {
  return (
    <Layout>
      <Head>
        <title>First Post</title>
      </Head>
      <h1>First Post</h1>
      <h2>
        <Link href="/">
          <a>Back to home</a>
        </Link>
      </h2>
    </Layout>
  )
}
```

**Adding CSS**

Now, let’s add some styles to the Layout component. To do so, we’ll use CSS Modules, which lets you import CSS files in a React component.

Create a file called components/layout.module.css with the following content:

```
.container {
  max-width: 36rem;
  padding: 0 1rem;
  margin: 3rem auto 6rem;
}
```

Important: To use CSS Modules, the CSS file name must end with .module.css.

To use this container class inside components/layout.js, you need to:

- Import the CSS file and assign a name to it, like styles
- Use styles.container as the className

Open components/layout.js and replace its content with the following:

```
import styles from './layout.module.css'

export default function Layout({ children }) {
  return <div className={styles.container}>{children}</div>
}
```

### Global Styles

CSS Modules are useful for component-level styles. But if you want some CSS to be loaded by every page, Next.js has support for that as well.

To load global CSS files, create a file called pages/_app.js with the following content:

```
export default function App({ Component, pageProps }) {
  return <Component {...pageProps} />
}
```

This App component is the top-level component which will be common across all the different pages. You can use this App component to keep state when navigating between pages

**Restart the Development Server**

> **Important:** You need to restart the development server when you add pages/_app.js. Press Ctrl + c to stop the server and run: `npm run dev`

### Styling Tips

Here are some styling tips that might be helpful.

- **Using classnames library to toggle classes**

- **Customizing PostCSS Config:**

Out of the box, with no configuration, Next.js compiles CSS using PostCSS.

To customize PostCSS config, you can create a top-level file called postcss.config.js. This is useful if you’re using libraries like Tailwind CSS.

Here are the steps to add Tailwind CSS. We recommend using postcss-preset-env and postcss-flexbugs-fixes to match Next.js’s default behavior. First, install the packages:

```
npm install tailwindcss postcss-preset-env postcss-flexbugs-fixes
```

Then write the following for postcss.config.js:

```
module.exports = {
  plugins: [
    'tailwindcss',
    'postcss-flexbugs-fixes',
    [
      'postcss-preset-env',
      {
        autoprefixer: {
          flexbox: 'no-2009'
        },
        stage: 3,
        features: {
          'custom-properties': false
        }
      }
    ]
  ]
}
```

- **Using Sass**

Out of the box, Next.js allows you to import Sass using both the .scss and .sass extensions. You can use component-level Sass via CSS Modules and the .module.scss or .module.sass extension.

Before you can use Next.js' built-in Sass support, be sure to install sass:

```
npm install sass
```
