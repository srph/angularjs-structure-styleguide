angularjs-structure-styleguide
==============================

Basic, personal, opinionated style on structuring your AngularJS, adhering to [John Papa](https://github.com/johnpapa)'s [LIFT principle](https://github.com/johnpapa/angularjs-styleguide#application-structure-lift-principle).

**What is LIFT?**

- ```L```ocating our code is easy
- ```I```dentify code at a glance
- ```F```lat structure as long as we can
- ```T```ry to stay DRY (Don’t Repeat Yourself) or T-DRY

## Table of Contents

1. [Context](#1-context)
2. [Overview](#2-overview)
3. [App](#3-app)
4. [Core](#4-core)
5. [Components](#5-components)
6. [Overall](#overall)

## 1. Context

This was created to offer a *more* in-depth guide for AngularJS developers that want their apps to scale, maintainable, and whatever

**This guide gives emphasis to components or directives** inspired by [ReactJS](facebook.github.io/react/) and [Flux](facebook.github.io/flux/). For more information in regards to this emphasis, check this [article](https://medium.com/@srph/breaking-down-angularjs-to-smaller-components-f2ab70a104d0) I wrote on Medium.

Please, feel free to diverge from this guide.

[Back to top](#table-of-contents)

## 2. Overview

Basically, this is how our app will be.

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
|  |  ├── components/
|  |  |  ├── ThatDirective/
|  |  |  |  ├── i18n/
|  |  |  |  ├── tests/
|  ├── user.create/
|  ├── user.edit/
|  ├── newsCategory/
|  ├── newsCategory.create/
|  ├── newsCategory.edit/
├── components/
├── core/
|  ├── constants/
|  ├── filters/
|  ├── resources/
|  ├── services/
|  ├── utils/
|  ├── app.js
|  ├── bootstrap.js
|  ├── constants.module.js
|  ├── filters.module.js
|  ├── components.module.js
|  ├── resources.module.js
|  ├── services.module.js
├── dist/
|  ├── css/
|  ├── js/
|  ├── images/
|  ├── views/
├── less|sass/
├── vendor/
```
[Back to top](#table-of-contents)

## 3. App

The ```app``` folder contains all the app's states. And each state may contain the controllers, directives, services to be **specifically** used for itself or child states.

```
├── app/
|  ├── user/
|  |  ├── i18n/
|  |  ├── components/
|  |  |  ├── en.js
|  |  |  ├── kr.js
|  |  ├── tests/
|  |  ├── partials/
|  |  ├── user.state.js
|  |  ├── user.controller.js
|  |  ├── user.html
├── ...
```

### Q: What is a state? ###

Used to register a state, see [ui-router](https://github.com/angular-ui/ui-router). This also has been asked before, see [this issue](https://github.com/srph/angularjs-structure-styleguide/issues?q=is%3Aissue+is%3Aclosed).

### Q: What if I started having more than 1 partial, controllers, and other things?

**Stop using multiple controllers, partials, services**. It is more **recommended** to use directives for the sake of modularity and maintainability. It is only okay to use partials for chunk of non-functioning mark-up.

```
├── app/
|  ├── user-profile/
|  |  ├── components/
|  |  |  ├── Avatar/
|  |  |  |  ├── Uploader.js
|  |  |  |  ├── Webcam/
|  |  |  |  |   ├── Webcam.js
|  |  |  |  |   ├── WebcamShootButton.js/
|  |  ├── user.state.js
|  |  ├── user.controller.js
|  |  ├── user.html
├── ...
```

### Q: What if I have nested states?

**As much as possible, try to avoid nested *directories* of states. For example, we have this state hierarchy:**

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

This is how we structure our directory. Why? This way, nested states can be easily found and understood while adhering to the **LIFT** principle.

```
├── app/
|  ├── news/
|  ├── news.create/
|  ├── news-category/
|  ├── news-category.edit/
```

### Q: How do I indicate a url nest or a state nest?

**Use ```.```(dot) for states of the same module**. For instance, if we have the ```users``` *CRUD* module, then we'd have ```users.create```, ```users.edit```, **...**).

**Otherwise, use ```-``` to signify hierarchy**. For instance, we have a ```group``` module which is under the ```users``` module, then we'd have: (```users```, ```users.create```, **...**, ```users-groups```, ```users-groups.create```, **..**).

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

### Q: What if my state is composed of two words?

**Use ``camelCase``; do not separate it with a ```-```(dash).** Do not use ```-```(dash) to signify that a name consists of two words. Use it to signify a nest or hierarchy. If a state consists of two separate words (e.g, soulja boy, sticky nav), use ```camelCase```. For example, ```souljaBoy```, ```stickyNav```.**.

Why? This allows us to properly signify and understand what a dot (```.```) and dash (```-```) does.

```
├── user/
├── user-awesomeName/
├── user-anotherModule/
├── user-maybeAnotherModule/
├── user-thatModule/
├── user-thatModule.index/
├── user-thatModule.create/
```

### Q: Where do I put my tests or i18n?

**Tests and i18n should be put close to our components or state as possible.**

*Why?* This avoids the replication of our structure for our tests; and, makes them easier to view.

### Q: How do I handle each language for the i18n?

**The filename of each i18n should signify only the language it is supposed to handle**. If a component only has one i18n file, simply put it at the same directory it will be used with.

[Back to top](#table-of-contents)

## 4. Core

The ```core``` folder contains all ```modules```, app ```bootstrapper``` (see [```angular.bootstrap```](https://docs.angularjs.org/api/ng/function/angular.bootstrap)), and all *common* or *shared* files (in short, non-specific components) used in the app such as ```services```, ```constants```, ```controllers```, ```resources```, ```utils```, and ```filters```.

```
├── core/
|  ├── constants/
|  ├── filters/
|  ├── resources/
|  ├── services/
|  ├── utils/
|  |   ├── progress.config.js/
|  |   ├── restangular.config.js/
|  ├── app.js
|  ├── bootstrap.js
|  ├── constants.module.js
|  ├── components.module.js
|  ├── filters.module.js
|  ├── resources.module.js
|  ├── services.module.js
```

### Q: What does each of the js file in the *root* of the ```core``` folder do?

- ```app.js``` is our application module, the highest-level module, which contains all other modules. For instance, ```angular.module('app', ['app.resources', 'app.directives', 'app.services', 'app.constants']); ```.
- ```bootstrap.js``` does nothing but bootstrap the app ```angular.bootstrap(document, ['app'])```.
- ```*.module.js``` is the module to be respectively used for each (all *constants* in the ```constants/``` directory should use ```constant.module.js```).

### Q: What are ```resources```?
These come in handy when you frequently request an API for data. Fill in ```resources``` with your $http-service-wrappers to wrap ```$http``` or ```Restangular``` methods, or store data from a server response. Otherwise, put in the ```service``` as a normal service.

## 5. Components

The ```components``` folder contains mostly general-solution|non-feature-specific ```directives```.

```
├── components/
|  ├──  StickyNavThatDoesThat/
|  |  ├── tests/
|  |  ├── StickyNavThatDoesThat.spec.js
|  |  ├── Hamburger.spec.js
|  |  ├── Search.spec.js
|  |  ├── i18n/
|  |  |  ├── en.js
|  |  |  ├── jp.js
|  |  |  ├── kr.js
|  |  ├── tests/
|  |  ├── StickyNavThatDoesThat.js
|  |  ├── Hamburger.js
|  |  ├── Search.js
|  ├── AwesomeProgressBar/
|  |  ├── i18n/
|  |  ├── tests/
|  |  ├── AwesomeProgressBar.js
|  ├── components.module.js
```

### Q: Why is this named as ```components``` not ```directives```?

It took me a long while to decide whether how it should be named. After some time, I preferred to use ```components``` *because* our ```directives``` are actually being used as components. If only the directive API wasn't that bad, everybody could have been doing this because it makes everything easier to predict, to test, and manageable with proper separation of concerns.

### Q: Why are the directives separated from other angular modules?

I had decided to separate directives because:

1. Directives are way too large to be nested inside the ```core``` folder, and this easily goes against the LIFT principle.
2. They need emphasis and will contain mostly a large part of your app

**Write the file names of your directives (components) in StudlyCase**.

*Why? I wanted to emphasize components.* In the future, I might tweak a huge part of the guide to *StudyClase*. *camelCase* does not seem to give proper emphasis to itself, so.

```
|  ├── user
|  |  ├── en.js
|  |  ├── user.controller.js
|  |  ├── user.state.js
|  |  ├── ...
```

[Back to top](#table-of-contents)

## 6. Overall

Do not forget that this is a personal, opinionated structure styleguide. Although I have been using an almost-similar structure in production, your structure will vary on your project (team size, etc) from time-to-time. Make sure to keep it simple.

It is *always* better to create feature-based instead of role-based structure. Because, based on my experience, role-based structure will always be harder to write, read, and maintain.

Like I said in the [Context](#1-context), please feel free to diverge.

[Back to top](#table-of-contents)

## Credits

This style guide has been largely influenced by [John Papa](https://github.com/johnpapa/)'s [AngularJS Style Guide](https://github.com/johnpapa/angularjs-styleguide#application-structure-lift-principle) (which also includes a lecture with regards to an AngularJS app structure).

## Contribution

I'd suggest to issue a proposal or a question first (for discussion purposes) before submitting a pull request. This avoids unrewarded / unmerged pull requests (if ever).

If you have any questions, issues, or whatever, feel free to send me a tweet on [twitter](https://twitter.com/srph), or just submit an issue.
