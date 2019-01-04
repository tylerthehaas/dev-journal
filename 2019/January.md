# January 2nd, 2019

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

# January 3rd, 2019

## cannot call dom events on emotion styled component

When styling an element emotion doesn't make that elements native dom methods such as `click` available. If this is needed you will have to use some other method to style the element such as the style prop or normal css.
