# April 17th, 2020

## crashing server post-mortem

### Problems that lead to this

1. Code reviews weren't thorough enough
    - Need to be checking that all possible network responses are handled.
    - Need to be thinking of the data model within components and pages more.
      - Where are possible runtime exceptions going to happen?
      - What possible states can each piece of data and each piece of state be in?
2. There was too wide of a gap between when the problem was introduced and when we started addressing it.
    - Need to treat test failures with more seriousness
3. Our unit tests aren't comprehensive enough
    - Only testing happy paths instead of all possible code paths.
4. The mesh point of where our app ends and core/victories/initiatives/align/prism begins has become blurred making debugging difficult.
5. Lack of understanding the effects of json parsing errors was ultimate cause.
6. Some growing pains with switching over to nextjs
7. Not having a good process on following up with developers from other teams who are contributing to our code base when test failures are happening in different environments.

### Solutions

1. Code review template that will provide a checklist of things we should be thinking about both when we open a PR and when we are reviewing a PR.
2. Creating tickets to improve the resiliency of our code to unexpected data at the boundaries of our code and making impossible states impossible in our front end code.
3. Work closer with API maintainers to understand the schema or review documentation when it has been generated for us
4. Increase test coverage. Should set minimum code coverage levels to make sure code paths are checked. Would typescript have helped?
5. Make sure the e2e tests are passing before promoting to stage and moving our tickets to done.
6. Find a better way to work with other teams to make sure they are following up when test failures do occur.
7. Find ways to be alerted sooner by SET (shift testing left)
8. In addition to tests writing eslint plugins that catch mistakes that could happen in multiple contexts and not specific to certain business logic or component implementation.

