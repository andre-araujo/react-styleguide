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

*Exemples: Button, Input, ...*

### Element rules:

- Elements doesn't have subcomponents.
  > Why? Elements should be small and has single responsability, if it needs any subcompenent, it should be a module.

- Every element should repass unnused props to their root element.
  > Why? It increase reusability, repassing remaining props to the root component makes gives them the ability to change html attributes, such as data-attributes, width, height, ...

- Elements shouldn't have margins.
  > Why? Give margins to an element makes it hard to align and reuse.

- Elements should be stateless.
  > Why? Elements should be easy to be controlled by their parents, doing this with a statefull component can be really tricky.

### Element sample:

```javascript
import React from 'react';

function MyElement({
  name,
  ...props,
}) {
  return (
    <p {...props}>
      I am the element {name}!
    </p>
  );
}

MyElement.defaultProps = {
  name: 'default name',
};

MyElement.propTypes = {
  name: string,
};

export default MyElement;
```

### Modules

Modules can use elements to create more complex components, they can have a state and should be easy to reuse, they rearange elements or specific module components, adding margins and modifing to elements, but they should't have margins or gaps on their own.

Modules might have subcomponents.

Exemples: Menu, Dropdown ...

### Modules rules:

*TO DO*

### Features

Features are complex components, each of them **must implement a feature**, they use modules and elements to achieve that, also, they can have subfolders with specific components for that feature.

Features can connect with state managers and they meant to be independent of any other feature.

**If the component does not implements a feature, it should be a [module](#modules).**

Features might have submodules.

Exemples: Authentication, Graphs, Search ...

### Features rules:

*TO DO*

**[Back to top](#summary)**

## Commits

[`Commitizen's cli`](https://github.com/commitizen/cz-cli) is recomended to ensure right commit messages.

**[Back to top](#summary)**

## References and special thanks

This styleguide is strongly influenced by the awesome Bradfrost's [atomic design](http://bradfrost.com/blog/post/atomic-web-design/).

Other references:

- [`Airbnb JavaScript Style Guide`](https://github.com/airbnb/javascript)

- [`Commitizen`](https://github.com/commitizen)

**[Back to top](#summary)**