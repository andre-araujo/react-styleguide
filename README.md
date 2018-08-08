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
    4. [Subcomponents](#subcomponents)
    4. [Reusing component logics](#reusing-component-logics)
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

#### Element rules:

- Elements doesn't have subcomponents.
  > Why? Elements should be small and has single responsability, if it needs any subcompenent, it should be a module.

- Every element should repass unnused props to their root element.
  > Why? It increase reusability, repassing remaining props to the root component makes gives them the ability to change html attributes, such as data-attributes, width, height, ...

- Elements shouldn't have margins.
  > Why? Give margins to an element makes it hard to align and reuse.

- Elements should be stateless.
  > Why? Elements should be easy to be controlled by their parents, doing this with a stateful component can be really tricky.

#### Element sample:

```javascript
import React from 'react';
import { string, node } from 'prop-types';

function MyElement({
  name,
  ...props,
}) {
  return (
    <p {...props}>
      I am the {name} element!
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

Modules might have [subcomponents](#subcomponents).

*Exemples: Menu, Dropdown, ...*

#### Module rules:

- Modules might have [subcomponents](#subcomponents).
  > Why? There are some modules that makes sense to split in other components as they increase their size, but not every module's component will be reused in other places instead the module itself, check [subcomponents](#subcomponents) section to learn more.

- Modules shouldn't have margins.
  > Why? Give margins to an module makes it hard to align and reuse.

- Modules should use elements if they can.
  > Why? Elements should be reused as much as possible to mantain the consistence of the application, but modules also can change element's styles to make then fit as best as possible.

- Modules can be stateful when necessary.
  > Why? Modules should be easy to be controlled by their parents, but, as they are more complex components, sometimes it needs to have an state.

- Modules should avoid getDerivedStateFromProps to manipulate their state.
  > Why? Read [this article](https://reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html).

#### Module sample:

```javascript
import React from 'react';
import { string, node } from 'prop-types';

import { MyElement } from '../..';

function MyModule({
  title,
  children
  ...props,
}) {
  return (
    <section>
      <h1>{title}</h1>
      <MyElement name="My Element Name" />
      {children}
    </section>
  );
}

MyModule.defaultProps = {
  title: 'My title',
  children: null,
};

MyModule.propTypes = {
  title: string,
  children: node,
};

export default MyModule;
```

### Features

Features are complex components, each of them **must implement a feature**, they use modules and elements to achieve that, also, they can have subfolders with specific components for that feature.

Features can connect with state managers and they meant to be independent of any other feature.

**If the component does not implements a feature, it should be a [module](#modules).**

Features might have [subcomponents](#subcomponents).

Exemples: Authentication, Graphs, Search ...

#### Feature rules:

- Features might have [subcomponents](#subcomponents).
  > Why? Features usually has multiple components inside, and within then, there are a great chance that some components will be created specifically for that feature, check [subcomponents](#subcomponents) section to learn more.

- Features shouldn't have margins.
  > Why? Give margins to an feature makes it hard to align and reuse.

- Features should use elements and modules if they can.
  > Why? Elements and modules should be reused as much as possible to mantain the consistence of the application, features can also change elements and modules styles.

- Features can be stateful.
  > Why? Features are end to end funcionality components, implementing logics and ui behaviour. Check [Reusing component logics](#reusing-component-logics) section to learn more.

#### Feature sample:

```javascript
import React, { Component } from 'react';

import { 
  MyElement,
  MyModule,
} from '../..';

class MyFeature extends Component {
  state = {
    didSomething: false,
  }

  doSomeAwesomeThing = () => {
    // Some code here...

    this.setState({
      didSomething: true,
    });
  }

  render() {
    const {
      didSomething,
    } = this.state;

    return (
      <section>
        <MyModule title="Awesome title">
          <MyElement name="Another Name" />
        </MyModule>

        {
          didSomething && 'I did an awesome thing!'
        }

        <button onClick={this.doSomeAwesomeThing}>
          I will do something!
        </button>
      </section>
    );
  }
}

export default MyFeature;
```

### Subcomponents

*TO DO*

**[Back to top](#summary)**

### Reusing component logics

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