# General {#general}

## Directory structure

Since a large AngularJS application has many components it's best to structure it in a directory hierarchy.  
There are two main approaches:

* Creating high-level divisions by component types and lower-level divisions by functionality.

In this way the directory structure will look like:

```
.
├─ app
│  ├── core          <--- Entry Files
│  │   ├── app.js
│  │   └── modules.js
│  └── modules       
│      ├── controllers
│      │   ├── home
│      │   │   ├── FirstCtrl.js
│      │   │   └── FirstCtrl.spec.js
│      │   │   └── SecondCtrl.js
│      │   │   └── SecondCtrl.spec.js
│      │   └── about
│      │       └── ThirdCtrl.js
│      │       └── ThirdCtrl.spec.js
│      ├── directives
│      │   ├── home
│      │   │   └── directive1.js
│      │   │   └── directive1.spec.js
│      │   └── about
│      │       ├── directive2.js
│      │       ├── directive2.spec.js
│      │       └── directive3.js
│      │       └── directive3.spec.js
│      ├── filters
│      │   ├── home
│      │   └── about
│      └── services
│          ├── CommonService.js
│          ├── CommonService.spec.js
│          ├── cache
│          │   ├── Cache1.js
│          │   ├── Cache1.spec.js
│          │   └── Cache2.js
│          │   └── Cache2.spec.js
│          └── models
│              ├── Model1.spec.js
│              ├── Model1.js
│              └── Model2.spec.js
│              └── Model2.js
├── partials
├── lib
└── e2e-tests
```

* Creating high-level divisions by functionality and lower-level divisions by component types.

Here is its layout:

```
.
├── app
│   ├── app.js
│   ├── common
│   │   ├── controllers
│   │   ├── directives
│   │   ├── filters
│   │   └── services
│   ├── home
│   │   ├── controllers
│   │   │   ├── FirstCtrl.js
│   │   │   ├── FirstCtrl.spec.js
│   │   │   └── SecondCtrl.js
│   │   │   └── SecondCtrl.spec.js
│   │   ├── directives
│   │   │   └── directive1.js
│   │   │   └── directive1.spec.js
│   │   ├── filters
│   │   │   ├── filter1.js
│   │   │   ├── filter1.spec.js
│   │   │   └── filter2.js
│   │   │   └── filter2.spec.js
│   │   └── services
│   │       ├── service1.js
│   │       ├── service1.spec.js
│   │       └── service2.js
│   │       └── service2.spec.js
│   └── about
│       ├── controllers
│       │   └── ThirdCtrl.js
│       │   └── ThirdCtrl.spec.js
│       ├── directives
│       │   ├── directive2.js
│       │   ├── directive2.spec.js
│       │   └── directive3.js
│       │   └── directive3.spec.js
│       ├── filters
│       │   └── filter3.js
│       │   └── filter3.spec.js
│       └── services
│           └── service3.js
│           └── service3.spec.js
├── partials
├── lib
└── e2e-tests
```

* In case the directory name contains multiple words, use lisp-case syntax:

```
app
 ├── app.js
 └── my-complex-module
     ├── controllers
     ├── directives
     ├── filters
     └── services
```

* Put all the files associated with the given directive \(i.e. templates, CSS/SASS files, JavaScript\) in a single folder. If you choose to use this style be consistent and use it everywhere along your project.

```
app
└── directives
    ├── directive1
    │   ├── directive1.html
    │   ├── directive1.js
    │   ├── directive1.spec.js
    │   └── directive1.sass
    └── directive2
        ├── directive2.html
        ├── directive2.js
        ├── directive2.spec.js
        └── directive2.sass
```

This approach can be combined with both directory structures above.

* The unit tests for a given component \(`*.spec.js`\) should be located in the directory where the component is. This way when you make changes to a given component finding its test is easy. The tests also act as documentation and show use cases.

```
services
├── cache
│   ├── cache1.js
│   └── cache1.spec.js
└── models
    ├── model1.js
    └── model1.spec.js
```

