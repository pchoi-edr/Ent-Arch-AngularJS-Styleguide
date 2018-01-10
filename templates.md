# Templates

* Use `ng-bind` or `ng-cloak` instead of simple to prevent flashing content.
  ```html
  {{ test }}
  ```
* Avoid writing complex expressions in the templates.
* When you need to set the `src` of an image dynamically use `ng-src` instead of `src` with \`\` template.
* When you need to set the `href` of an anchor tag dynamically use `ng-href` instead of `href` with \`\` template.
* Instead of using scope variable as string and using it with `style` attribute with ```, use the directive``ng-style\` with object-like parameters and scope variables as values:

```html
    <div ng-controller="MainCtrl as main">
        <div ng-style="main.divStyle">my beautifully styled div which will work in IE</div>;
    </div>
```

```JavaScript
  angular
    .module('app')
    .controller('MainCtrl', MainCtrl);

  MainCtrl.$inject = [];

  function MainCtrl() {
    var vm = this;
    vm.divStyle = {
        width: 200,
        position: 'relative'
    };
  }
```



