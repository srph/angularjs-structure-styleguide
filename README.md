angularjs-structure-styleguide
==============================

Personal, opinionated style on structuring your AngularJS, adhering to [John Papa](https://github.com/johnpapa)'s [LIFT principle](https://github.com/johnpapa/angularjs-styleguide#application-structure-lift-principle).

**What is LIFT?**

- ```L```ocating our code is easy
- ```I```dentify code at a glance
- ```F```lat structure as long as we can
- ```T```ry to stay DRY (Don’t Repeat Yourself) or T-DRY

## Table of Contents

1. [Context](#1-context)
2. [App Overview](#2-app-overview)
3. [App](#3-app)
4. [Core](#4-core)
5. [Components](#5-components)
6. [Overall](#overall)
7. [Recommendation & Tips](#7-recommendation--tips)

## 1. Context

This was created to offer a *more* in-depth guide for newcomers to AngularJS and programming itself as well. If you would like to see more about the concept behind this guide, please view [John Papa's](https://github.com/johnpapa)'s [AngularJS style guide](https://github.com/johnpapa/angularjs-styleguide).

**This guide gives emphasis to components or directives**.

An "infrastructure" helps you solve not only a scalability problem, but also a maintainability problem. This guide was created to share a newbie's solution with his struggles from his infrastructure, rooting from impractical guides shared in the internets, and inexperience. Like said:

> Cardinally and fundamentally, reading code is harder than writing code.

[Back to top](#table-of-contents)

## 2. App Overview

Basically, this is how our app will be.

** Tests should be put close to our components as possible. **

*Why?* This avoids the replication of our structure for our tests, and also makes them easier to view.

### First-level simplification

```
├── app/
├── components/
├── core/
├── dist/
├── less|sass/
├── vendor/
```

### Extended

```
├── app/
|  ├── user
|  |  ├── partials/
|  |  ├── directives/
|  |  |  ├── ThatDirective/
|  |  |  |  ├── tests/
|  ├── user.create/
|  ├── user.edit/
|  ├── newsCategory/
|  ├── newsCategory.create/
|  ├── newsCategory.edit/
├── components/
├── core/
|  ├── controllers/
|  ├── constants/
|  ├── resources/
|  ├── services/
|  ├── utils/
|  ├── app.module.js
|  ├── app.bootstrap.js
|  ├── core.module.js
|  ├── constants.module.js
|  ├── controllers.module.js
|  ├── directives.module.js
|  ├── resources.module.js
|  ├── services.module.js
├── dist/
|  ├── css/
|  ├── js/
|  ├── images/
|  ├── views/
├── less|sass/
├── tests/
├── vendor/
```
[Back to top](#table-of-contents)

## 3. App

```
├── app/
|  ├── user/
|  |  ├── user.state.js
|  |  ├── user.controller.js
|  |  ├── user-picture.tpl.html
|  |  ├── user.html
├── ...
```

The ```app``` folder contains all the app's states. And each state may contain the controllers, directives, services to be **specifically** used for itself or child states.

### Q: What is a state? ###

Used to register a state, see [ui-router](https://github.com/angular-ui/ui-router).


### Q: What if I started having more than 1 partial, controllers, and other things?

If the state starts to use more than 1 partial, this is when you start grouping them to a folder. If the state starts to have more than 2 controllers, you're doing it wrong. Take advantage of the directives, the ```controllerAs``` syntax, and isolated scope. For example:

```
├── core/
|  ├── user/
|  |  ├── partials/
|  |  |  ├── user-picture.tpl.html
|  |  |  ├── user-swag.tpl.html
|  |  ├── components/
|  |  |   ├── Webcam/
|  |  |   ├── UploaderThing/
|  |  ├── user.state.js
|  |  ├── user.controller.js
|  |  ├── user.html
├── ...
```

**Q: What if I have nested states?**

As much as possible, try to avoid nested directories of states. For example, we have this state hierarchy:

```
- main
  - user
    - user.create
    - user.edit
    - user.delete
    - profile
      - profile.create
      - profile.edit
      - profile.delete
```

This is how we structure our directory. Nested states can be easily found and understood while adhering to the **LIFT** principle.

```
├── app/
|  ├── news/
|  ├── news.create/
|  ├── news-category/
|  ├── news-category.edit/
```

Use ```.```(dot) for (possibly nested)states of the same module. Otherwise, use ```-```; helpful for states with a url that's simply nested under a different state.

To elaborate, here's an example: an app consists of a news CRUD [url: ```/news/*```] and a news category CRUD [url: ```/news/categories/*```]. We expect it to have these states:

```
news (abstract state)
news.index
news.create
news.edit

news-category (abstract state)
news-category.index
news-category.create
news-category.edit
```

This is how we create our directory:

```
├── app/
|  ├── news/
|  ├── news.index/
|  ├── news.create/
|  ├── news.edit/
|  ├── news.category/
|  ├── news-category.index/
|  ├── news-category.create/
|  ├── news-category.edit/
```

**Q: What if my state is composed of two words?**

Use **camelCase**; do not separate it with a ```-```(dash).

```
├── user/
├── user-awesomeName/
├── user-anotherModule/
├── user-maybeAnotherModule/
├── user-thatModule/
├── user-thatModule.index/
├── user-thatModule.create/
```
[Back to top](#table-of-contents)

## 4. Core

```
├── core/
|  ├── constants
|  ├── controllers/
|  ├── resources/
|  ├── services/
|  ├── utils/
|  |   ├── progress.config.js/
|  |   ├── restangular.config.js/
|  ├── app.module.js
|  ├── app.bootstrap.js
|  ├── core.module.js
|  ├── constants.module.js
|  ├── controllers.module.js
|  ├── directives.module.js
|  ├── resources.module.js
|  ├── services.module.js
```

The ```core``` folder contains all app modules, app bootstrapper, and all *common* or *shared* files (in short, non-specific components) used in the app such as ```services```, ```constants```, ```controllers```, ```resources```, ```utils```, and ```filters```.

### Q: Why do we have the ```resources``` folder?
These come in handy when you frequently request an API for data. Use ```resources``` to wrap ```$http``` or ```Restangular``` methods, or store data from a server response. Otherwise, use services.

## 5. Components

```
├── components/
|  ├──  StickyNavThatDoesThat/
|  |  ├── stickyNavThatDoesThat.controller.js
|  |  ├── stickyNavThatDoesThat.directive.js
|  |  ├── stickyNavThatDoesThat.provider.js
|  |  ├── stickyNavThatDoesThat.module.js
|  ├── AwesomeProgressBar/
|  |  ├── AwesomeProgressBar.controller.js
|  |  ├── AwesomeProgressBar.directive.js
|  |  ├── AwesomeProgressBar.provider.js
|  |  ├── AwesomeProgressBar.module.js
|  ├── components.module.js
```

The ```components``` folder contains mostly general-solution|non-feature-specific ```directives```.

### Q: Why are the directives separated from other angular modules?

** Write the file names of your components in CamelCase**.

Why? This guide gives emphasis to components which each of or module contains.

I had decided to separate directives because:

1. Most general-solution directives has its own separate controller and provider.
2. Directives are way too large to be nested inside the ```core``` folder, and this easily goes against the LIFT principle.
3. If you are familiar of ReactJS, directives are the **Components** of ReactJS (obviously).

[Back to top](#table-of-contents)

## 6. Overall

[ ... ]

[Back to top](#table-of-contents)

## 7. Recommendation & Tips

### File / directory naming convention

If a state or a directive consists of two separate words (e.g, soulja boy, sticky nav), use ```camelCase```. For example, ```souljaBoy```, ```stickyNav```.

Do not use ```-```(dash) to signify that a name consists of two words. Use it to signify a nest or hierarchy.

[Back to top](#table-of-contents)

## Credits

This style guide has been largely influenced by [John Papa](https://github.com/johnpapa/)'s [AngularJS Style Guide](https://github.com/johnpapa/angularjs-styleguide#application-structure-lift-principle) (which also includes a lecture with regards to an AngularJS app structure).

## Contribution

I'd suggest to issue a proposal or a question first (for discussion purposes) before submitting a pull request. This avoids unrewarded / unmerged pull requests (if ever).
