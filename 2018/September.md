# September 14th, 2018 - UtahJS

## Tommy Edison - Accessibility Technology

## Jen Luker - Accessibility the Sprint Filler Way

Disability occurs because they do not fit their surroundings or the common way that a typical person does things.

### Types of disabilities

Permanent disabilities
Temporary impairments
situational limitations
age-related impairments
multiple disabilities
Health Conditions
Changing abilities

### types of impairment

Visual
Auditory
Physical
Speech
Cognitive
  -> Childhood
  -> old age
Neurological

Accessibility ensures all are able to gain same benefits.

### Parts of Accesibility

Screen Readers
Keyboard Navigation
Network Connection Limitations
Language Complexity
Internationalization
Documentation

### Documentation

#### Writing Documentation for your past self

include accessibility and other best practices in your code examples

### Accessibility is for everyone and its for you!

### Accessibility issues are bugs!

bugs are when someone cannot use your site as intended and that is exactly what happens when you have accessibility issues.

### How to approach an existing app

1. start with your definition of done
2. build accessibility into reusable components
3. fix individual Pages

### Convincing your bosses

15% of world live with a disability
15% more access === 15% more revenue

### Tools

aXe plugin for chrome and firefox
  -> Tells you what needs to be fixed and suggestions on how to fix it
eslint-plugin-jsx-a11y


## Thorsen Lorenz - PWA are they easy now?

### What are they?

apps built with progressive enhancement as a core tenent
Mobile first and responsive
connectivity independent
native app-like performance
data as up to date as possible
secure
installable
discoverable
re-engageable through push notifications
sharable through link to a url

### Service Worker

like a proxy server in your browser

programmable network proxy allowing for control of how network requests are handled.

supports:
  caching
  automatic updates
  background sync
  push notifications

## Daniel Sellers - What We Make Matters

JS one of the most accessible languages in computer science

The tradeoff we make for accessibility in JS is we accept dynamic typing among other characteristics that make it easier to digest and learn. This allows us to incrementally learn features as we need it.

"There is a lot of confusion between where JavaScript ends and the Dom begins"

Open Specification on Github which means we can influence the core language through opening issues.

JS biggest advantage is its community so keep it approachable!
Totally changed my view on this.

## Michele Campbell - Climbing the mountain of CI/CD

## Jonathan Mills - SOLID JavaScript

Design Principles intended to make software 

Single Responsibility
Open Close
Liskov Substitution
Interface Segragation
Dependency Inversion

"SOLID means of forcing us to stop and thing of the right way of doing something."

### Single Responsibility

every module should have a single responsibility as part of the system

"A class (module) should have one reason to change." - Robert Martin

example of this is container/pure functional component architecture in React

### Open/Closed

Modules should be open for extension but closed for modification.

using Object.create we can make copies of an Object that will allow for extension of an existing Object but keep instances of an Object the same thus it is closed for modification.

### Liskov Substitution

Barbara Liskov, mother of OOP, said that if a something inherits from its parent you should be able to substitute a child for its parent and not change functionality.

### Interface Segregation

No client should be forced to depend on methods it does not use.

Keep your interfaces as simple as possible

create interfaces that can be implemented by larger classes/modules to restrict its capabilities

#### Facade Pattern

object that provides a simplified interface to a larger body of code.

### Dependency Inversion

things should be passed in and operate on it not implement the method.

## Justin McMurdy - Liberating your application from Framework Tyranny

support for frameworks are dependent on the companies/ppl donating their time and money to that project

options:
  1. rewrite whole app
  2. use an adapter

### SingleSPA

splits app into microservice type front ends that are glued together through a single-spa-config.

single-spa-config tells the application when each microapp should be rendered.

documentation available at [single-spa](http://single-spa.js.org)

## callbags: a new spec for streams

## 
# September 10th, 2018

## Running yearbook as a BAC recipient locally


