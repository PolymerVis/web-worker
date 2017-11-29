web-worker
[![GitHub release](https://img.shields.io/github/release/PolymerVis/web-worker.svg)](https://github.com/PolymerVis/web-worker/releases)
[![Build Status](https://travis-ci.org/PolymerVis/web-worker.svg?branch=master)](https://travis-ci.org/PolymerVis/web-worker)
[![Published on webcomponents.org](https://img.shields.io/badge/webcomponents.org-published-blue.svg)](https://www.webcomponents.org/element/PolymerVis/web-worker)
[![styled with prettier](https://img.shields.io/badge/styled_with-prettier-ff69b4.svg)](https://github.com/prettier/prettier)
==========

<!---
```
<custom-element-demo>
  <template>
    <link rel="import" href="../polymer/lib/elements/dom-bind.html">
    <link rel="import" href="web-worker.html">
    <dom-bind>
      <template is="dom-bind">
        <next-code-block></next-code-block>
      </template>
    </dom-bind>
  </template>
</custom-element-demo>
```
-->
```html
<!-- create and run web-worker with the inline code. -->
<web-worker id="worker" last-message="{{reply}}">
  <script type="text/js-worker">
    onmessage = function(e) {
      postMessage(e.data.a + e.data.b);
    };
  </script>
</web-worker>

<!-- data-binded div to show msg from worker -->
<div>1 + 2 = <b>[[reply]]</b></div>

<!-- button to run function -->
<button onclick="javascript:document.querySelector('#worker').postMessage({a: 1, b: 2});">Calculate 1+2</button>
```

## Installation
```
bower install --save PolymerVis/web-worker
```

## Documentation and demos
More examples and documentation can be found at `web-worker` [webcomponents page](https://www.webcomponents.org/element/PolymerVis/web-worker).

## Disclaimers
PolymerVis is a personal project and is NOT in any way affliated with Polymer or Google.

## `web-worker`
`web-worker` is a Polymer 2.0 element to provide an easy way to data-bind with a [web worker](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers)-based task. Compute-intensive tasks are usually done on a web worker to prevent blocking of the UI thread.

## Quick start
### Create a web worker
Creating a web worker from a URL.
```html
<web-worker url="some_script.js"></web-worker>
```

Creating a web worker through `script` attribute.
```html
<web-worker script="postMessage('hello!');"></web-worker>
```

Creating a web worker from inline code.
```html
<web-worker>
  <script type="text/js-worker">
    postMessage('hello!');
  </script>
</web-worker>
```

### Message from web worker
Data-bind to the last message/reply from the worker with the `last-message` attribute.
```html
<web-worker last-message="{{reply}}">
  <script type="text/js-worker">
    postMessage('hello!');
  </script>
</web-worker>
```

Or listen to the `message` event.
```html
<web-worker on-message="onReply">
  <script type="text/js-worker">
    postMessage('hello!');
  </script>
</web-worker>
```

### Message to web worker
You can post a message to the worker with `postMessage` function or through one of the 3 message attributes (`text-to-worker`, `object-to-worker`, or `array-to-worker`).

`postMessage` function
```js
var ele = document.createElement('web-worker'); // create web-worker element
ele.url = 'some_script.js'; // set url to worker script
ele.postMessage('hello!') // return a Promise to the next message from worker
  .then(reply => console.log(reply));
```

Data-binding to `text-to-worker`, `object-to-worker`, or `array-to-worker`
```html
<web-worker url="some_script.js"
  text-to-worker="hello"
  last-message="{{reply}}"></web-worker>
```

### Running ad-hoc functions
You can wrap a function and execute it in a once-off web worker with `executeFnOnce`. This function must be stateless and has no external dependencies, as the function and arguments are serialized and executed in the worker context.


```js
document.createElement('web-worker')  // create web-worker element
  .executeFnOnce((a, b) => a + b, a, b) // serialized function and run in worker
  .then(sum => alert(a + ' + ' + b + ' = ' + sum)); // return a Promise to the output
```

### Running ad-hoc jobs
You can execute once-off web workers with `executeUrlOnce`.
```js
     document.createElement('web-worker')
       .executeUrlOnce('echo.js', 'hello')
       .then(msg => console.log('msg'));
```

## Development

### Unit testing
```
npm test
```
