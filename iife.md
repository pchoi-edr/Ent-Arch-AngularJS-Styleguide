# IIFE

* Always use IIFE to encapsulate AngularJS modules, services, directives, config, controllers, etc...
* Always, always use `Strict` mode of Javascript

> **Bad:**
```javascript

angular.module('myApp', []);

```

> **Best:**
```javascript

(function( window, angular ) {
    'use strict'

    angular.module('myApp', []);

})(window, window.angular);

```

* Strict mode prevents the use of JavaScript hoist

> **Bad:**
```javascript

newVariable = 6;

newFunction = function() {

}

```

> **Good:**
```javascript

var newVariable = 6;

var newFunction = function() {

}

```
