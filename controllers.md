# Controllers

> **Bad:**
```javascript

var app = angular.module('app', []);
app.controller('MyCtrl', function () {

});

```

> **Good:**
```javascript

function MainCtrl () {

}
angular
  .module('app', [])
  .controller('MainCtrl', MainCtrl);

```

> **Best:**
```javascript

(function () {
  angular.module('app', []);

  // MainCtrl.js
  function MainCtrl () {

  }

  angular
    .module('app')
    .controller('MainCtrl', MainCtrl);

  // AnotherCtrl.js
  function AnotherCtrl () {

  }

  angular
    .module('app')
    .controller('AnotherCtrl', AnotherCtrl);

  // and so on...

})();

```

* Do not manipulate DOM in your controllers, this will make your controllers harder for testing and will violate the [Separation of Concerns principle](https://en.wikipedia.org/wiki/Separation_of_concerns). Use directives instead.
* The naming of the controller is done using the controller's functionality \(for example shopping cart, homepage, admin panel\) and the substring `Ctrl` in the end.
* Controllers are plain javascript [constructors](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/constructor), so they will be named UpperCamelCase \(`HomePageCtrl`, `ShoppingCartCtrl`, `AdminPanelCtrl`, etc.\).
* The controllers should not be defined as globals \(even though AngularJS allows this, it is a bad practice to pollute the global namespace\).
* Use the following syntax for defining controllers:

  ```JavaScript
  function MyCtrl(dependency1, dependency2, ..., dependencyn) {
    // ...
  }
  module.controller('MyCtrl', MyCtrl);
  ```

  In order to prevent problems with minification, you can automatically generate the array definition syntax from    the standard one using tools like [ng-annotate](https://github.com/olov/ng-annotate) \(and grunt task          [grunt-ng-annotate](https://github.com/mzgol/grunt-ng-annotate)\).

  Another alternative will be to use `$inject` like:

  ```JavaScript
  angular
    .module('app')
    .controller('HomepageCtrl', HomepageCtrl);

  HomepageCtrl.$inject = ['$log', '$http', 'ngRoute'];

  function HomepageCtrl($log, $http, ngRoute) {
    // ...
  }
  ```

* Avoid use of `$scope` service to define functions and properties as part of controllers. Use `$scope` only if It's really needed:  
    0. For publish and subscribe to events: `$scope.$emit`, `$scope.$broadcast`, and `$scope.$on`.  
    0. For _watch_ values or collections: `$scope.$watch`, `$scope.$watchCollection`

* Prefer using `controller as` syntax and capture `this` using a variable:

  ```html
  <div ng-controller="MainCtrl as main">
     {{ main.things }}
  </div>
  ```

  ```JavaScript
  app.controller('MainCtrl', MainCtrl);
  MainCtrl.$inject = ['$http'];

  function MainCtrl ($http) {
    var vm = this;
    //a clearer visual connection on how is defined on the view
    vm.title = 'Some title';
    vm.description = 'Some description';

    $http.get('/api/main/things').then(function (response) {
        vm.things = response.data.things; // Adding 'things' as a property of the controller
    });
  }
  ```

  Avoid using `this` keyword repeatedly inside a controller:

  ```JavaScript
    app.controller('MainCtrl', MainCtrl);
    MainCtrl.$inject = ['$http'];

    // Avoid
    function MainCtrl ($http) {
      this.title = 'Some title';
      this.description = 'Some description';

      $http.get('/api/main/things').then(function (response) {
          // Warning! 'this' is in a different context here.
          // The property will not be added as part of the controller context
          this.things = response.data.things;
      });
    }
  ```

  Using a consistent and short variable name is preferred, for example `vm`.

  The main benefits of using this syntax:

  * Creates an 'isolated' component - binded properties are not part of `$scope` prototype chain. This is good practice since `$scope` prototype inheritance has some major drawbacks \(this is probably the reason it was removed on Angular 2\):
    * It is hard to track where data is coming from.
    * Scope's value changes can affect places you did not intend to affect.
    * Harder to refactor.
    * The '[dot rule](http://jimhoskins.com/2012/12/14/nested-scopes-in-angularjs.html)'.
  * Removes the use of `$scope` when no need for special operations \(as mentioned above\). This is a good preparation for AngularJS V2.
  * Syntax is closer to that of a 'vanilla' JavaScript constructor

    Digging more into `controller as`: [digging-into-angulars-controller-as-syntax](http://toddmotto.com/digging-into-angulars-controller-as-syntax/)

* If using array definition syntax, use the original names of the controller's dependencies. This will help you produce more readable code:

  ```JavaScript
  function MyCtrl(l, h) {
    // ...
  }

  module.controller('MyCtrl', ['$log', '$http', MyCtrl]);
  ```

  which is less readable than:

  ```JavaScript
  function MyCtrl($log, $http) {
    // ...
  }

  module.controller('MyCtrl', ['$log', '$http', MyCtrl]);
  ```

  This especially applies to a file that has so much code that you'd need to scroll through. This would possibly cause you to forget which variable is tied to which dependency.

* Make the controllers as lean as possible. Abstract commonly used functions into a service.

* Avoid writing business logic inside controllers. Delegate business logic to a `model`, using a service.  
  For example:

  ```Javascript
  //This is a common behaviour (bad example) of using business logic inside a controller.
  angular.module('Store', [])
  .controller('OrderCtrl', function () {
    var vm = this;

    vm.items = [];

    vm.addToOrder = function (item) {
      vm.items.push(item);//-->Business logic inside controller
    };

    vm.removeFromOrder = function (item) {
      vm.items.splice(vm.items.indexOf(item), 1);//-->Business logic inside controller
    };

    vm.totalPrice = function () {
      return vm.items.reduce(function (memo, item) {
        return memo + (item.qty * item.price);//-->Business logic inside controller
      }, 0);
    };
  });
  ```

  When delegating business logic into a 'model' service, controller will look like this \(see 'use services as your Model' for service-model implementation\):

  ```Javascript
  // order is used as a 'model'
  angular.module('Store', [])
  .controller('OrderCtrl', function (order) {
    var vm = this;

    vm.items = order.items;

    vm.addToOrder = function (item) {
      order.addToOrder(item);
    };

    vm.removeFromOrder = function (item) {
      order.removeFromOrder(item);
    };

    vm.totalPrice = function () {
      return order.total();
    };
  });
  ```

  Why business logic / app state inside controllers is bad?

  * Controllers instantiated for each view and dies when the view unloads
  * Controllers are not reusable - they are coupled with the view
  * Controllers are not meant to be injected

* Communicate within different controllers using method invocation \(possible when a child wants to communicate with its parent\) or `$emit`, `$broadcast` and `$on` methods. The emitted and broadcasted messages should be kept to a minimum.

* Make a list of all messages which are passed using `$emit`, `$broadcast` and manage it carefully because of name collisions and possible bugs.

  Example:

  ```JavaScript
   // app.js
   /* * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
   Custom events:
     - 'authorization-message' - description of the message
       - { user, role, action } - data format
         - user - a string, which contains the username
         - role - an ID of the role the user has
         - action - specific action the user tries to perform
   * * * * * * * * * * * * * * * * * * * * * * * * * * * * * */
  ```

* When you need to format data encapsulate the formatting logic into a [filter](#filters) and declare it as dependency:

  ```JavaScript
   function myFormat() {
     return function () {
       // ...
     };
   }
   module.filter('myFormat', myFormat);

   function MyCtrl($scope, myFormatFilter) {
     // ...
   }

   module.controller('MyCtrl', MyCtrl);
  ```

* In case of nested controllers use "nested scoping" \(the `controllerAs` syntax\):

  **app.js**

  ```javascript
   module.config(function ($routeProvider) {
     $routeProvider
       .when('/route', {
         templateUrl: 'partials/template.html',
         controller: 'HomeCtrl',
         controllerAs: 'home'
       });
   });
  ```

  **HomeCtrl**

  ```javascript
   function HomeCtrl() {
     var vm = this;

     vm.bindingValue = 42;
   }
  ```

  **template.html**

  ```html
   <div ng-bind="home.bindingValue"></div>
  ```





