# resizable-elements

## Why is resizable HTMLElement needed?

Usually a web application consists of 4 UI regions: header, navigation, main content, and the footer. Sometimes an application may have more depending on the architecture information. It has become a common pattern dictated by users repeating requests to allow to resize some of the UI regions in a web application. The reason behind that is to give the users more control over the areas that parts of the application occupy on the screen.

## What is a resizable HTML element?

A resizable HTML element is an element that has the `resize` attribute set with the value of the direction the user can resize the element into. These elements allow the user to "grab" the edge of the element's rectangle and drag it into the position of the allowed resize value.

## How the resizable elements work?

There are two base states of the element to consider when thinking about resizing an element. First one is when the element follows the natural flow in the document. This means that the element is not in the other group meaning when they are positioned absolutely, statically, and it is not sticky.

For the first group the resize is performed by setting the `width` and `height` property of the element.
The other group is resized by changing the `top`, `right`, `bottom`, or `left` values to the resized value.

### Resize attribute / property

The `resize` attribute added to the HTMLElement and the `resize` property accept the list of directions the element can be resized. The directions `north`, `east`, `south`, and `west` tells the browser which edge of the element rectangle can be resized.

Authors have to make sure that the `resize` values make sense for the element. For example, an element that is a first child element makes no sense to have `west` value as this effectively would resize the `east` edge. However, an element that is n-th element have have both `east` and `west` values.

### The resize event

When the user stop resizing the element and a new size is set the element dispatches the `resize` event. Authors can then store any style data related to sizing to restore the state when the application runs again.

## Example

```html
<div class="page">
  <nav resize="east">
    Navigation content
  </nav>
  <main>
    Page content
  </main>
  <aside resize="west">
    Side content
  </aside>
</div>
```

Which is equivalent to:

```html
<div class="page">
  <nav>
    Navigation content
  </nav>
  <main  resize="west east">
    Page content
  </main>
  <aside>
    Side content
  </aside>
</div>
```

## Polyfill

See the `polyfill.js` file for a polyfill of this behavior. Currently the polyfill only support resizing of the elements by setting their width and height. Absolute positioned elements are not yet supported.

## Alternative solutions

The browser may not change the DOM at all while resizing the element. Similarly to how the drag and drop is implemented in the browsers, the resizer may create a clone of the resized node and generate a feedback view for the user. Only when the user finish resizing the element the `resize` event is dispatched by the element and the author can decide what should be done next. The `resize` event contains additional properties describing what direction changes and the delta between the original edge position and the end position. Authors then can update the style property depending on what is the element's positioning.
