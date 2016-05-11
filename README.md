# Module 13: Angular

## Overview
This module is a _brief_ introduction to using the [Angular.js](https://angularjs.org/) framework. As your dynamic visualizations grow in complexity, you'll need to keep track of a slew of variables, data sources, and user interactions. Angular provides an incredibly useful structure for tracking information and organizing web applications, but requires that you understand how its components fit together. This module **only scratches the surface** of the framework, but should give you enough grounding to begin building interactive visualizations within its framework. While there's a bit of overhead to get started, **it's worth it**.

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Contents**

- [Resources](#resources)
- [Why Use Angular for Visualization?](#why-use-angular-for-visualization)
- [Model-View-Controller Structure](#model-view-controller-structure)
- [Angular in HTML](#angular-in-html)
  - [Directives](#directives)
  - [Expressions](#expressions)
- [Angular in JavaScript](#angular-in-javascript)
  - [$scope](#scope)
- [Custom Directives](#custom-directives)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Resources
Here are a few resources to get you started using Angular:

- [Angular.js Website](https://angularjs.org/)
- [Angular.js Tutorial](https://www.codecademy.com/learn/learn-angularjs) _(Codecademy)_
- [Angular.js Tutorial](http://www.w3schools.com/angular/default.asp) _(w3schools)_
- [Interactive Angular Activities](http://www.learn-angular.org/) _(learn-angular.org)_
- [Model-View-Controller Architecture](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller) _(wikipedia)_


## Why Use Angular for Visualization?
Before we consider the reasons for using Angular, we'll need to discuss the reasons for using a JavaScript framework _at all_. Unlike a **library**, which _provides a set of tools_ that makes web-development simpler, a **framework** _provides an architecture_ in which you structure your code. A framework helps keep your code organized by specifying sections for common tasks (querying data, manipulating the DOM, etc.). Frameworks often abstract away tasks that are difficult to keep track of, such as the _state_ of an application, event handlers, callback functions, etc.

As your visualizations scale in complexity, it becomes increasingly difficult to keep track of the state of your application -- which gender should be displayed on the chart? Which color encoding is currently being used? How do I move through a story? Did I propagate the data changes to _all_ relevant charts? Tracking this information in a variety of JavaScript variables is error prone and tedious. The Angular framework will enforce a consistent state between your data and your DOM, relieving you of the responsibility of updating the DOM whenever your data or a selection is changed.

## Model-View-Controller Structure
Angular uses a [Model-View-Controller](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller)-like architecture that **abstracts data from visual representations**. The primary purpose of the MVC structure is to alleviate the worry of figuring out when to update the DOM. At it's core, the concept is simple: when the data (`model`) is changed, that change should be reflected on the DOM (`view`). This way, anytime a variable changes that should change the output, that change is made automatically (and without you having to write the code to do it).

A huge advantage of this structure is that your data (`model`) can be connected to multiple different representations on the DOM (`view`s). Now, instead of worrying about whether or not you've properly propagated changes to various DOM elements, all you have to do is make changes to your model, and Angular will take care of the rest.

One way to think about Angular is as an extensive templating system. Given a set of element types and relationships between them, you construct the web-page you want. However, what makes Angular unique is **two-way data binding**. Rather than simply bind data to particular elements and then render them on the DOM, Angular allows you to use the DOM to change the underlying data structure. In other words, you can **use your `view` to change your `model`**.  The [documentation](https://docs.angularjs.org/guide/databinding) depicts the relationship as follows:

![angular two-way data binding diagram](imgs/data-binding.png)

But wait, what about the **C** in **MVC**? The controller performs the necessary business logic for your application. It helps manage fetching and preparing your data (`model`), which may happen repeatedly given your user. I like to think of the controller as sitting between the `view` and the `model`, performing necessary checks on the inputs/outputs as they're developed:

![angular two-way data binding with controller diagram](imgs/data-binding-controller.png)

Importantly, your `controller` will manage the data of your application as part of a variable it will define, `$scope`. More on this below...

## Angular in HTML
Enough theory -- let's get started. The basic structure of an Angular application requires that you define your **application** (`app`), and set up a `controller` for your application. The Angular specific syntax you use define `apps` and `controllers` will span both HTML and JavaScript, but we'll begin by looking at the HTML syntax. Angular provides an **extended HTML vocabulary** that allows you to dictate element behavior by assigning **attributes** within HTML `<tags>`. These attributes all begin with the prefix `ng-`, and are referred to as **directives**.

### Directives
Angular **directives** allow you to signify certain behaviors or styles to HTML elements on your DOM. When an Angular webpage is compiled, angular will identify these tags and manipulate the DOM appropriately. The [documentation](https://docs.angularjs.org/guide/directive) provides this high level description of directives:

>At a high level, directives are markers on a DOM element (such as an attribute, element name, comment or CSS class) that tell AngularJS's HTML compiler (`$compile`) to attach a specified behavior to that DOM element (e.g. via event listeners), or even to transform the DOM element and its children.

Note, when the page is **compiled**, Angular may _"transform the DOM element and it's children"_. This allows you to use concise syntax to render a variety of HTML elements (without even writing JavaScript, yet). In order to signify to the Angular compiler _where_ to manipulate the DOM, we need to declare an Angular application with the `ng-app` directive:

>`ng-app`: Declares an Angular application within the element. You may specify an application name, such as `<div ng-app="myApp">`, but in our first few examples, we don't have any defined applications, so we'll use a nameless app by declaring `<div ng-app="">`.

The second step in defining our MVC system is to declare a **controller** using the `ng-ctrl` directive:

>`ng-ctrl`: Declares the Angular **controller** that manages the data (and corresponding logic) for your application. You can declare your controller in the same element as your application `<div ng-app="myApp" ng-ctrl="myCtrl">`, but in the first few examples, we won't have any defined controllers, so we'll simply state `<div ng-app="" ng-ctrl="">`. While an app may have many controllers for different sections, simpler applications only have one controller.

Once we've established the area of the page for Angular to compile (using `ng-app`) and the desired data-source and corresponding logic (using `ng-ctrl`), we're able to start leveraging a variety of useful directives for declaratively building our DOM. First, we'll introduce the `ng-model` directive:

>`ng-model`: Declares a specific angular **model** you would like to bind to the element. This directive is commonly used with `<input>` elements, as they allow the user to update the underlying model with the given name. For example, you may want an input element to represent someone's first-name, so you could say `<input ng-model="firstName">`. Anytime the user edits the input box, the underlying model `firstName` will be changed, and those changes will be propagated throughout your application.


Here is a simple application in which multiple input elements have been bound to the same model.

```html
<!-- Define an unnamed angular application in your div element. -->
<div ng-app-"" ng-ctrl="">
  <div>
    <input ng-model="text" placeholder="we will always be the same"></input>
  </div>
  <div>
    <input ng-model="text" placeholder="we will always be the same"></input>
  </div>
</div>
```

The result of the app is that, anytime changes are made in one `input` element, those changes are reflected in the other one.

![gif showing input elements changing values](imgs/input-demo.gif)

There are myriad useful Angular directives (a few of which we'll introduce below), and as you progress through your familiarity with Angular, you can even **write your own** directives (which is super cool). We'll now introduce another HTML syntax, commonly used in conjunction with certain directives, called **expressions**.

### Expressions
Another foundational concept of the extended HTML vocabulary Angular provides is the use of **Angular expressions**. _Expressions_ allow you to evaluate the value of an angular _model_ in your HTML element. As we'll describe more in depth below, `model` values are contained within the **scope** of an applications **controller**. In other words, if you had multiple different controllers, the model values could differ across them.

Expressions use **double curly braces** (`{{}}`) to indicate a model that Angular should evaluate. For example, if you had a model `name` that was set to `James`, the expression `{{name}}` would render `James` in your HTML. Here is a quick example. Note, `ng-bind` is similar to `ng-model`, but only renders the value (as opposed to allowing the user to update it).

```html
<!-- Define an unnamed angular application in your div element. -->
<div ng-app="" ng-ctrl="">
  <div>
    <input ng-model="name"></input>
  </div>
  <p>{{name}}</p>
</div>
```

That expression would render the following responsive DOM elements:

![gif showing input elements changing paragraph values](imgs/input-paragraph-demo.gif)

Here's one more exciting directive that will help showcase the value of using _expressions_: `ng-repeat`. This directive allows you to loop through elements **in your HTML**! An `ng-repeat` statement reads like a loop, where you specify a variable and pass in a set of values for that variable to take on. For example:

```html
<!-- Define an unnamed angular application in your div element. -->
<div ng-app="" ng-ctrl="">
  <p ng-repeat="name in ['Jim', 'Sarah', 'Maria']">Hello, my name is {{name}}</p>
</div>
```

That code section would evaluate the value of `{{name}}`, which would iterate through the array `['Jim', 'Sarah', 'Maria']`, creating a new paragraph element for each element in the array:

![image with three paragraphs rendered by the code above](imgs/ng-repeat-demo.png)

While this is a powerful start, the real value of using Angular is the way in which it connects elements in your JavaScript code to elements in the DOM.

## Angular in JavaScript
Don't worry -- Angular isn't just a new way to write HTML code -- it provides a way to translate from elements in your JavaScript code to elements in the DOM. In your JavaScript code, you can create elements such as **applications** and **controllers**, which you can reference in your HTML.

### $scope


## Custom Directives
