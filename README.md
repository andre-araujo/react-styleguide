# WIP - React project styleguide

**This document is in "work in progress" state, and may be modified as needed**

[ReactJS](https://reactjs.org/) applications can scale very well, but can be really chaotic in no time.
This styleguide tries to solve some common problems of large react applications.

## Purpose

Create and documentate an opinionated [ReactJS](https://reactjs.org/) style guide for component-based projects.

## Summary

1. [Folder Structure](#folder-structure)
2. [Components](#components)
    1. [Elements](#elements)
    2. [Modules](#modules)
    3. [Features](#features)
3. [Commits](#commits)

## Folder structure

This styleguide covers only `/src` folder.

```
/src
  /components
    /elements
      /ElementName
        ElementName.js
        ElementName.spec.js
        ElementName.stories.js
        ElementName.styles.js
        index.js
    /modules
      /ModuleName
        ModuleName.js
        ModuleName.spec.js
        ModuleName.stories.js
        ModuleName.styles.js
        index.js
    /features
      /FeatureName
        FeatureName.js
        FeatureName.spec.js
        FeatureName.stories.js
        FeatureName.styles.js
        index.js
  /pages
    /my-page
      ...
    /another-page
      ...
    router.js
  /lib
    /stateManager
      /actions
        ...
      /reduces
        ...
      index.js
    /utils
      ...
  /static
    /styles
      reset.css
      another-static-style.css
    /fonts
      my-font.woff
    /images
      my-image.jpg
  index.js
```

**[Back to top](#summary)**

## Components

[react-create-component](https://github.com/andre-araujo/react-create-component) can be used to help component creation.

Each component has the following structure:
```
ModuleName.js
ModuleName.spec.js
ModuleName.stories.js
ModuleName.styles.js
index.js
```

Where:

- `ComponentName.js`
  - The component itself.
- `ComponentName.spec.js`
  - Component's test cases.
- `ComponentName.stories.js`:
  - [Storybook](https://storybook.js.org/), or any other component documentation file, replacing ".stories" if needed.
- `ComponentName.styles.js`: Component styles, it can be any CSS or CSS-in-JS solution.
- `index.js`: Component's index, any HOC should be applied here.

### Elements

In this system elements are the smallest type of component, they should be highly encapsulable, therefore stateless and also shouldn't contain margins, gaps or any style that might interfere with the overall style of the page.

Exemples: Button, Input, ...

### Modules

Modules can use elements to create more complex components, they can have a state and should be easy to reuse, they rearange elements or specific module components, adding margins and modifing to elements, but they should't have margins or gaps on their own.

Exemples: Menu, Dropdown ...

### Features

Features are complex components, each of them should implement a feature, they use modules and elements to achieve that, also, they can have subfolders with specific components for that feature.

Features can connect with state managers and they meant to be independent of any other feature.

Exemples: Authentication, Graphs, Search ...

**[Back to top](#summary)**

## Commits

[`Commitizen's cli`](https://github.com/commitizen/cz-cli) is recomended to ensure right commit messages.

**[Back to top](#summary)**

## References and special thanks

This styleguide is strongly influenced by the awesome Bradfrost's [atomic design](http://bradfrost.com/blog/post/atomic-web-design/).

Other references:

- [`Commitizen`](https://github.com/commitizen)

**[Back to top](#summary)**