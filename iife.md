# IIFE

* Always use IIFE to encapsulate AngularJS modules, services, directives, config, controllers, etc...
* Always, always use `Strict` mode of Javascript

> **Bad:**

```js
angular.module('myApp', []);
```

> **Best:**

```js
(function( window, angular ) {
    'use strict'

    angular.module('myApp', []);

})(window, window.angular);
```

* Strict mode prevents the use of JavaScript hoist

> **Bad:**

```js
newVariable = 6;

newFunction = function() {

}
```

> **Good:**

```js
var newVariable = 6;

var newFunction = function() {

}
```



