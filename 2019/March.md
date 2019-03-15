# March 4th, 2019

## TypeScript: Differentiating Types

[Type Guards](https://www.typescriptlang.org/docs/handbook/advanced-types.html#type-guards-and-differentiating-types)

if you have a parameter you cannot feed a check for a certain type into a variable and have the compiler recognize that if the check is true it has to be of one type or another.

For Example:

```typescript
interface Thing {
    total: number
    percentage: number;
}

function getNumber(thing: Thing | number): { basic: number} {
    const isNumber = typeof thing === 'number';
    const isSingular = isNumber ? thing === 1 : thing.total === 1;
    return { basic: isNumber ? thing : thing.percentage }
}
```

In this code snippet `.total` and `.percentage` would throw errors because typescript would see that those properties don't exist on the `number` type even though we did an explicit check for them.

However, If we moved the check inline it would pass.

For Example:

```typescript
interface Thing {
    total: number
    percentage: number;
}

function getNumber(thing: Thing | number): { basic: number} {
    const isSingular = typeof thing === 'number' ? thing === 1 : thing.total === 1;
    return { basic: typeof thing === 'number' ? thing : thing.percentage }
}
```

In this snippet everything type checks because the compiler is able to recognize that thing has to be a number when the `typeof` check is true. But this is suboptimal because I have to rewrite the check everytime `thing` is referenced. To fix this typescript has a feature called type guards. The way type guards work is that you can take in a parameter to a function and as the functions return type you would cast it to whatever type you expect it to be.

Consider: 

```typescript
interface Thing {
    total: number
    percentage: number;
}


function isNumber(x: any): x is number {
    return typeof x === "number";
}

function stuff(a: Thing | number) {
    const isSingular = isNumber(a) ? a === 1 : a.total === 1;
    return { basic: isNumber(a) ? a : a.percentage }
} 
```

This would typecheck properly. But there is a big problem with casting it in this way. Imagine for a moment that you made a mistake and wrote `typeof x === "string"`. Like so:

```typescript
interface Thing {
    total: number
    percentage: number;
}


function isNumber(x: any): x is number {
    return typeof x === "string";
}

function stuff(a: Thing | number) {
    const isSingular = isNumber(a) ? a === 1 : a.total === 1;
    return { basic: isNumber(a) ? a : a.percentage }
}
```

Here we can tell that the return type clearly cant be a number because we checked that it was a string. Yet when we call isNumber we do not get any warnings that something is amiss. Instead typescript assues that everything is a number even though the code says otherwise. This is a poor approach.

## Todo on Gigg

1. Getting a 502 on `/exchange-token` once mongo stuff is pulled in.
2. need to get long lived token in `Instagram.js`.
3. get facebook userId
4. pass long lived token and userId to `Instagram constructor` as class properties.
5. use token and userId in calls to graph api

# March 5th, 2019

## attachments not pinning to one on ones in IE11

tags: `#attachments`

The issue here is that when an attachment is added a modal pops up to have the user select the manager. When this happens the manager id is added to the attachment. In IE11 the `attachment.managerId` property is getting an empty string as a value.

the attachments info is stored in firebase. So everything that updates the managerId for the attachment would have to happen in some firebase call. 

In chrome (which is working) the only message sent that would alter the redux store with the necessary info is `child added changed`. but in chrome there is a managerId set on initial page load. it doesn't seem to be that way for IE11.

# March 8th, 2019

## Test file upload

tags: `#fileupload #testing #react-testing-library`

create a file using the File api use that to upload the file. See example below.

```javascript
  it('is able to upload csv files', async () => {
    const { container, getByText } = render(
      createProvider(<AddPeople {...props} />),
    );

    const filename = 'mylesgaskin.csv';
    const file = new File(['(⌐□_□)'], filename, { type: 'text/csv' });
    const fileInput = container.querySelector(
      '#__test__add-people--file-upload',
    );

    // button should be disabled
    expect(getByText(/upload/i).disabled).toBe(true);

    // display text should be default text
    expect(getByText(/addFile/i)).toBeInTheDocument();

    fireEvent.change(fileInput, { target: { files: [file] } });

    await wait(() => getByText(filename));

    // button should now be enabled
    expect(getByText(/upload/i).disabled).toBe(false);
    expect(getByText(/upload/i).type).toBe('submit');

    // display text should show file name
    expect(getByText(filename)).toBeInTheDocument();

    fireEvent.click(getByText(/upload/i));

    expect(services.getUploadStatus).toHaveBeenCalled();
    expect(services.uploadUsersFile).toHaveBeenCalledTimes(1);
  });
```

# March 13th, 2019

## Problems testing material ui components.

tags: `#material-ui #testing`

`PeopleInput` is built so that you click on the input change the value and then the focus event causes another modal to appear with suggested people based on text match to the inputs value. When attempting to test this we were unable to find a way to trigger the modal because we cannot determine an event that would cause the modal to appear. we attempted this with both focus and click events on all elements in the dom tree throughout the part of the tree that is created by `PeopleInput`.

# March 15th, 2019

## testing Search component

tags: `#testing #search #add-people`

in testing search it was difficult getting the popup menu to appear after the text was changed. The issue with that ended up being two things. 

1. the text input is debounced
  - This causes the assertion to be called before the Search input renders the popup button. 
  - This is resolved by using wait to delay when the assertion happens

2. the selection of a menu result happens on mouse down rather than click

I was able to get this tested with the following:

```javascript
   it('renders selected users', async () => {
    const { getByText } = render(
      createProvider(<AddTeamMembers {...defaultProps} />),
    );

    fireEvent.change(document.querySelector('[placeholder="peopleSearch"]'), {
      target: { value: 'Barry' },
    });

    fireEvent.focus(document.querySelector('[placeholder="peopleSearch"]'));

    const customMatcher = (content, element) => {
      return (
        /barry<\/span> baz/i.test(element.innerHTML) && /baz/i.test(content)
      );
    };

    await wait(() => getByText(customMatcher));

    fireEvent.mouseDown(getByText(customMatcher));

    expect(getByText(/barry baz/i)).toBeInTheDocument();
  });
```