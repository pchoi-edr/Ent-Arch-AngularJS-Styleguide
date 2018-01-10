# Services

This section includes information about the service component in AngularJS. It is not dependent of the way of definition \(i.e. as provider, `.factory`, `.service`\), except if explicitly mentioned.

* Use camelCase to name your services.

  * UpperCamelCase \(PascalCase\) for naming your services, used as constructor functions i.e.:

    ```JavaScript
    function MainCtrl(User) {
        var vm = this;
        vm.user = new User('foo', 42);
    }

    module.controller('MainCtrl', MainCtrl);

    function User(name, age) {
      this.name = name;
      this.age = age;
    }

    module.factory('User', function () {
      return User;
    });
    ```

  * lowerCamelCase for all other services.

* Encapsulate all the business logic in services. Prefer using it as your `model`. For example:

  ```Javascript
  // order is the 'model'
  angular.module('Store')
  .factory('order', function () {
      var add = function (item) {
        this.items.push (item);
      };

      var remove = function (item) {
        if (this.items.indexOf(item) > -1) {
          this.items.splice(this.items.indexOf(item), 1);
        }
      };

      var total = function () {
        return this.items.reduce(function (memo, item) {
          return memo + (item.qty * item.price);
        }, 0);
      };

      return {
        items: [],
        addToOrder: add,
        removeFromOrder: remove,
        totalPrice: total
      };
  });
  ```

  See 'Avoid writing business logic inside controllers' for an example of a controller consuming this service.

* Services representing the domain preferably a `service` instead of a `factory`. In this way we can take advantage of the "klassical" inheritance easier:

  ```JavaScript
    function Human() {
      //body
    }
    Human.prototype.talk = function () {
      return "I'm talking";
    };

    function Developer() {
      //body
    }
    Developer.prototype = Object.create(Human.prototype);
    Developer.prototype.code = function () {
      return "I'm coding";
    };

    myModule.service('human', Human);
    myModule.service('developer', Developer);
  ```

* For session-level cache you can use `$cacheFactory`. This should be used to cache results from requests or heavy computations.

* If given service requires configuration define the service as provider and configure it in the `config` callback like:

  ```JavaScript
    angular.module('demo', [])
    .config(function ($provide) {
      $provide.provider('sample', function () {
        var foo = 42;
        return {
          setFoo: function (f) {
            foo = f;
          },
          $get: function () {
            return {
              foo: foo
            };
          }
        };
      });
    });

    var demo = angular.module('demo');

    demo.config(function (sampleProvider) {
      sampleProvider.setFoo(41);
    });
  ```





