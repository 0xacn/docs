# Installation

There are 2 ways to include Million into your project:

- Importing it as a module
- Including it from a `<script>` tag

Million doesn't require build tools by default, but it is highly recommended you use NPM to install, especially if you are building a UI library.

## As a module

If you use NPM to install Million, ensure that you use a bundler such as [Vite](https://vitejs.dev) or [Rollup](https://rollupjs.org/).

Run the following command to install it.

```sh
npm install million
```

Now, import Million into your source file like so:

```js
import {
  m,
  patch,
  createElement,
  /* or any other exports you want to access */
} from 'million';
```

## From a script tag

This is by far the simplest way to get started with Million. Include the following `<script>` tag in the head of your HTML page. Then, use a CDN like UNPKG and import million by ES Module.

```html
<script type="module">
  import {
    m,
    patch,
    createElement,
    /* or any other exports you want to access */
  } from 'https://unpkg.com/million?module';

  // Your code here
</script>
```

That's it! Million is now available for use inside your page.