* The `app.js` file should contain route definitions, configuration and/or manual bootstrap \(if required\).
* Each JavaScript file should only hold **a single component**. The file should be named with the component's name.
* Use AngularJS project structure template like [Yeoman](http://yeoman.io), [ng-boilerplate](http://ngbp.github.io/ngbp/#/home).

Conventions about component naming can be found in each component section.

## Markup

[TLDR;](http://developer.yahoo.com/blogs/ydn/high-performance-sites-rule-6-move-scripts-bottom-7200.html) Put the scripts at the bottom.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>MyApp</title>
</head>
<body>
  <div ng-app="myApp">
    <div ng-view></div>
  </div>
  <script src="angular.js"></script>
  <script src="app.js"></script>
</body>
</html>
```

Keep things simple and put AngularJS specific directives after standard attributes. This will make it easier to skim your code and will make it easier to maintain because your attributes are consistently grouped and positioned.

```html
<form class="frm" ng-submit="login.authenticate()">
  <div>
    <input class="ipt" type="text" placeholder="name" require ng-model="user.name">
  </div>
</form>
```

Other HTML attributes should follow the Code Guide's [recommendation](http://mdo.github.io/code-guide/#html-attribute-order)

## Naming conventions

The following table is shown the naming conventions for every element:

| Element | Naming style | Example | usage |
| --- | --- | --- | --- |
|  | Modules | lowerCamelCase | angularApp |
|  | Controllers | Functionality + 'Ctrl' | AdminCtrl |
|  | Directives | lowerCamelCase | userInfo |
|  | Filters | lowerCamelCase | userFilter |
| Services | UpperCamelCase | User | constructor |
| Factories | lowerCamelCase | dataFactory | others |

## Others

* Use:
  * `$timeout` instead of `setTimeout`
  * `$interval` instead of `setInterval`
  * `$window` instead of `window`
  * `$document` instead of `document`
  * `$http` instead of `$.ajax`
  * `$location` instead of `window.location` or `$window.location`
  * `$cookies` instead of `document.cookie`

This will make your testing easier and in some cases prevent unexpected behaviour \(for example, if you missed `$scope.$apply` in `setTimeout`\).

* Automate your workflow using tools like:

  * [NPM](https://www.npmjs.com/)
  * [Grunt](http://gruntjs.com)
  * [Gulp](http://gulpjs.com)
  * [Yeoman](http://yeoman.io)
  * [Bower](http://bower.io)

* Use promises \(`$q`\) instead of callbacks. It will make your code look more elegant and clean, and save you from callback hell.

* Use `$resource` instead of `$http` when possible. The higher level of abstraction will save you from redundancy.

* Use an AngularJS pre-minifier \([ng-annotate](https://github.com/olov/ng-annotate)\) for preventing problems after minification.

* Don't use globals. Resolve all dependencies using Dependency Injection, this will prevent bugs and monkey patching when testing.

* Avoid globals by using Grunt/Gulp to wrap your code in Immediately Invoked Function Expression \(IIFE\). You can use plugins like [grunt-wrap](https://www.npmjs.com/package/grunt-wrap) or [gulp-wrap](https://www.npmjs.com/package/gulp-wrap/) for this purpose. Example \(using Gulp\)

  ```Javascript
    gulp.src("./src/*.js")
    .pipe(wrap('(function(){\n"use strict";\n<%= contents %>\n})();'))
    .pipe(gulp.dest("./dist"));
  ```

* Do not pollute your `$scope`. Only add functions and variables that are being used in the templates.

* Prefer the usage of [controllers instead of `ngInit`](https://github.com/angular/angular.js/commit/010d9b6853a9d2718b095e4c017c9bd5f135e0b0). There are only a few appropriate uses of ngInit, such as for aliasing special properties of ngRepeat, and for injecting data via server side scripting. Besides these few cases, you should use controllers rather than ngInit to initialize values on a scope. The expression passed to `ngInit` should go through lexing, parsing and evaluation by the Angular interpreter implemented inside the `$parse` service. This leads to:

  * Performance impact, because the interpreter is implemented in JavaScript
  * The caching of the parsed expressions inside the `$parse` service doesn't make a lot of sense in most cases, since `ngInit` expressions are often evaluated only once
  * Is error-prone, since you're writing strings inside your templates, there's no syntax highlighting and further support by your editor
  * No run-time errors are thrown

* Do not use `$` prefix for the names of variables, properties and methods. This prefix is reserved for AngularJS usage.

* Do not use `JQUERY` inside your app, If you must, use `JQLite` instead with `angular.element`.

* When resolving dependencies through the DI mechanism of AngularJS, sort the dependencies by their type - the built-in AngularJS dependencies should be first, followed by your custom ones:

```javascript
module.factory('Service', function ($rootScope, $timeout, MyCustomDependency1, MyCustomDependency2) {
  return {
    //Something
  };
});
```



