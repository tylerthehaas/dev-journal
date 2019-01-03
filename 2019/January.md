# January 2nd, 2018

## Cannot rotate psuedo element

Today I had an issue getting the following to rotate:

```css
.iconChevronRight:before {
  content: '\e906';
  transform: rotate(180deg);
}
```

Turns out this is problematic because transforms do not work on inline elements and all psuedo elements are inline by default. The solution is to change the display property to `inline-block`. Like so...

```css
.iconChevronRight:before {
  content: '\e906';
  display: inline-block;
  transform: rotate(180deg);
}
```
