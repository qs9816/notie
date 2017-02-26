# notie

[![js-standard-style](https://img.shields.io/badge/code%20style-standard-brightgreen.svg)](http://standardjs.com/)

notie is a clean and simple notification, input, and selection suite for javascript, with no dependencies.
demo: https://jaredreich.com/projects/notie

#### With notie you can:
* Alert users
* Confirm user choices
* Allow users to input information
* Allow users to select choices
* Allow users to select dates

![Alt text](/demo.gif?raw=true "Demo")

## Features

* Pure JavaScript, no dependencies, written in ES6
* Easily customizable
* Change colors to match your style/brand
* Modify styling with the sass file (notie.scss) or overwrite the CSS
* Font size auto-adjusts based on screen size

## Browser Support

* IE 10+
* Chrome 11+
* Firefox 4+
* Safari 5.1+
* Opera 11.5+

## Installation

#### HTML:
```html
<head>
  ...
  <link rel="stylesheet" type="text/css" href="https://unpkg.com/notie/dist/notie.min.css">
  <style>
    /* override styles here */
    .notie-container {
      box-shadow: none;
    }
  </style>
</head>
<body>
  ...
  <!-- Bottom of body -->
  <script src="https://unpkg.com/notie"></script>
</body>
```

#### npm:
```bash
npm install notie
```


## Usage

#### ES6:
```javascript
import notie from 'notie'
// or
import { alert, force, confirm, input, select, date, setOptions, hideAlerts } from 'notie'
```

#### Browser:
```javascript
notie
// or
window.notie
```

#### Available methods:
```javascript
notie.alert({
  type: Number|String, // optional, default = 4, enum: [1, 2, 3, 4, 5, 'success', 'warning', 'error', 'info', 'neutral']
  message: String,
  stay: Boolean, // optional, default = false
  time: Number // optional, default = 3, minimum = 1
})

notie.force({
  type: Number|String, // optional, default = 5, enum: [1, 2, 3, 4, 5, 'success', 'warning', 'error', 'info', 'neutral']
  text: String,
  buttonText: String // optional, default = 'OK'
}, callback())

notie.confirm({
  text: String,
  yesText: String, // optional, default = 'Yes'
  noText: String // optional, default = 'Cancel'
}, yesCallbackOptional(), noCallbackOptional())

notie.input({
  text: String,
  submitText: String, // optional, default = 'Submit'
  cancelText: String // optional, default = 'Cancel'
  autocapitalize: 'words', // default: 'none'
  autocomplete: 'on', // default: 'off'
  autocorrect: 'off', // default: 'off'
  autofocus: 'true', // default: 'true'
  inputmode: 'latin', // default: 'verbatim'
  max: '10000',// default: ''
  maxlength: '10', // default: ''
  min: '5', // default: ''
  minlength: '1', // default: ''
  placeholder: 'Jane Smith', // default: ''
  spellcheck: 'false', // default: 'default'
  step: '5', // default: 'any'
  type: 'text', // default: 'text'
  allowed: ['an', 's'] // Default: null, 'an' = alphanumeric, 'a' = alpha, 'n' = numeric, 's' = spaces allowed. Can be custom RegExp, ex. allowed: new RegExp('[^0-9]', 'g')
}, submitCallbackOptional(value), cancelCallbackOptional(value))

notie.select({
  text: String,
  cancelText: String,
  choices: [
    {
      type: Number|String, // optional, default = 1
      text: String,
      handler: Function
    }
    ...
  ]
})

notie.date({
  value: Date,
  submitText: String, // optional, default = 'OK'
  cancelText: String // optional, default = 'Cancel'
}, submitCallbackOptional(date), cancelCallbackOptional(date))
```

#### For example:
```javascript
notie.alert({ text: 'Info!')
notie.alert({ type: 1, text: 'Success!', stay: true) // Never hides unless clicked, or escape or enter is pressed
notie.alert({ type: 'success', text: 'Success!', time: 2 }) // Hides after 2 seconds
notie.alert({ type: 2, text: 'Warning<br><b>with</b><br><i>HTML</i><br><u>included.</u>' })
notie.alert({ type: 'warning', text: 'Watch it...' })
notie.alert({ type: 3, text: 'Error.' })
notie.alert({ type: 'error', text: 'Oops!' })
notie.alert({ type: 4, text: 'Information.' })
notie.alert({ type: 'info', text: 'FYI, blah blah blah.' })

notie.force({
  type: 3,
  text: 'You cannot do that, sending you back.',
  buttonText: 'OK'
}, function () {
  notie.alert({ type: 3, text: 'Maybe when you\'re older...' })
})

notie.confirm({
  text: 'Are you sure you want to do that?',
  yesText: 'Yes',
  noText: 'Cancel'
}, function () {
  notie.alert({ type: 1, text: 'Good choice!' })
}, function () {
  notie.alert({ type: 3, text: 'Aw, why not? :(' })
})
notie.confirm({ text: 'Are you sure?' }, function() {
  notie.confirm({ text: 'Are you <b>really</b> sure?' }, function() {
    notie.confirm({ text: 'Are you <b>really</b> <i>really</i> sure?' }, function() {
      notie.alert({ text: 'Okay, jeez...' })
    })
  })
})

notie.input({
  text: 'Please enter your email:',
  submitText: 'Submit',
  cancelText: 'Cancel',
  value: 'jane@doe.com',
  type: 'email',
  placeholder: 'name@example.com'
}, function(value) {
  notie.alert({ type: 1, text: 'You entered: ' + value })
}, function(value) {
  notie.alert({ type: 3, text: 'You cancelled with this value: ' + value })
})

notie.input({
  text: 'Please enter your name:',
  type: 'text',
  placeholder: 'Jane Doe',
  allowed: ['a', 's']
}, function(value) {
  notie.alert({ type: 1, text: 'You entered: ' + value })
}, function(value) {
  notie.alert({ type: 3, text: 'You cancelled with this value: ' + value })
})

notie.input({
  text: 'Please enter the price:',
  type: 'text',
  placeholder: '500',
  allowed: new RegExp('[^0-9]', 'g')
}, function(value) {
  notie.alert({ type: 1, text: 'You entered: ' + value })
}, function(value) {
  notie.alert({ type: 3, text: 'You cancelled with this value: ' + value })
})

notie.select({
  text: 'Demo item #1, owner is Jane Smith',
  cancelText: 'Cancel',
  choices: [
    {
      text: 'Share',
      handler: function () {
        notie.alert({ type: 1, text: 'Share item!' })
      }
    },
    {
      text: 'Open',
      handler: function () {
        notie.alert({ type: 1, text: 'Open item!' })
      }
    },
    {
      type: 2,
      text: 'Edit',
      handler: function () {
        notie.alert({ type: 2, text: 'Edit item!' })
      }
    },
    {
      type: 3,
      text: 'Delete',
      handler: function () {
        notie.alert({ type: 3, text: 'Delete item!' })
      }
    }
  ]
})

notie.date({
  value: new Date(2015, 8, 27),
  submitText: 'Submit',
  cancelText: 'Cancel'
}, function (date) {
  notie.alert({ type: 1, text: 'You selected: ' + date.toISOString() })
}, function (date) {
  notie.alert({ type: 3, text: 'You cancelled: ' + date.toISOString() })
})
```

#### Use ES6 to inherit `this`:
``` javascript
notie.confirm('Is ES6 great?', 'Yes', 'Cancel', () => {
  this.location.href = 'htts://google.com'
})
```

## Custom Styles

#### SASS:
```scss
// Before notie is imported:
$notie-color-success: #57BF57;
$notie-color-warning: #D6A14D;
$notie-color-error: #E1715B;
$notie-color-info: #4D82D6;
$notie-color-neutral: #A0A0A0;
// See all overwriteable variables in src/notie.scss

// Then import notie:
@import '../../node_modules/notie/src/notie';
```

#### CSS:
```css
/* After notie styles are applied to DOM: */
.notie-container {
  box-shadow: none;
}
```

## Options & Methods

```javascript
// Showing all available options with defaults
notie.setOptions({
  alertTime: 3,
  dateMonths: ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December']
  overlayClickDismiss: true,
  overlayOpacity: 0.75,
  transitionCurve: 'ease',
  transitionDuration: 0.3,
  transitionSelector: 'all'
  classes: {
    container: 'notie-container',
    textbox: 'notie-textbox',
    textboxInner: 'notie-textbox-inner',
    button: 'notie-button',
    element: 'notie-element',
    elementHalf: 'notie-element-half',
    elementThird: 'notie-element-third',
    overlay: 'notie-overlay',
    backgroundSuccess: 'notie-background-success',
    backgroundWarning: 'notie-background-warning',
    backgroundError: 'notie-background-error',
    backgroundInfo: 'notie-background-info',
    backgroundNeutral: 'notie-background-neutral',
    backgroundOverlay: 'notie-background-overlay',
    alert: 'notie-alert',
    inputField: 'notie-input-field',
    selectChoiceRepeated: 'notie-select-choice-repeated',
    dateSelectorInner: 'notie-date-selector-inner',
    dateSelectorUp: 'notie-date-selector-up'
  },
  ids: {
    overlay: 'notie-overlay'
  }
})
```

```javascript
// programmatically hide all alerts with an optional callback function
notie.hideAlerts(callbackOptional)
```

## License
MIT
