+++

title = "Serviser framework - introduction"
slug = "serviser-framework-introduction"
date = 2020-03-03
[taxonomies]
tags = ["Node.JS", "back-end", "serviser", "expressjs", "open-api"]
+++

Is writing and maintaining up to date documentation for your APIs a pain for you? Do you miss an environment where it is safe to raise exceptions and not to worry about how they will propagate and be processed?  
What about having out of the box solution for validating complex input and output data structures without it polluting your domain logic?  

These were the main reasons of my motivation to create yet another `Node.js` web framework ([serviser](https://github.com/lucid-services/serviser)).

<!-- more --> 

Minimalist frameworks like [**expressjs**](https://expressjs.com/) do excelent job of providing us with bare-bones web server features and are great starting point for a project.  
However it's left up to the developer to build rigid project foundations.
Developers need to worry about how they are gonna handle all the `async` & `sync` errors and even create bridges between `callback` & `promise` patterns. They need to make sure that internal exceptions get logged and do not leak as part of a response or cause nodejs process to crash.  

A lot of additional setup work needs to be done including solutions for service configuration, data validation, request identification and more.  
Chances are that the developer has a set of middlewares (plugins) that help minimize boilerplate code.  
Although that being very flexible approach, it has considerable disadvantage in it being more error-prone solution as the developer can not avoid reinventing the wheel in terms of integrating everything together.  
Taking more robust approach when laying down project foundations allows for better consistency in terms of behavior and makes the application more reliable and scalable.

The goal of the [serviser](https://github.com/lucid-services/serviser) framework is to provide `out of the box` rigid base for web applications and allow for faster production ready development while keeping the core functionality lightweight with modular, swappable plugin based architecture.  


__Main features:__  

- shell integration 
    - run single process / cluster mode
    - inspect http endpoints or service configuration
    - user defined commands

- Error handling and logging
- Resource management
- Powerful Input and Output data validation
- `OpenAPI` integration
    - API documentation (in form of frontend application) autogeneration from code


Even though the library has been production tested in multiple commercial projects, it has not found its community yet.
Any contributions are welcome and greatly appreciated!  

In the next post we will go through tutorial on how to build simple API service with [serviser](https://github.com/lucid-services/serviser).
