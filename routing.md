# Routing

* Use `resolve` to resolve dependencies before the view is shown.
* Do not place explicit RESTful calls inside the `resolve` callback. Isolate all the requests inside appropriate services. This way you can enable caching and follow the separation of concerns principle.



