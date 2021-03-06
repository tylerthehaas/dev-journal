# November 12th, 2018

## impressions from align-web readme

```
> * All interactive elements should be given an ID QA can use for automation.
> > * Format `__test__{component-name}--{descriptive-name}` i.e `__test__save-cancel-header--primary-button`
```

* Would it be better to do this as data attributes?
  * Advantages:
    * more explicit as to what it is for
    * doesn't require multiple id's if you need an id for your implementation
    * It's inline with what the community is following based on what facebook has done with react natives testID prop
  * Disadvantages:
    * requires a change in convention on the team
    * requires more indepth knowledge on working with css selectors though this has a low learning curve


* Can we start using lodash-loader to decrease bundle size from pulling in all of lodash?

* Love that enums are preferred to string literals

* Discriminated Union
  * There is some code that makes use of these especially when creating 
  * for explanation of how this works see: [Discriminated Unions]("https://www.typescriptlang.org/docs/handbook/advanced-types.html#discriminated-unions")

# November 13th, 2018

## [Git Tagging](https://www.atlassian.com/git/tutorials/inspecting-a-repository/git-tag)

for a wip tag do: `git tag wip`

## updating translations

`en_us` is treated as the master file. If a translation is in en_us and not the others it will add it (with the english text), and vice verse - if there is a translation in the others that isn’t in en_us it will remove it

That happens in the `alphabetize-json.js` file, it runs with the `yarn prettier` script

to sync everything up just run `npm run alpha:translations` (this script is also included in `npm run prettier` script)

# November 14th, 2018

## How promises are handled in redux store

