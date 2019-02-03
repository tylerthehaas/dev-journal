# December 3rd, 2018

# December 4th, 2018

## isRecommended calendar event

if an event from outlook has a user invited that exists in align (or it might be exists on the outlook users align team), it becomes a recommended 1:1 because there is a relationship between the creator of the event and the invitee of the event.

# December 5th, 2018

## stripe border

Should only be on unreconciled events

# December 10th, 2018

## persona logins

* rihanna1@nas.run
* mark1@nas.run
* jane1@nas.run

password: [click here to see password](https://tannerlabs.slack.com/archives/DDPCBRC91/p1540582298012400)

# December 12th, 2018

## Targeting Psuedo Elements with Emotion

```javascript
const Branding = styled(Container)({
  display: 'flex',
  flexDirection: 'row',
  alignItems: 'center',
  height: '100%',
  '&::before': { content: '" "', display: 'table' }
})
```

if you dont put the quotes inside the quotes in the content property declaration the whole line will be ignored. The alternative is to use template literals which would look like this...

```javascript
const Branding = styled(Container)`
  display: flex;
  flexDirection: row;
  alignItems: center;
  height: 100%;
  &::before {
    content: ' ';
    display: table;
  }
`
```

I think its better to use template strings at this point. There is an editor solution to get intellisense within template strings.

# December 13th, 2018

## Setting up syntax highlighting and intellisense within emotion template strings

1. Install [VS Code Styled Components extension](https://github.com/styled-components/vscode-styled-components).
2. Install [Typscript Styled Plugin](https://github.com/Microsoft/typescript-styled-plugin) with yarn by running `yarn add -D typescript-styled-plugin` or with npm `npm i -D typescript-styled-plugin`.

# December 14th, 2018

## Get all Dates in a Week with Moment.js

```TypeScript
const getDatesToHighlight = (date: moment.Moment) => {
  const startDate = moment(date).startOf('week');
  const endDate = moment(date).endOf('week');
  const dates = [];

  do {
    dates.push(startDate.clone().toDate());
  } while (startDate.add(1, 'days').diff(endDate) < 1);

  return dates;
};

console.log(getDatesToHighlight(moment())
```

# December 17th, 2018

## setState callback

setState takes a callback as a second parameter which is useful for doing things that rely on the new state after setState is called. The new state will be available through `this.state` within the callback. [Read this](https://medium.learnreact.com/setstate-takes-a-callback-1f71ad5d2296) for more details.

## react-datepicker

### customizing the input

You can customize the input element by passing a component to the customInput prop. This will render the html you want like this...

```javascript
class ExampleCustomInput extends React.Component {
  render() {
    return (
      <button className="example-custom-input" onClick={this.props.onClick}>
        {this.props.value}
      </button>
    );
  }
}

...

<DatePicker
  customInput={<ExampleCustomInput />}
  selected={this.state.startDate}
  onChange={this.handleChange}
  highlightDates={this.state.highlightWithRanges}
  placeholderText="This highlight two ranges with custom classes"
/>
```

## Typescript error `mouseevent is not generic`

React's synthetic events are different from DOM events when an event is emitted from reacts system you must use `React.MouseEvent<HTMLInputElement>`. [More Info](https://stackoverflow.com/questions/44764146/why-is-the-mouseevent-in-the-checkbox-event-handler-not-generic).

# December 18th, 2018

## googling for syntax

> So when I see syntax that I'm unfamiliar with, I like to read about it and understand how it works with the rest of the language. The problem is that it can be difficult to "Google" syntax. Seriously... Try Googling the syntax itself as if you didn't know that it's called "destructuring." Pretty tough! So here's my trick. I go to [astexplorer.net](astexplorer.net) and paste in the code that I don't understand. - Kent C. Dodds

# December 19th, 2018

## How to cause a click of one elemnt to trigger the onClick of another element to be called in react

Need to get a reference to the node and then all methods available to HTMLElement's can be called. It would look something like this...

```javascript
class Component extends React.Component {
  triggerInputClick = (_e: React.MouseEvent) => {
    const { dateField } = this.refs;
    dateField.click();
  };

  render() {
    return (
      <div>
        <input
          ref="dateField"
          onClick={onClick}
          defaultValue={dateValue}
          data-testid={`${testid}--display`}
        />
        <Icon
          data-testid={`${testid}--dropdown`}
          handleClick={this.triggerInputClick}
        />
        <Icon data-testid={`${testid}--dropdown`} />
      </div>
    );
  }
}
```

# December 20th, 2018

## Getters and Setters

### Getter

> "The get syntax binds an object property to a function that will be called when that property is looked up." - MDN

This allows some logic to be run every time a property is accessed. This would make the following two code snippets equivilent...

```javascript
const ex1 = {
  prop: 'some property',
  getProp() {
    // ... some logic
    return this.prop;
  },
};
```

```javascript
const ex2 = {
  get prop() {
    // ...some logic
    return 'some property'
  },
};
```

Note: must abide by following 3 rules
  * property identifier must be either a number or string
  * must have exactly 0 parameters
  * only one getter can be set for a particular property

### Setter

> "The set syntax binds an object property to a function to be called when there is an attempt to set that property." - MDN

This allows you to run some logic anytime there is an attempt to bind a value to a given property. This is most often useful when you need to change some other property in conjunction with the property the setter is bound to.

Note: must abide by following 3 rules
  * property identifier must be either a number or string
  * must have exactly 1 parameters
  * only one setter can be set for a particular property
