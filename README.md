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

Overall, excluding other files, this is how we should be structuring our application. I will be breaking this to simple bits with extra explanation on "why".

Although, before that, I'd like to specify my reason upon this creation:First things first, with an "infrastructure" style guide, you are trying to solve a developer problem. Like said:

> Cardinally and fundamentally, reading code is harder than writing code.

Upon solving this problem, you are giving yourself these favors:

1. **Developing becomes more fun**. No matter how "good" an app and code is documented, it has to be instinctively readable and exciting. Your structure should immediately reflect the purpose of the app and how the gears turn.
2. **Adding features become awesome tasks**. Normally, as you progress through basic and essential features, regardless of its scale, it becomes bothering and annoying. Your app makes its developers feel that it is bloated and heavy; you get confused every now and then. This becomes a very big problem, especially when you are working with a team, no matter what the size. **Fundamentally**, even if you are a *full-stack developer*, your code should be *workable*.

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
		/common
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
/app.core
/...
```

The ```app``` folder contains the main 

## 4. App Core

The ```app.core``` folder contains all *common* or *shared* files (in short, non-specific components) used in the app such as ```services```, ```controllers``` 

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
