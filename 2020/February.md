# February 25th, 2020

## My Teams Kickoff

* download report is a link to victories report url
* "recognize now" button opens give widget
* ecards and nominations weighted equally

### Header Test Conditions
* Only displays My Teams tab in header if has team members

### My Team Page Test Conditions

* can visit my-teams route if user has team members
* redirects to home if does not have team members
* doesn't show my teams tab if no team members (consider this case as unauthorized)
* time frame labels stay in sync with selected filtered option
* data updates when filtered option changes
* recognize now option always shows when they haven't received recognition in the selected time frame
* recognize now shows only on hover when they have received recognition
* shows ecards and nominations sent/received as colored bars in appropriate column
* shows avatar and name under name column 
* routes to victories when download report is clicked
* shows my team in header
* shows sent/received ecards for team
* shows comparison between current time filter and previous time period (30, 60, 90) for team ecards
* shows sent/received nominations for team
* shows comparison between current time filter and previous time period (30, 60, 90) for team nominations
* shows number of sent/received bars over 27 when cumulative bars > 30

### My Team Feed Widget Test Conditions

* has My Team label
* shows time frame label for last 30 days
* always shows data for last 30 days
* shows ecards sent/received for team
* shows nominations sent/received for team
* Doesn't appear if user has no team members (unauthorized for my teams)

