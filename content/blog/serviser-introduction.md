+++

title = "test"
date = 2020-03-03
[taxonomies]
tags = ["Node.JS", "back-end", "serviser"]
+++

Four years back at Bohemia Interactive we first started by building APIs with `expressjs` as many other teams out there.

For me personaly, I liked how lightweight and flexible the framework was. But as time went by and our codebase and number of services growed, we started to experience
multiple issues both with development workflow and `expressjs` itself while operating with it at scale.  

Application reliability
-----------------------
Nature of `expressjs` being `callback` based started to stand in our way since we shifted to `Promise` based paradigm years ago.  
It was unnecessarily tedious to handle synchronous and asynchronous exceptions together in request lifecycle.  
Consider this example:  

```javascript
const router = express.Router();

router.get('/', function (req, res, next) {
    var result = someSyncFunction(); //can throw and cause uncaughtException
    someAsyncFunction(function(err, result) {
        if (err) {
            next(err); // Handle this error
        }
    }
});
```

I see few issues with above code:  
- its error-prone - meaning exceptions can leak
- you have to wrap everything in `try catch` blocks and manualy ensure the errors are corectly propagated
- `Promises` are not natively supported - leaving the developer with unecassary task of bridging promises to callbacks
  (true that with bluebird its farely simple as calling asCallback(next))

What if you could do something like this safely:  
```javascript
router.get('/', function(req, res) {
    if (somethingWentWrong) {
        throw new Error('safely handled synchrounous exception');
    }
    return asyncFetchUser(); //returns a Promise
});
```

Any framework, especially in `javascript` world, should ensure that a node process doesnt crash due to unhandled exceptions and that internal exceptions are never discloused to public.

Other issues with service reliability included handling of unreachable or failing service resources like database connections or dependencies to other services.  
What if you could register required resources and define behavior of what to do when something unpredictable happens during service (re)start.


Validation:
--------------

Validating input data (and filtering output data) is important enough in request lifecycle that it should be powerful feature and integral part of any framework, preferably without much boilerplate code needed to use.  
I think that its obvious enough when I say that relying on validation on ORM layer or similar, is never a good idea - reason being, its not secure.  

Ideally, it could be as simple as calling `validate` method on `route` object:  
```javascript
route.validate({
    type: 'object',
    properties: {
        user_id: {
            type: 'string',
            format: 'uuid'
        },
        username: {
            type: 'string',
            minLength: 3,
            maxLength: 32,
            pattern: "^[a-zA-Z0-9 .'_-]*$"
        },
        email: {
            type: 'string',
            format: 'email'
        },
    }
}, 'body');

route.step(function(req, res) {
     //business logic with safe data
});
```


Documentation, Documentation and Documentation:
------------------------------------------------

It gets quickly frustrating when using undocumented software or worse, API that's behaving different to how it is described in a documentation.  

When developing in very small team, documenting everything manually either in comments or other way should not be that big of an issue with at least a little of discipline.  
However if you have multiple teams working on many microservices in parallel, documentation tends to get out of sync really fast. Its due to developers being lazy as it gets boring for them to repeat themselves writing documentation when you've just finished implementing an endpoint where you already described everything at least once.  

Thats why `serviser` is designed so that the documentation is always automatically synchronized and up to date with the actual implementation.  
  
![openAPI-frontend](https://user-images.githubusercontent.com/7884288/74855341-d7b1fd80-5340-11ea-937f-3efe0c065f0b.png)


Those are the main reasons `serviser` came to a life.
- It does not try to reinvent the wheel but rather uses mature libraries (like `expressjs` and `ajv`) and does its job on top of them.
- all expressjs middlewares are compatible
- Promises are supported natively
- Validation as core feature
- robust approach to Error handling, making sure that service always behaves in predictable manner
- `Open API` (swagger) - API specification is generated straight from code implementation which reflects current implementation.
                     - Specification is then used as single source of truth,
                     - making it possible to generate detailed API documentation for consumers, client SDKs and more, for free, without a need for any extra manual labor  

What experience you have when developing scalable API services? Have you encountered similar issues or different?

More info about `serviser` including documentation and tutorials:
https://github.com/lucid-services/serviser
