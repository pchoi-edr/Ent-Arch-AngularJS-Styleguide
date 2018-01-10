# Performance

* Optimize the digest cycle

  * Watch only the most vital variables. When required to invoke the `$digest` loop explicitly \(it should happen only in exceptional cases\), invoke it only when required \(for example: when using real-time communication, don't cause a `$digest` loop in each received message\).
  * For content that is initialized only once and then never changed, use single-time watchers like [`bindonce`](https://github.com/Pasvaz/bindonce) for older versions of AngularJS or one-time bindings in AngularJS &gt;=1.3.0.

    ```html
      <div>
        {{ ::main.things }}
      </div>
    ```

    or

    ```html
        <div ng-bind="::main.things"></div>
    ```

    After that, **no** watchers will be created for `main.things` and any changes of `main.things` will not update the view.

  * Make the computations in `$watch` as simple as possible. Making heavy and slow computations in a single `$watch` will slow down the whole application \(the `$digest` loop is done in a single thread because of the single-threaded nature of JavaScript\).

  * When watching collections, do not watch them deeply when not strongly required. Better use `$watchCollection`, which performs a shallow check for equality of the result of the watched expression and the previous value of the expression's evaluation.

  * Set third parameter in `$timeout` function to false to skip the `$digest` loop when no watched variables are impacted by the invocation of the `$timeout` callback function.

  * When dealing with big collections, which change rarely, [use immutable data structures](http://blog.mgechev.com/2015/03/02/immutability-in-angularjs-immutablejs).

* Consider decreasing number of network requests by bundling/caching html template files into your main javascript file, using [grunt-html2js](https://github.com/karlgoldstein/grunt-html2js) / [gulp-html2js](https://github.com/fraserxu/gulp-html2js). See [here](http://ng-learn.org/2014/08/Populating_template_cache_with_html2js/) and [here](http://slides.com/yanivefraim-1/real-world-angularjs#/34) for details. This is particularly useful when the project has a lot of small html templates that can be a part of the main \(minified and gzipped\) javascript file.



