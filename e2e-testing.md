# E2E Testing

E2E tests are the next common sense step after unit tests, that will allow you to trace bugs and errors in the behaviour of your system. They are great for providing a sanity check that most common scenarios of using your application works. This way you can automate the process and run it each time before you deploy your application.

Ideally, Angular End-to-End tests are written in Jasmine. These tests are run using the Protractor E2E test runner which uses native events and has special features for Angular applications.

File structure:

```
.
├── app
│   ├── app.js
│   ├── home
│   │   ├── home.html
│   │   ├── controllers
│   │   │   ├── FirstCtrl.js
│   │   │   ├── FirstCtrl.spec.js
│   │   ├── directives
│   │   │   └── directive1.js
│   │   │   └── directive1.spec.js
│   │   ├── filters
│   │   │   ├── filter1.js
│   │   │   └── filter1.spec.js
│   │   └── services
│   │       ├── service1.js
│   │       └── service1.spec.js
│   └── about
│       ├── about.html
│       ├── controllers
│       │   └── ThirdCtrl.js
│       │   └── ThirdCtrl.spec.js
│       └── directives
│           ├── directive2.js
│           └── directive2.spec.js
├── partials
├── lib
└── e2e-tests
    ├── protractor.conf.js
    └── specs
        ├── home.js
        └── about.js
```



