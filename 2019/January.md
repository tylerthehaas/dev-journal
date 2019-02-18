# January 2nd, 2019

## Cannot rotate psuedo element

Today I had an issue getting the following to rotate:

```css
.iconChevronRight:before {
  content: '\e906';
  transform: rotate(180deg);
}
```

Turns out this is problematic because transforms do not work on inline elements and all psuedo elements are inline by default. The solution is to change the display property to `inline-block`. Like so...

```css
.iconChevronRight:before {
  content: '\e906';
  display: inline-block;
  transform: rotate(180deg);
}
```

# January 3rd, 2019

## cannot call dom events on emotion styled component

When styling an element emotion doesn't make that elements native dom methods such as `click` available. If this is needed you will have to use some other method to style the element such as the style prop or normal css.

# January 4th, 2019

## using react context api

I came across an issue yesterday where I had to update the state of a parent from a child component. At first I thought that using the context api was the right solution for this. After implementing it that introduces complexities that are unnecessary and context is not intended to solve this problem. The right way to solve this is to keep all updater methods in the parent component and pass down the updater functions to the child via props and simply have the child component call those functions. This has the advantage of keeping the components simpler. 

# January 10th, 2019

## adding canvas support to jsdom

[canvas support docs](https://github.com/jsdom/jsdom#canvas-support)

By default jsdom doesnt support canvas methods available in the browser. Using the canvas package you can add these methods. This works because jsdom has some code that looks to see if the canvas package is available inside `node_modules/jsdom/lib/jsdom/utils.js`. If it is available then it will import the package which adds the canvas api to jsdom. 

# January 14th, 2019

# programmatically altering stylesheets

The stylesheet can be altered using the [CSSOM](https://developer.mozilla.org/en-US/docs/Web/API/CSS_Object_Model).

# January 15th, 2019

## vh, vw

These are relative units that refer to 1/100th of the height and width of the viewport respectively.

## display files and folders vertically in terminal

`ls -al` will show the ownership, last modified date, and a lot more. If you want just the name of the file `ls -1a` will do that which is what I want most the time. I aliased `ls -1a` to `ll`.

# January 16th, 2019

## authorizing with swagger

1. go to align app and open dev console
2. paste the following snippet

```javascript
indexedDB.open('firebaseLocalStorageDb').onsuccess = (e) => {
   e.target.result.transaction(['firebaseLocalStorage']).objectStore('firebaseLocalStorage').openCursor().onsuccess = (e) => {
   console.log(e.target.result.value.value.stsTokenManager.accessToken);
   }
}
```

3. copy logged accessToken
4. click authorize
5. paste Bearer and copied accessToken into value field

# January 19th, 2019

## composing styles with emotion

you can compose styles using the css function from emotion/core like so...

```javascript
const FormStyles = css`
  margin: 0;
  order: 1;
  padding-right: 3.75rem;
  width: 60%;
`;

const StyledCheckout: StyledComponent<
  FormikConfig<FormikValues>,
  { form: boolean },
  {}
> = styled(Formik)`
  max-width: "none";
  padding: 0;
  width: 100%;
  ${props => props.form && FormStyles};
`;
```
## using context api with hooks

[React context api with hooks](https://tinkerylabs.com/react-context-api-with-hooks/)

# January 21st, 2019

## scoping searches in react-testing-library

If you need to make sure that the text you are searching for appears in a certain part of a component that can be done using dom-testing-library.  To do that just pull in the query you want to use from dom-testing-library. That will take a container as its first argument that you want to search in. you can then render using react-testing-library and then find the element you want using querySelector. it would look something like the following:

```javascript
import { getByText } from 'dom-testing-library';
import React from 'react';
import { Route } from 'react-router';
import { render } from 'react-testing-library';

import { createProvider } from '../../test-utils/create-provider';

const { container } = render(
  createProvider(
    <Route path="/" component={AddTeamMembersContainer} />,
    initialState,
  ),
);

const header = container.querySelector('header');

expect(
  getByText(header, new RegExp(`addTeamMember.${expected}`, 'i')),
).toBeInTheDocument();
```

It is necessary to use dom-testing-library for this because react-testing-library auto binds the container returned from render as the HTMLElement used in the first argument to the query from dom-testing-library.

# January 22nd, 2019

## What it means to hydrate

[What does it mean to hydrate an object?](https://stackoverflow.com/questions/6991135/what-does-it-mean-to-hydrate-an-object)

## testing components with route params from react-router

This can be done by wrapping the component in Router and Route components from react-router. Then set the path prop to include the route param that your component looks for. also set the location prop with an object that contains a pathname matching the format you set on the path prop. It would look something like:

```javascript
function testEmptyPanelParagraphVocab(
  initialState,
  expected,
  pathname = '/team-members/004ae615-0d1c-4a9e-a6c9-e89c11b0558f/one-on-ones',
) {
  const hydratedInitialState = set(initialState, 'firebase.allUsers', [
    {
      createdAt: 1538521526551,
      email: 'fermium0319bde6@octanner.mailinator.com',
      firstName: 'User-Store',
      lastName: 'Test-Created',
      modifiedAt: 1544133846474,
      id: '004ae615-0d1c-4a9e-a6c9-e89c11b0558f',
    },
  ]);
  const history = createMemoryHistory({ initialEntries: [pathname] });
  const { getByText } = render(
    createProvider(
      <Router history={history}>
        <Route
          path="/team-members/:employeeId/one-on-ones"
          location={{ pathname }}
          render={props => <ProfileOneOnOnesContainer {...props} />}
        />
      </Router>,
      hydratedInitialState,
    ),
  );

  expect(
    getByText(
      new RegExp(
        `afterYouHaveAligned.${
          pathname === '/me' ? 'manager' : 'employee'
        }.${expected}`,
        'i',
      ),
    ),
  ).toBeInTheDocument();
}
```

# January 25th, 2019


