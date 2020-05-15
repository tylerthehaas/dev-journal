# July 9th, 2019

## goal migration

### updating a goal

Currently when we update a goal it does the following:

  1. update the goal in redux
  2. update the goal in firebase
  3. when the firebase update resolves update goals pinned state
  4. reset the goal template.
  5. refetch goals from firebase.
  6. update redux with new goals.

Desired update architecture:
  1. update goal in postgres
  2. reroute to goal list
  3. fetch goals from postgres on GoalList mount

Notes: 
  * `goal/:goalId` endpoint will handle all the diffing so only the parts that you send in the updateBody are changed.
  * will need to include all checklist items in each update
    * to do this we will need to create a goal context in GoalForm, update the context whenever a goal changes and update the goal when GoalForm is saved.