redux-promise-middleware is used to dispatch pending, fulfilled and rejected versions when it receives a message that has a promise in its payload. [redux-promise-middleware](https://docs.psb.codes/redux-promise-middleware/)

## Removing v0/one-on-ones/:oooid/goals

[initial state](https://www.draw.io/?lightbox=1&highlight=0000ff&nav=1&title=v0%20goals%20removal#R7VhdU%2BIwFP01PLrTprbooyiwO%2BOOzvKgPoY2ttkNvd005cNfvzc0pS0BQWWRB2ccm5zcmzTnHG4CHe96Mh9KmiU%2FIWKiQ5xo3vFuOoQQz%2FfxoZFFibjk%2FKJEYskjg9XAiL8wAzoGLXjE8lagAhCKZ20whDRloWphVEqYtcOeQbRXzWjMLGAUUmGjDzxSSYle%2BE6Nf2c8TqqVXceMTGgVbIA8oRHMGpDX73jXEkCVrcn8mgnNXsVLmTfYMrp6MclStVeCyZhSUZjNTTFjACk7gxT%2FkGadfAUAmEMGMVCBUCBw%2Bt5Y6i2oheEl%2BFvo9%2B49Q6rO8qVqVxhASIbS9%2BpxbMXlk7TyCYuQZtMFqRKIIaWiX6M9CUUaMf3yDvYSNRHYdLHJ5lw9avgb8U33qRpKlVw0x3T%2FyUzxmym1MA6jhQKE6pVvATIzSQKSv%2BC%2BqDCJepMmT8vWs5k31OZQyNBskBizUhkzE%2BWVkN56I82oNWQwYfi2GCCZoIpP2w6kxsjxKq7WGhtG7i3SE0v6Sle9tw8pO3U3eUgbqFwA%2B%2BUalRFO2E%2FdV%2FzU%2FfLTyk%2B2nR7YuKnsFpHaEswSrtgoo8sdzvD8aMuylZUpk4rNX92xGfUCc%2FyY06cqxbO6krsVljSqeOB8nKPL45e8dzs0R1epK31aIhAKmuc8rOABFzsVafrUs30afFrdc%2F6PCK%2FViVMVwf80ETyrWtzyXB86Q7xg4OPHzR6l4wC6uQ3RGjo9NjWsx%2FaVbXMxX6P5PYKRD%2BqzTEU%2F0UUjIAOeqrwx870G6ooZeE6rYgbna5fKHfG%2Bs2aN8gVqo6x2sp93zi3v3FBFlxd5fYEY8TRG8S373KUIOpDiv2XzGPZyttvL%2BbKXma%2F7Nnutxx%2FYXnZl%2BkV5%2BzvPid5kAveIVxnfoul%2BaDGCX28z3QwXgiM1cjct45LD2%2FEKoOGfeMnsXaFwFnY4%2Fny%2FbaTLDVfBYAN9FwegL7DoG9iGOm36SPdo9GG3%2Fk2k%2FEzXPy15%2FX8%3D)

we need to just remove the call to `v0/one-on-ones/:oooid/goals` in order to do so we I have to trace where the attachedContent prop in AttachedSection component is getting its data from. 

currently calling `getGoalsAttachedToOneOnOne` which then dispatches an action to set goals based on their type when the call is fulfilled.
I can try to replace the call to  `one-on-ones/:oooid/goals` with `v1/one-on-ones/:id` within the `getGoalsAttachedToOneOnOne` fn. then I could pass `data.agendaContent` to the `transformGoals` fn. I would just need to figure out how align is getting agendaContent.type currently. I will look into that tomorrow.

# November 15th, 2018

## Removing v0/one-on-ones/:oooid/goals continued

This endpoint was not used in the rendering of any part of the UI. I was able to remove that and all referenced redux actions safely.

## Deleting remote git tags

`git tag -d [tagname]`

## Calendar Web: Calendar tab and Calendar Settings

### Location of tests for proper routing

It doesn't look like there is anything currently testing that routing is done correctly when a link is clicked. But it seems to make sense that this should be done in component test files.

### Testing that navigation works

1. create store with call to createStore passing to it the reducer from our store.
2. use the Provider component from react-redux and set the store prop to the store created in step 1.
3. Create history with a call to createMemoryHistory from the history library and pass an an object with a key of initialEntries set to a value of an array containing initialEntry strings.
4. Call the Router component from react-router as a child to the Provider component and pass your history to its history prop.
5. Call your apps base component as a child to Router.
6. make any assertions you need.

When your done it should look something like this

```
  it('renders the calendar component when I navigate to those pages', () => {
    const store = createStore(reducer);
    const history = createMemoryHistory({ initialEntries: ['/'] });
    const { getByTestId, queryByTestId, getByText } = render(
      <Provider store={store}>
        <Router history={history}>
          <App {...appProps} />
        </Router>
      </Provider>,
    );
    expect(getByTestId('home')).toBeInTheDocument();
    expect(queryByTestId('calendar')).not.toBeInTheDocument();
    fireEvent.click(getByText(/calendar/i));
    expect(queryByTestId('home')).not.toBeInTheDocument();
    expect(getByTestId('calendar')).toBeInTheDocument();
  });
```

This doesn't quite work though because I am rendering just the app. where the app renders it like this....

```
  <Provider store={store}>
    <ThemeProvider theme={cssVariables}>
      <Router history={history}>{routes}</Router>
    </ThemeProvider>
  </Provider>,
```

If I mirror that it should fix my problem.

# November 16th, 2018

## Calendar Web: Calendar tab and Calendar Settings

### Testing that navigation works continued 

That worked except that firebase auth isn't being set correctly in the redux store causing an invariant violation. I may need to mock out the call to firebase auth to get this working correctly. This would require a more in depth knowledge of how firebase auth works within align. I'll work on that Monday.

## migration off v0/one-on-ones/:oooid/goals

Turns out the redux action I removed triggers a bunch of other calls that are necessary to be made somehow. This is triggered because AttachedSection component calls the getGoalsAttachedToOneOnOne action inside its componentWillMount lifecycle hook. This seems like a bad setup to me because a redux action shouldn't trigger unrelated calls to be made ( poor seperation of concerns ) but I will need to dig into this on Monday as well.

# November 18th, 2018

## Testing that navigation works continued

I successfully got past the error with firebase auth by mocking call to `auth.loadAuth`. The problem I am running into now is that without authorizing firebase we are stuck in a loading state so that is all that will get rendered in the test. I'm not sure this is solvable with the way that our app works currently so I think that this part is not testable at least until we are off firebase.

# November 19th, 2018

## migration off v0/one-on-ones/:oooid/goals continued

the v0/one-on-ones/:oooid/goals endpoint is only used to set and retrieve attached-section data. The v1/one-on-ones/:id endpoint doesn't have any functionality to fill in for what the deprecated endpoint was doing.

# November 20th, 2018

## disappearing sidebar

There is a regex that determines which routes should have the sidebar. This is located at `src/reducers/app.js` in the `isOverview` key of the `handleActions` call.

Got testing working to my satisfaction by creating a createProvider helper function. Tomorrow I will finish writing tests for Empty state.

# November 21st, 2018

## empty state

There is an `Empty` component that renders our empty states.

## side widgets

The `AttachedSection` component handles all side widgets. This is currently mostly used for goals.

## Home view

The home view is rendered by the `Schedule` component. I added a prop to control whether or not to render unscheduled team members.

## React-Testing-Library portals

It is currently very hard if not impossible to test that a modal appears without testing implementation details. I want to look into this further but for now I will just test that the function that renders the modal is called. I need to post a question on slack and the testingjavascript community channel with a code example. I will do this by looking at `components/modal` which uses `dialog` from `material-ui` sometime by Saturday.

## Calendar work

I was able to get empty state and list view working. This is really good progress. I feel much better about hitting our calendar deadline on the 30th.

# November 26th, 2018

## [.netrc purpose](https://ec.haxx.se/usingcurl-netrc.html)

# November 28th, 2018

## [SSCCE](http://sscce.org/)

## testing modal appears

I was able to get this to work as an sscce on [codesandbox](https://codesandbox.io/s/v38m2x8o55). After further investigation I think my problem before had to do with the props I was passing into the test. I went down this path but decided it wasn't necessary because this functionality is being tested in other tests. I suspect I will revisit this problem sometime later

## connect to outlook

change from the `Link` component to use the `Button` component and call the `connectOutlook` service `onClick`.

