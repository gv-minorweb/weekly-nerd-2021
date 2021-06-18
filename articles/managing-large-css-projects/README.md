# Applying CSS at large-scale web projects

## Introduction

Front-end development is a fast evolving space with new technologies and 'best practices' emerging all the time. Refactoring old projects is almost inevitable at some point, but how do you structure projects to the best of your ability now, keeping future scaling and manageability in mind?

In this article I'll go over ways to manage CSS specifically for any front-end project at scale. Whether you use React or code everything in vanilla, the techniques and methods described here can be applied to any project.

## Difficulties with CSS

- CSS is declarative: there is no logic to tell developers about the state of a project.
- Global namespace: all CSS lives on the same level, making collissions possible.
- Specificity: CSS uses inheritance, making styles interdependent.

Some of these problems only come to light when working on very large, complex projects or when they become large. That's where a CSS architecture that takes scalability and manageability into account comes into play.

## The right methodology

In 2021, there are dozens of ways to go about setting up and managing your CSS. According to the [2020 State of CSS](https://2020.stateofcss.com/en-US/technologies/methodologies/) survey the most popular ones are BEM, Atomic CSS, OOCSS, SMACSS and ITCSS. I don't necessarily think one is better than another, but ITCSS has worked for me, so in this article I'll go over ITCSS and how you can apply that methodology in your projects.

Also, just as a side note, you can definitely mix and use multiple methodologies together. For example, ITCSS for your project structure and BEM for the naming convention can work very well.

## Introducing ITCSS

> **ITCSS** — which stands for Inverted Triangle CSS — is a fully managed CSS architecture. It’s not a framework or library; there’s nothing to download or install.

It’s a collection of principles and metrics by which developers should group and order their CSS in order to keep it scalable, terse, logical, and manageable.

_— Harry Roberts, [Manage large-scale web projects with new CSS architecture ITCSS](http://www.creativebloq.com/web-design/manage-large-scale-web-projects-new-css-architecture-itcss-41514731)_

ITCSS helps you organise your CSS files in such a way that you can better deal with CSS specifics. A structured CSS system can help us maintain code bases for the foreseeable future no matter how small or big.

One of the key principles is that it separates your CSS codebase to several layers, which take the form of the inverted triangle:

![ITCSS Layers](images/itcss-layers.svg)

_One thing to note is that you are allowed to add/remove/rename layers that you deem fit for your project. I.e. for projects where you use Atomic Design for components, you might decide to not use a `Components` folder._

### Principles of ITCSS

#### Reusability
A good CSS architecture should be flexible and reusable. Therefor classes should be used over IDs, since they can be used multiple times on the same page. IDs are difficult to override, due to high selector specificity.

#### Component based
Components are reusable blocks of code with some logic and styling. They are self-contained and are the building blocks of your project. Frameworks such as React, Vue and Angular all use components and can therefor utilise ITCSS as underlying methodology.

### Layers/directory structure

The numbers in front of the layer name have no meaning . Purely used to sort the directories.

#### `/01_settings`

This layer holds **global** settings and variables for the project and should only contain settings that need to be accessed from anywhere. Things like colors, typography, breakpoints, easings, shadows, spacing, etc. can be defined here.

#### `/02_tools`

Global mixins and functions. Things like headings, responsive font sizing, visibility mixins.

#### `/03_generic`

This is the first layer that generates actual CSS and houses low-specificity, far-reaching styles. Contains things like CSS resets, styles for the html/body, font imports.

#### `/04_elements`

Includes styling for bare HTML elements such as H1-H6, tables, form elements, etc. These come with default styling from the browser, so we redefine them here.

#### `/05_objects`

Class based selectors to define undecorated design patterns and layout (no cosmetics). For example `grid` .

#### `/06_components`

Specific UI components such as an accordion or a slider. As mentioned, you are allowed to add/remove/rename layers that you deem fit for your project. React for example is a component based framework, so in such cases you might want to remove the components layer and keep that separate from the rest of the CSS.

#### `/09_utilities`

High-specificity, very explicit selectors. Includes helper/utility classes with the ability to override anything that goes in a previous layer. For example flexbox helpers, classes to hide elements, etc.


#### Example directory structure:

```
01_settings
└ _all.scss
└ _settings.colors.scss
└ _settings.easings.scss
└ _settings.media.scss
└ _settings.typography.scss

02_tools
└ _all.scss
└ _tools.headings.scss

03_generic
└ _generic.base.scss
└ _generic.fonts.scss
└ _generic.reset.scss

04_elements
└ _elements.headings.scss

05_objects
└ _objects.grid.scss

06_components
└ _components.accordion.scss

09_utilities
└ _utilities.flex.scss

index.scss
```

Then in the index.scss file you'd import all individual files, something like:

```js
// Settings
@import '01_settings/all'

// Tools
@import '02_tools/all'

...

// Components
@import '06_components/components.accordion'

...
```

Using a structure like above keeps your files organised and easy to recognize and manage.

The triangle also shows how CSS is ordered, from generic styles to explicit styles, from low-specificity selectors to more specific ones.



![](images/itcss-key-metrics.svg)

## Resources

*Use the following resources to get a better understanding of ITCSS:*

- [ITCSS: Scalable and Maintainable CSS Architecture](https://www.xfive.co/blog/itcss-scalable-maintainable-css-architecture/) - ITCSS explained by XFive.
- [Manage large CSS projects with ITCSS](http://www.creativebloq.com/web-design/manage-large-css-projects-itcss-101517528) – ITCSS introduction by Harry Roberts (the original article republished from the print version, missing are shorter columns on the specificity graph and preprocessors)
- [Manage large-scale web projects with new CSS architecture ITCSS](http://www.creativebloq.com/web-design/manage-large-scale-web-projects-new-css-architecture-itcss-41514731) – ITCSS introduction and interview with Harry Roberts
- [Journey to Enjoyable, Maintainable Styling with React, ITCSS, and CSS-in-JS](https://medium.com/maintainable-react-apps/journey-to-enjoyable-maintainable-styling-with-react-itcss-and-css-in-js-632cfa9c70d6)