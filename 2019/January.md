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
