# May 20th, 2019

## talking points data flow graph

talking points are written in redux through the following graph:

App.componentWillMount -> actions.auth.loadAuth -> actions.auth.loadMe -> actions.firebase.managedRemindersSub  ->
                                                                          actions.firebase.personalRemindersSub ->  actions.firebase.doSubscribe

doSubscribe will then handle dispatching initial data on page load and update the redux store any time anything in firebase changes.

talking points are read from redux in the following areas:

__Containers__                                            __Components__
* RemindersContainer                                      * Reminders
* ProfileRemindersContainer                               * ProfileReminders
                                                          * RemindersModal

# May 28th, 2019

## multiple elements in an if expression with React

each react if expression can only return one element because each expression must return a call to create element.
for example: 

```        
function Test() {
  return (
    <div>
      <button>button</button>
      {true && (
        
        <div>innerDiv</div>
        <div>innerDiv2</div>
        
        )}
      <button>button2</button>
    </div>
  )
}
```

would attempt to compile but when it hits the second call to create element for innerDiv2 it would get confused where does it put the second call to createElement. Its not a child so putting it as the children argument wouldn't make sense. if you do it as shown below the comma after the first createElement call ends the if expression so the second element would be placed outside the if expression. An error is the only reasonable thing to do.

```
React.createElement(
...
  true &&
    React.createElement('div', null, 'innerDiv'),
    React.createElement('div', null, 'innerDiv2'),
...
);
```

This is another instance where typescript errors suck. when I run this the ts compiler gives me:

`Left side of comma operator is unused and has no side effects.`

Babel's compiler gives me:

```
/repl: Adjacent JSX elements must be wrapped in an enclosing tag. Did you want a JSX fragment <>...</>? (8:8)

   6 |         
   7 |         <div>innerDiv</div>
>  8 |         <div>innerDiv2</div>
     |         ^
   9 |         
  10 |         )}
  11 |       <button>button2</button>
```
