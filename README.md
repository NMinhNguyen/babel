==================

**This is a (hopefully temporary) fork of `@babel/plugin-transform-destructuring` that optimises array destructuring of `useState` and `useReducer` React Hooks.**

**It only includes these two changes:**

```diff
@@ -1,6 +1,10 @@
 import { declare } from '@babel/helper-plugin-utils';
 import { types as t } from '@babel/core';

+const forcedLoose = ['useReducer', 'useState'].map(
+  name => new RegExp(`^_(?:React\\$)?${name}\\d*$`)
+);
+
 export default declare((api, options) => {
   api.assertVersion(7);
```

```diff
@@ -129,7 +133,11 @@ export default declare((api, options) => {
     }

     toArray(node, count) {
-      if (this.arrayOnlySpread || (t.isIdentifier(node) && this.arrays[node.name])) {
+      if (
+        this.arrayOnlySpread ||
+        (t.isIdentifier(node) &&
+          (this.arrays[node.name] || forcedLoose.some(regex => regex.test(node.name))))
+      ) {
```

==================

# @babel/plugin-transform-destructuring

> Compile ES2015 destructuring to ES5

See our website [@babel/plugin-transform-destructuring](https://babeljs.io/docs/en/next/babel-plugin-transform-destructuring.html) for more information.

## Install

Using npm:

```sh
npm install --save-dev @babel/plugin-transform-destructuring
```

or using yarn:

```sh
yarn add @babel/plugin-transform-destructuring --dev
```
