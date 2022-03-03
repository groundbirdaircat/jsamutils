# jsamutils
Front end utilities that I commonly use, packaged as an ESModule.


# Index

* [Get Started](#get-started)<br>
* [Color Log](#color-log)<br>
* [Color Error](#color-error)<br>
* [Wait Timeout](#wait-timeout)<br>
* [Element Things](#element-things) (3)<br>
* [Randoms](#randoms) (3)<br>
* [Converters](#converters) (3)<br>
* [Calculations](#calculations) (2)<br>
* [noResizeTransition](#noresizetransition)<br>

# Get Started
You can import this module just like any other ESModule.
```javascript
import { color, wait, rand } from './filepath/jsamutils.js'

// or

import * as utils from './filepath/jsamutils.js'
```
Available imports:<br>
[`color`](#color-log), [`wait`](#wait-timeout),<br>
[`div`](#div), [`create`](#create), [`el`](#el), <br>
[`rand`](#rand), [`randFloor`](#randfloor), [`randBool`](#randbool),<br>
[`pxToVmax`](#pxtovmax), [`pxToVmin`](#pxtovmin), [`msToTime`](#mstotime),<br>
[`angle`](#angle), [`distance`](#distance),<br>
[`noResizeTransition`](#noresizetransition)

# Color Log
`color.log` adds some style to your browser console logs.<br><br>
To use, simply log as you normally would, and then add a string as the last parameter with the styles you want, seperated by spaces.
```javascript
color.log('Hi', 'green big')
```
Color Log uses groups. For inline logs, such as one the example above would produce, you can click on the log to expand the group and expose the trace for that log.<br><br>
Color Log will concat your log items to one line, as long as all items are strings or numbers.<br><br>
If any logged items are not a number or string, it will log an expanded group, containing a normal log (to preserve normal object/array console capabilities). The trace will be displayed underneath as an expandable sub-group.<br><br>

Available styles:<br><br>
Colors: `'red'` `'orange'` `'yellow'` `'green'` `'blue'` `'purple'`<br>
Sizes:  `'small'` `'big'` `'huge'`<br>
Other:  `'nobg'` (removes background)<br><br>

You don't have to pass styles, and in that case, a default styling will be used.<br><br>
Keep in mind that if the last passed parameter is a string that just happens to contain any of these style keywords, that string will not be logged, all valid styles in that string will be used, and the rest of that string will be ignored.<br><br>

# Color Error
`color.err` allows you to label your errors, and applies a default style.<br><br>
It accepts 1 or 2 parameters, the first being the error label, the second being the error.<br><br>
If only 1 parameter is passed, it will be used as the error.<br><br>
If the (error) parameter passed is not an `Error` object, one will be created, using the passed argument as the message.<br><br>
The logged message is a group that can be clicked to reveal the trace stack.

```javascript
// these are all valid ways of using color.err

color.err('this is the error')
color.err('error label', 'this is the error')

try {
    // ...
}
catch(error){
    color.err(error)
    color.err('error label', error)
}
```
Note: `color.err` always uses default styles `'red'` and `'big'`, so there is no need to pass any styles.

# Wait Timeout
`wait` is a shortcut for `setTimeout` that reverses the order of the parameters, and returns a `clear()` function.<br><br>
`wait` accepts 1 or 2 parameters.<br><br> 
If only one parameter is passed, it must be a function, and will be treated the same as `setTimeout(fn, 0)`<br><br>
If two parameters are passed, the first will be used as time (in ms), and the second must be a function.<br><br>
When assigning `wait()` to a variable, `wait` will return an object with a single function: `clear()`<br><br>
If `clear` is called on the function before the `wait` time runs out, it will clear the timeout.<br><br>
Any following calls to clear will print an error to the console, notifying you that the timeout has already been cleared.<br><br>
If the function was already executed, `wait` will also print an error to console if you try to clear.
```javascript
var timeout = wait(100, ()=>color.log('function executed'))
timeout.clear() // timeout clears
timeout.clear() // error: can't clear, timeout already cleared
```
```javascript
var timeout = wait(100, ()=>color.log('function executed'))
wait(101, timeout.clear) 
// ... 100ms passes
// log: function executed
// error: can't clear, function already executed
```

# Element Things
### div
`div` is used to create a div element.<br><br>
It accepts 0 - 2 parameters. <br><br>
The first parameter is a string that contains the css classes that you want to add to that element.<br><br>
The second parameter is the element that you want to append the div to.<br><br>
If no second parameter is passed, appending the div will be up to you.<br><br>
If no parameters are passed, a simple div, (empty classList, not appended) will be returned.
```javascript
var newDiv = div('cssClassesHere', elementToAppendTo)
```

### create
`create` is similar to `div` except that it requires a string as a first parameter, indicating what kind of element you want to create.<br><br>
The second parameter is optional, and should be a string containing the css classes you want to add to the created element.<br><br>
The third parameter is optional, and should be an element that you want to append the created element to.<br><br>
If no third parameter is passed, appending the element will be up to you.
```javascript
var newElement = create('img', 'cssClassesHere', elementToAppendTo)
```

### el
`el` is a shortcut for `document.querySelector`.<br><br>
It accepts one parameter, a string identifying the element you want to query.<br><br>
`el` adopts it's functionality from `document.querySelector`, so it works with IDs, and css classes.<br><br>
In the case that there are mutiple items for the query to find, it will only return the first one.
```javascript
var refByClass = el('.someCssClass')
var refByID = el('#someElementID')
```

# Randoms

### rand
`rand` is a shortcut for `Math.random()` with a little bit of added functionality.<br><br>
It accepts 0 - 2 number parameters.<br><br>
If no parameters are passed, it will return a number between 0 and 1.<br><br>
If only one parameter is passed, `rand` will return a number between that number and 0.<br><br>
If two parameters are passed, it will return a number between those numbers.<br><br>
`rand` will not include the larger number, even if the larger number is 0.
```javascript
rand() // 0 to 1, not including 1
rand(-5) // -5 to 0, not including 0
rand(-9, 9) // -9 to 9, not including 9
rand(9, -9) // same, order doesn't matter
```

### randFloor
`randFloor` has the same functionality as `rand`, but with `Math.floor()` applied to it.<br><br>
This causes it to return a random whole number.<br><br>
Essentially, it is `Math.floor(rand())`.<br><br>
Keep in mind that using `randFloor` with no parameters will always return 0, and is thus pointless to do so.

### randBool
You guessed it, `randBool` returns a random `Boolean` value. Impressive, I know.<br><br>
It is literally `Boolean(randFloor(2))`.<br><br>
It will always return either `true` or `false`.

# Converters

### pxToVmax
`pxToVmax` converts a number of pixels to the corresponding vmax value, based on the size of the current window.<br><br>
It requires one number parameter, the number of pixels that you want to convert.

### pxToVmin
`pxToVmin` is the same as `pxToVmax` except that it calculates vmin instead of vmax.<br><br>
It also requires one number parameter, the number of pixels that you want to convert.

### msToTime
`msToTime` converts milliseconds to a string representing the time in English.<br><br>
It requires one number parameter, the number of milliseconds that you want to convert.<br><br>
`msToTime` will only use the applicable time identifiers, so if the time is less than a day, 'days' will not be included.
```javascript
msToTime(9999999) // 2 hours, 46 minutes and 39 seconds
msToTime(9999999 * 24) // 2 days, 18 hours, 39 minutes and 59 seconds
msToTime(90000000) // 1 day and 1 hour
```

# Calculations

### angle
`angle` returns a number, representing the angle between two points.<br><br>
It requires four number parameters `x1`, `y1`, `x2`, `y2`, in that order.<br><br>
The return value will be a number between 0 and 360, with 0 meaning up, and 90 to the right.
```javascript
angle(0, 0, 1, 1) // 45
```

### distance
`distance` returns a number, representing the distance between two points.<br><br>
It requires four number parameters `x1`, `y1`, `x2`, `y2`, in that order.<br><br>
The return value will either be a positive number, or 0 if the points are the same.

```javascript
distance(0, 0, 1, 1) // 1.4142135623730951
```

# noResizeTransition
`noResizeTransition` is a module that prevents elements from animating css transitions during a window resize.<br><br>
It works by listening for the `resize` event, and then adds a css class named `notransition` onto all elements that are added to the module.<br><br>
It removes the class from all added elements 250ms after the window has stopped resizing.<br><br>
For the module to work, you must add the following class to your stylesheet:
```css
.notransition{
    transition: none !important;
}
```
The module exposes 3 functions that you can use, `add()`, `remove()`, and `inspect()`.<br><br>
`add()` requires at least one element as a parameter, but can accept an unlimited amount.<br><br>
After adding an element, it will no longer transition when a user resizes their window.
```javascript
var element = div()
noResizeTransition.add(element)
```
Note: `noResizeTransition` will not work if you have specified a transition using JavaScript. It will only override regular css declarations.<br><br>
`remove()` requires at least one element as a parameter, but can accept an unlimited amount.<br><br>
After removing an element, it will no longer exist in the array of elements to add the `notransition` class to.<br><br>
`inspect()` doesn't accept any parameters, and when called, it will log the array of elements that have been added to the module.
```javascript
var element = div()
noResizeTransition.add(element)
noResizeTransition.add(document.body)
noResizeTransition.remove(element)
noResizeTransition.inspect() // [body]
```
