# `toVNode`

**Syntax:** `toVNode(element)`\
**Example:** `toVNode(document.querySelector('#app'))`

The `toVNode` function converts an HTMLElement or Text to a VNode. This is generally used to rehydrate HTML from SSR.

```js
import { m, toVNode, patch } from 'million';

const el = document.querySelector('#app');
const vnode = m('div', { id: 'app' }, ['Hello World'], VFlags.ONLY_TEXT_CHILDREN);
const oldVNode = toVNode(el);

patch(el, vnode, oldVNode);
```

```html
<div id="app">Hello World</div>
```
