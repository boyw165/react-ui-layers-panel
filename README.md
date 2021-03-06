React UI : Layers Panel
=======================

It's a Photoshop-liked layers pannel (build on [React](https://github.com/facebook/react) / [Flux](https://github.com/facebook/flux)) supporting **SORT** / **HIDE** / **LOCK** / **DELETE** / **DUPLICATE** layers. I assume you're already familiar with [React](https://github.com/facebook/react) and [Flux](https://github.com/facebook/flux). Check them out if not.

> This project is still young. Feel free to correct me if there're wrong/bad codes.

Demo
----

![alt capture from Facebook](./demo/images/demo.gif)

[demo](https://dl.dropboxusercontent.com/u/3587501/react/ui/layers-panel/index.html)

Usage
-----

Before creating component you should create `layers store` for the componenet. The store is a JSON array indicating the properties of a layer. Of course you can share the store among different React components.

```
var createLayerStore = require('react-ui-layers-panel').createLayerStore;
var store = createLayerStore([{isVisible: true, isLocked: false, data: {tagName: 'svg', ...}}]);

```
If you use [browserify](https://github.com/substack/node-browserify) or [watchify](https://github.com/substack/watchify) to build JavaScript bundle, remember to externalize the `react` for better performance and less size of JavaScript bundle.

Example:

```
browserify --external react ...
```

Component (view) and its data (module) Example:

```
var React = typeof window === 'object' ? window.React : require('react');
var LayersPanel = require('react-ui-layers-panel').LayersPannel;
var createLayerStore = require('react-ui-layers-panel').createLayerStore;

// Initialize the store for the LayersPanel.
var store = createLayerStore([{isVisible: true, isLocked: false, data: 1111},
                              {isVisible: false, isLocked: true, data: 2222},
                              {isVisible: false, isLocked: false, data: 3333},
                              {isVisible: true, isLocked: false, data: 4444},
                              {isVisible: false, isLocked: true, data: 5555}]);

React.render(
  <LayersPanel store={store}/>,
  document.getElementById('react-example')
);
```

Actually `createLayerStore ` returns an object containing the `store` and `action` properties. You could use `action` to apply changes to layers and get layer's properties through `store`.

> Note: You **SHOULD NOT** change the layer store directly. Or it will break the logic and make the whole system inconsistent.

![alt capture from Facebook](./demo/images/flux-simple-f8-diagram-explained-1300w.png)
copyright@Facebook

Layer Store API
---------------
You should care about the format of **Layer Object**. It is quite simple and only has few properties. These are properties you'd care.

```
{
  isVisible: true,
  isLocked: false,
  data: arbitrary object | null | undefined
}
```

Useful APIs:

```
/**
 * @param {Integer} token - The token(or index) of the layer in store you want.
 * @return {Object} - The layer object.
 */
getLayerState(token)
```

```
/**
 * @return {Array} - The array of all layer objects.
 */
getAll()
```

```
/**
 * @return {Integer} - The total amount of layers.
 */
length()
```

```
/**
 * @param {Function} callback - The callback function subscribed for layer change.
 */
listen(callback)
```

```
/**
 * @param {Function} callback - The callback function subscribed for layer change.
 */
unlisten(callback)
```

Layer Action API
----------------

The APIs you'd care:

```
/**
 * @param {Integer} token - The position of the layer in store you want to insert. Append it to the end if undefined.
 * @param {Object} layer - A layer object.
 */
insertLayer(token, layer)
```

```
/**
 * @param {Integer} token - The position of the layer in store you want to insert. Append it to the end if undefined.
 * @param {Object} layer - Array of layer objects.
 */
insertLayers(token, layers)
```

```
/**
 * @param {Integer} token - The token(or index) of the layer in store you want to delete.
 */
deleteLayer(token)
```

```
/**
 * @param {Integer} token - The token(or index) of the layer in store you want to change.
 * @param {Boolean|undefined} isVisible - Whether the layer is visible or not. Won't change if undefined.
 * @param {Boolean|undefined} isLocked - Whether the layer is locked or not. Won't change if undefined.
 */
setLayerState(token, isVisible, isLocked)
```

```
/**
 * @param {Integer} token - The token(or index) of the layer in store you want to duplicate.
 */
duplicateLayer(token)
```

```
/**
 * @param {Integer} from - The 1st position of the layer you want to exchange with.
 * @param {Integer} to - The 2nd position of the layer you want to exchange with.
 */
exchangeLayers(from, to)
```


