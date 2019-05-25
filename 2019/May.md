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

## talking points migration plan

create a 
