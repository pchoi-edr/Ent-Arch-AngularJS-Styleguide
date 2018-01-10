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
