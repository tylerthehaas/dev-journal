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

#December 14th, 2018

## Get Beginning and End of Week with Moment.js

```javascript
const today = moment();
const from_date = today.startOf('week');
const to_date = today.endOf('week');
console.log({
  from_date: from_date.toString(),
  today: moment().toString(),
  to_date: to_date.toString(),
});

// {
//   from_date: "Sun Apr 22 2018 00:00:00 GMT-0500",
//   today: "Thu Apr 26 2018 15:18:43 GMT-0500",
//   to_date: "Sat Apr 28 2018 23:59:59 GMT-0500"
// }
```
