# Manifesto

## Past

Back in the days when JavaScript was first manifested, direct DOM manipulation was used to introduce interactivity for static web sites. Eventually, the JavaScript scene moved on to new technologies such as MooTools and jQuery, which helped **simplify certain actions into simple, reusable functions**. However, there was still one pressing issue.

Even though web development became easier, scaling large web applications that required an extensive amount of interactivity was difficult, as operations were generally imperative and difficult to organize. This led the rise to several frameworks and libraries, notably React. React aimed to allow for **declarative development and componentization to compose user interfaces scalably**. To achieve this, the team behind React used a Virtual DOM architecture, where computation and logic was deferred to an algorithm to determine changes. This means that the developer just has to figure out how the markup will look.

React helped to pioneer extensive usage of JavaScript to render web applications, leading to the rise of the Virtual DOM architecture in other libraries and usage of new technologies like JSX.

## Present

Nowadays, the prevalence of web applications is non-negligible. JavaScript and React [are everywhere](https://nextjs.org/showcase), and the Virtual DOM architecture is used in many of the popular libraries today (React.js, Vue.js, Preact.js, Elm, etc). "Pure JavaScript rendering" has subsided as development of preprocessors such as Webpack have taken hold as a standard, leading the way to preprocessors, allowing for **cross-browser compatibility (e.g. Babel), prerendering, and JAMstack technologies**.

JavaScript frameworks and libraries have entire ecosystems around them, with tools like Next.js around React supercharging how web development is done. Library developers are slowly realizing the importance of performance sacrificed in the previous generation of developers for the purpose of developer experience. This is seen with **trends towards static analysis** with Svelte, which currently leverages the prevalent usage of bundlers in most web applications.

Performance is slowly becoming a priority, but **technologies that depend on the Virtual DOM like React are fundamentally outdated**, even with incremental improvements with lazy loading and ISR in Next.js.

## Future

It is easy to to recite, reinvent, and monkey-patch what exists. With React and the Virtual DOM being such a backbone technology in the JavaScript library space, it is paramount that bleeding-edge libraries like Next.js leverage their position in the ecosystem to pioneer [compiler-based optimizations](https://tomdale.net/2017/09/compilers-are-the-new-frameworks/). While the most straightforward solution is to ditch everything and use the browser just as compile target with a series of imperative operations, the Virtual DOM arguably brings many advantages. It helps us write declarative interfaces **without templating, meaning conditions are constrained to the limit of JavaScript**, as well as [many other benefits](https://reactjs.org/blog/2013/06/05/why-react.html).

The future of the Virtual DOM is not destruction of the Virtual DOM, rather it is to **constrain usage of the Virtual DOM where it is powerful** while leveraging static analysis and compilation ([This is done through Million.js](https://dev.to/aidenybai/million-js-the-future-of-virtual-dom-1e6d)).

[→ Interested in how Million is different?](https://dev.to/aidenybai/million-js-the-future-of-virtual-dom-1e6d)

> I recently had the opportunity to chat with [@rauchg](https://twitter.com/rauchg) about Million.js as well as some of my thoughts on the current state of Virtual DOM. He explained to me that optimization of the Virtual DOM was only a facet of the next phase of web frameworks. This includes how we can improve development mode experience, tree-shaking and bundle size, etc.
> aggressive tree shaking - reduce bundle size. I highly recommend you to check out his [Twitter](https://twitter.com/rauchg) and [blog](https://rauchg.com/), as he has so much valuable insight into the industry and how impact can and needs to be made.
