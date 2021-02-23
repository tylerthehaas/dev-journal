# February 22, 2021

## Dealing with optional/undefined values in `monocle-ts`

When working with an optics library such as `monocle-ts` you'll need to deal with possibly `undefined` values. Lets say we had the following structure.

```TypeScript
const State = {
  details: {
    detail?: {
      groups: {id: string}[]
    },
  }
}
```

The problem is that `detail` is possibly `undefined` so trying to access `undefined.groups` will throw a runtime exception. We can handle this case through the use of optionals. To create an optional we first have to create the optional by passing it `getter` and `setter` callback functions.

```TypeScript
import {Optional} from 'monocle-ts'
import {none, some} from 'fp-ts/lib/Option'

const optional = new Optional<Detail | undefined, Detail>(
  detail => (detail ? some(detail) : none),
  () => detail => detail
)
```

Here we are using `some` and `none` from the `fp-ts` library to wrap our detail object if we have one using `some` and if it is undefined return `none`. This helps monocle know whether there is a valid value in the object it is trying to access and should proceed further or stop. This also tells TypeScript that our `optional` takes something that is `Detail | undefined` and maps it to `Detail` so that the types line up correctly. This is great because it prevents us from trying to call properties on `undefined` and getting typescript errors. From there we will need to compose lenses together with something called traversals to get what we want out of our deeply nested object. Lets look at the full example and we'll break it down afterward. 

```typescript
import {Lens, fromTraversable, Optional} from 'monocle-ts'
import {array} from 'fp-ts/lib/Array'
import {none, some} from 'fp-ts/lib/Option'

const details = Lens.fromProp<State>()('details')

const detailAccessor = Lens.fromProp<PriorityDetails>()('detail')

const detailOptional = new Optional<Detail | undefined, Detail>(
  detail => (detail ? some(detail) : none),
  () => detail => detail
)

const detailGroupsAccessor = Lens.fromProp<Detail>()('groups')

const groupsTraversal = fromTraversable(array)<Group>()

const groupIds = Lens.fromProp<Group>()('id')

export const detailIdsAccessor = details
  .compose(detailAccessor)
  .composeOptional(detailOptional)
  .composeLens(detailGroupsAccessor)
  .composeTraversal(groupsTraversal)
  .composeLens(groupIds)
  .asFold()

detailIdsAccessor.getAll(state)
```

The first thing we do is create a Lens with monocles `Lens.fromProp` method to focus on the details property in the object. Next we create another Lens that will give us access to the detail property of an object. Then we use the optional we created above and another Lens `detailGroupsAccessor` to get the groups property of an object.

The next line might look a little confusing. We are using monocles fromTraversable and passing it fp-ts's array constructor to create something that can be iterated over by a Lens or Optional. Don't let this part bog you down just think of it as monocles way of creating a composable array.

once we have that we create one more lens `groupIds` that gets the id property of an object. from there we just need to compose all these lenses and optionals and traversables. So all that is happening is we are creating a function that we assign to `detailIdsAccessor`. To do this we start with the `details` lens compose it with `detailAccessor` at this point we have a function that can take an object that has a details property and inside that a detail property. And we get back an object with getter and setter methods.

This is where it gets really cool. Since we have told TypeScript that detail is possibly undefined we now need to handle that possibility or else we will get a compiler error. But at this point we have a lens that might be undefined and we want to get inside that object if it exists. `composeOptional` lets us compose our `detailOptional` with a lens and return an optional using the getter and setter function we created in the constructor when defining it. once we pass our `detailOptional` function to that it will take the possibly undefined detail property and map it to an actual value. Either an object if it exists or undefined if not (if it uses undefined it causes an early return so no runtime exceptions occur 🎉). Then we are composing our `detailGroupAccessor` lens to turn it back into a lens. Next using composeTraversal and passing it `groupsTraversal` we'll convert the lens into a traversal so we can iderate. Next we'll use `composeLens` again to get the id property for each item in our array. Finally `asFold()` will convert our lens into a fold. You can think of a fold as a reduce function that allows us to call certain getter and setter methods on our traversable. So when we call `getAll` on the last line of our code snippet we will get back just the id's of each member of the groups array.
