---
title: A Sass Workflow
layout: base
---
Use Eleventy 2.0.1, Pico.css 2.0.0-alpha1 and Sass 1.64.2 to automatically build CSS files from the SCSS framework. Then, serve Eleventy and refresh the browser to see the modifications.

## Article in the blog of LogRocket
[Building a color palette with CSS: 3 methods](https://blog.logrocket.com/building-color-palette-css/)

### The 60-30-10 design rule
- each set of digits represents a color weight
- primary color is 60% of the space on a page, generally the **background**
- secondary color is 30%, most of the time it's **text elements** over the primary color
- accent color is 10%, like **buttons** and **hyperlinks**
- two concepts:
  - **color harmony**, provides a sense of order and balance
  - **color contrast**, measures how readable or accessible one color is
- color theory principles: 
  - **complementary colors** (2x 180°), creates maximum contrast and stability
  - **analogous colors** (3x 30°), calming and pleasing to the eye

### Creating the color palette with Sass variables
- hue
- complementary (+180)
- rightAnalogous (+30)
- leftAnalogous (-30)

```
//
// Complementary hue can be obtained by adding or 
// substracting 180 from the specified hue amount.
//
@function complementary($hue) {
  @return $hue + 180; 
}
//
// The right analogous neighbor of a given hue 
// amount can be obtained by adding 30 to it.
//
@function rightAnalogous($hue) {
  @return $hue + 30;
}

//
// The left analogous neighbor of a given hue 
// amount can be obtained by subtracting 30 
// from it.
//
@function leftAnalogous($hue) {
  @return $hue - 30;
}

```
- calculate the complementary and analogous hues for the $hue: 150
```
$hue: 150;
$hueAnalogousV1: rightAnalogous($hue);
$hueAnalogousV2: leftAnalogous($hue);
$hueComplement: complementary($hue);
```
- adjusting the HSL parameters for saturation and lightness
```
$primaryColor: hsl($hue 30% 90%);
$secondaryColor: hsl($hueComplement 25% 20%);
$accentColor: hsl($hueAnalogousV1 50% 50%);
```
- darker variations of primary color for borders and separators
- lighter variations of secondary color for captions and labels
```
$primaryDark500: hsl($hue 20% 85%);
$primaryDark600: hsl($hue 20% 75%);
$secondaryLight500: hsl($hueComplement 5% 30%),
$secondaryLight900: hsl($hueComplement 5% 95%),
```
- colors that contrast with the accent color
```
$accentColor: hsl($hueRightAnalogous 40% 40%);
$accentColorLight900: hsl($hueRightAnalogous 40% 95%);
$accentColor2: hsl($hueLeftAnalogous 40% 40%);
$accentColor2Light900: hsl($hueLeftAnalogous 40% 90%);
```
- highlighted box-like element that uses variations of the primary color as its background and border. Class = cp-box
- another box element themed using variations of the secondary color. Classes = cp-box + cp-box--dark
- call to action button. Class = cp-btn