# Understanding Virtual DOM

## Introduction

The virtual DOM is a tree of virtual nodes that represents what the DOM looks like. Virtual nodes are light, stateless, and are strings or JavaScript objects that only contain necessary fields. Virtual nodes can be assembled into trees, and "diffed" to make pinpoint changes to the DOM.

The reasoning behind this is because modification and access of DOM nodes is computationally expensive. A diff between virtual nodes, accessing the DOM only for modification, is the premise of virtual DOM. It avoids the DOM as much as possible, favoring plain JavaScript objects instead, making reading and writing much cheaper.

![](https://raw.githubusercontent.com/millionjs/docs/master/.github/assets/vdom.png)

## How does it work?

The Million virtual DOM contains three main functions: `m`, `createElement`, `patch`. To completely understand how virtual DOM works, let's try and create our own rudimentary virtual DOM based off of these functions (**~7 minutes read time**).

Before we start, we need to define what a virtual node is. A virtual node can either be a JavaScript object (virtual element) or a string (text).

---

The `m` function is a helper function that creates virtual elements. A virtual element contains three properties:

- `tag`: which stores the tag name of the element as a string.
- `props`: which stores the properties/attributes of the element as an object.
- `children`: which stores virtual node children of the element as an array.

An example implementation of the `m` helper function is below:

```js
const m = (tag, props = {}, children = []) => ({
  tag,
  props,
  children,
});
```

This way, we can construct virtual elements easily:

```js
m('div', { id: 'app' }, ['Hello World']);
// Is the same as:
{
  tag: 'div',
  props: { id: 'app' },
  children: ['Hello World']
}
```

---

The `createElement` function turns a virtual node into a real DOM element. This is important because we'll be using this in our `patch` function and the user may also use it to initialize their application.

We'll need to programmatically create a new detached DOM element, then iterate over the virtual element props while adding them to the DOM element, and finally iterating over the children, initializing them as well. An example implementation of the `createElement` helper function is below:

```js
const createElement = (vnode) => {
  if (typeof vnode === 'string') return document.createTextNode(vnode);

  const el = document.createElement(vnode.tag);

  for (const propName in vnode.props) {
    el[propName] = vnode.props[propName];
  }

  for (const child of vnode.children) {
    el.appendChild(createElement(child));
  }

  return el;
};
```

This way, we can convert virtual nodes to DOM elements easily:

```jsx
createElement(m('div', { id: 'app' }, ['Hello World']));
// Is the same as: <div id="app">Hello World</div>
```

---

The `patch` function takes an existing DOM element, old virtual node, and new virtual node. This won't necessarily be the most performant implementation, but this is just for demonstration purposes.

We'll need to diff the two virtual nodes, then replace out the element when needed. We do this by first determining whether one of the virtual nodes is a text, or a string, and replacing it if the old and new virtual nodes do not equate each other. Otherwise, we can safely assume both are virtual elements. After that, we diff the tag and props, and replace the element if the tag has changed. We then iterate over the children and recursively patch if a child is a virtual element. An example implementation of the `patch` helper function is below:

```js
const patch = (el, newVNode, oldVNode) => {
  if (!newVNode && newVNode !== '') return el.remove();
  if (typeof oldVNode === 'string' || typeof newVNode === 'string') {
    if (oldVNode !== newVNode) return el.replaceWith(createElement(newVNode));
  } else {
    if (oldVNode?.tag !== newVNode.tag) {
      return el.replaceWith(createElement(newVNode));
    }

    if (oldVNode) {
      for (const propName in oldVNode.props) {
        if (oldVNode.props[propName] === newVNode.props[propName])
          return el.replaceWith(createElement(newVNode));
      }

      for (let i = el.childNodes.length - 1; i >= 0; i--) {
        patch(el.childNodes[i], newVNode.children[i], oldVNode.children[i]);
      }
    }
  }
};
```

This way, we can patch DOM elements based on virtual nodes easily:

```js
const oldVNode = m('div', { id: 'app' }, ['Hello World']);
const newVNode = m('div', { id: 'app' }, ['Goodbye World']);
const el = createElement(oldVNode);
// <div id="app">Hello World</div>

patch(el, oldVNode, newVNode);
// el will become: <div id="app">Goodbye World</div>
```

---

**Notes:**

- The old virtual node must always model the DOM element until after it is patched.
- Generally speaking, applications aren't directly written with these methods, rather they should be abstracted out into components and JSX for simplicity.
- This is not the same as Million's implementation, rather it is a demonstration to better allow you to understand how the virtual DOM works.

## So... What's unique about Million then?

Million provides many major improvements: granular patching, fewer iterative passes, fast text interpolation, keyed virtual nodes, compiler flags, deltas, batching, and scheduling.

- **Granular patching:** Instead of just replacing the entire element when there is a difference in props or children, only the necessary props are changed.
- **Fewer iterative passes:** Million attempts to reduce the amount of passes during diffing, allowing for better time and space complexity.
- **Fast text interpolation:** Instead of replacing text nodes with DOM methods, Million uses compiler flags to set the `textContent` of elements to boost performance.
- **Keyed virtual elements:** This allows for the patching algorithm to skip nodes if the new virtual element key is the same as the old one, minimizing the amount of unnecessary work.
- **Compiler Flags:** This allows for the patching algorithm to skip condition branches, meaning less work is done.
- **Deltas:** Microactions can be preprogrammed to skip diffing children all together, resulting in a better time complexity and while being easily leveraged by a compiler.
- **Batching & Scheduling:** Batching together DOM manipulations is baked in by default, and task scheduling is composable for your own uses.
