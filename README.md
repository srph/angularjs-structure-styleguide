angularjs-structure-styleguide
==============================

A personal style on structuring your AngularJS. Personal opinion, LIFT principle.

## Table of Contents

1. Context
2. App Overview
3. App
4. App Core
5. App Components
6. Overall
7. Recommendation & Tips

## 1. Context

This style guide has been largely influenced by [John Papa](https://github.com/johnpapa/)'s [AngularJS Style Guide](https://github.com/johnpapa/angularjs-styleguide#application-structure-lift-principle) (which also includes a lecture with regards to an AngularJS app structure).

I will be breaking this to simple bits with extra explanation on "why".

Although, before that, I'd like to specify my reason upon this creation: With an "infrastructure" style guide, you should solve a scalability and maintainability problem.

Like said:

> Cardinally and fundamentally, reading code is harder than writing code.

Upon solving this problem, you are giving yourself these favors:

[1] **Developing the app, ulteriorly, becomes more fun**.

No matter how "good" an app and code is documented, it has to be instinctively readable and exciting. Your structure should immediately reflect the purpose of the app and how the gears turn.

[2] **Adding features become awesome tasks**.

Normally, as you progress through basic and essential features, regardless of its scale, it becomes bothering and annoying. Your app makes its developers feel that it is bloated and heavy; you get confused every now and then. This becomes a very big problem, especially when you are working with a team, no matter what the size. **Fundamentally**, even if you are a *full-stack developer*, your code should be *workable*.

## 2. App Overview

### First-level simplification

```
/app
/app.components
/app.core
/less|sass
```

### Extended

```
/app
  /user
    /partials
    /directives
    /controllers
    /services
  /user-create
  /user-edit
  /newsCategory
  /newsCategory-create
  /newsCategory-edit
  app.module.js
  app.bootstrap.js
/app.components
/app.core
  /controllers
  /resources
  /services
  /utils
  app.core.js
  app.controllers.js
  app.directives.js
  app.resources.js
  app.services.js
  app.utils.js`
/less|sass
```

## 3. App

```
/app
  /user
    user.state.js
    user.controller.js
    user-picture.tpl.html
    user.html
/...
```

I will use the word ```state``` to refer to each page|whatsoever, and ```route``` to the path (URL) of the state.

The ```app``` folder contains all the app's states. And each state may contain the controllers, directives, services which will be used specifically for the page.

**Q: What if I started having more than 1 partial, controllers, and other things?**
If the state starts to use more than 1 partial, this is when you start grouping them to a folder. If the state starts to have more than 2 controllers, you're doing it wrong. Take advantage of the directives, the ```controllerAs``` syntax, and isolated scope. For example:

```
/app.core
  /user
    /partials
      user-picture.tpl.html
      user-swag.tpl.html
    /webcam.directive
    /uploaderThing.directive
    user.state.js
    user.controller.js
    user.html
/...
```

**Q: What if I have nested states?**

As much as possible, I avoid nested directories of states. For example, we have this state hierarchy:

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

This is how we structure our directory. Nests are easily read while adhering to the LIFT principle.

```
/app
  /news
  /news.create
  /news.edit
  /news.delete
  /news-category
  /news-category.edit
```

Use ```.```(dot) for (possibly nested)states of the same module. Otherwise, use ```-```; helpful for states with a url(news/category) that's simply nested under a different state.

To elaborate, here's an example. We have a news CRUD [url: ```/news/*```] and a news category CRUD [url: ```/news/categories/*```]. Then, we expected to have these states:

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
/app
  /news
  /news.index
  /news.edit
  /news-category.index
  /news-category.create
  /news-category.edit
```

**Q: What if my state is composed of two words?**

Use **StudlyCase**; do not separate it with a ```-```(dash).

```
/user
/user-awesomeName
/user-anotherModule
/user-maybeAnotherModule
```

## 4. App Core

```
/app.core
  /controllers
  /services
```

The ```app.core``` folder contains all *common* or *shared* files (in short, non-specific components) used in the app such as ```services```, ```controllers```.

## 5. App.Components

```
/...
/app.components
  /stickyNavThatDoesThat
    /stickyNavThatDoesThat.controller.js
    /stickyNavThatDoesThat.directive.js
    /stickyNavThatDoesThat.provider.js
    /stickyNavThatDoesThat.module.js
  /awesomeProgressBar
    /awesomeProgressBar.controller.js
    /awesomeProgressBar.directive.js
    /awesomeProgressBar.provider.js
    /awesomeProgressBar.module.js
  app.components.module.js
```

The ```app.components``` folder contains mostly general-solution|non-feature-specific ```directives```. I had decided to separate directives because 

1. Most general-solution directives has its own separate controller and provider.
2. Directives are way too large to be nested inside the ```app.core``` folder, and this easily goes against the LIFT principle.
2. If you are familiar of ReactJS, directives are the **Components** of ReactJS (obviously).

## 6. Overall

## 7. Recommendation & Tips

## Contribution

For contribution guidelines, please see this.
