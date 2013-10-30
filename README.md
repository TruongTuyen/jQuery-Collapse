# jQuery Collapse

A lightweight and flexible jQuery plugin that allows you to collapse content. A feature also
known as 'progressive disclosure'.

jQuery Collapse is tested agaisnt the latest version of jQuery but requires at least jQuery 1.7.0 or newer.

Enjoy!


## Features

- [WAI ARIA](http://dev.opera.com/articles/view/introduction-to-wai-aria/) compliant
- Lightweight (~1.2kb minified and gzipped)
- Cross Browser compliant (Tested in >= IE6, Firefox, Safari, Chrome, Opera)
- **Accordion** behaviour can be enabled.
- **Persistence** to remember open sections on page reload!


### Demo

A demo showcasing all the features of the plugin can be found at 'demo/demo.html' in
this repository.

## Usage

Load jQuery and the jQuery Collapse plugin into your document:

```html
<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.8.1/jquery.min.js"></script>
<script src="jquery.collapse.js"></script>
```

Here's some sample HTML markup:

```html
<div data-collapse>
  <h2>Fruits</h2>
  <ul>
    <li>Apple</li>
    <li>Pear</li>
    <li>Orange</li>
  </ul>
  <h2>Info</h2>
  <div>
    <p>You can use any container you like (in this case a div element)</p>
  </div>
</div>
```

That's it! The *data-collapse* attribute will automatically trigger the script.

### Open/Collapse section by default

The standard behaviour is to collapse all the sections on page load.
If you want to show a section to the user on page load you can
achieve this by adding an 'collapsed' class to the section heading

```html
<div data-collapse>
  <h2 class="collapsed">I'm open by default</h2>
  <p>Yay</p>
</div>
```

### Open all sections

You can open or close sections by utilizing events. Assume you have the following markup:

```html
<div id="test" data-collapse>
  <h2>Section 1</h2>
  <p>I'm first</p>
  <h2>Section 2</h2>
  <p>I'm second/p>
</div>
```
You can now trigger events on the elements you want to affect. For instance:

```js
$("#test").trigger("open") // Open all sections
$("#test").trigger("close") // Close all sections
$("#test h2").first().trigger("open") // Open first section
```

For further information, please refer to the [events](#links) documentation.

## JavaScript usage

If you'd rather omit the 'data-collapse' attribute in the HTML and load the plugin via jQuery, you can:

```js
$("#demo").collapse({
  // options...
});
```

If you don't want to use the jQuery ($) wrapper, you can also access the
plugin with *vanilla* JavaScript:

```js
new jQueryCollapse($("#demo"), {
  // options...
});
```

### Using custom markup

By default the plugin will look for groups of two elements.
In real life™ your markup may vary and you'll need to customize how the
plugin interprets it. For example

```html
<div id="demo">
  <div>
    <h2>Summary</h2>
    <div>details...</div>
  </div>
  <div>
    <h2>Summary</h2>
    <div>details...</div>
  </div>
</div>
```

In order for the plugin to understand the above markup, we can pass a 'query'
option specifying where to find the header/summary element of a section:

```js
new jQueryCollapse($("#demo"), {
  query: 'div h2'
});
```


## Accordion

To activate the accordion behaviour set 'accordion' as the value of the 'data-collapse' attribute:

```html
<div data-collapse="accordion">
  ...
</div>
```


## Persistence

By default, if the user reloads the page all the sections will be closed.
If you want previously collapsed sections to stay open you can add 'persist' to the data-collapse attribute:

```html
<div id="demo" data-collapse="persist">
  ...
</div>
```
And include the storage module in your document *after* the other
scripts.

```html
<script src="jquery.collapse_storage.js"></script>
```

As in the example above, the target element (#demo) **will require an ID** in order for the
persistence to work.

You can combine the accordion and persistence options by adding
both values to the data-collapse attribute:

```html
<div id="demo" data-collapse="accordion persist">
  ...
</div>
```

jQuery Collapse uses HTML5 localStorage if available, otherwise it
will attempt to use cookies (read about IE support below). If that also fails, it will degrade
to work but without any persistence.

### Internet Explorer =< 7 Support

For IE 6-7 you'll need to include the cookie storage and JSON2 libraries
for the cookie storage support to work properly:

```html
<!--[if lt IE 8]>
  <script src="jquery.collapse_cookie_storage.js"></script>
  <script src="json2.js"></script>
<![endif]-->
```

## API Documentation

Here are the exposed options and events that you can play around with
using JavaScript. Enjoy.

### Options

You can pass the following options when initializing
the plugin with JavaScript.

* **open** (function) : Custom function for opening section (default: function(){ this.show() })
* **close** (function) : Custom function for collapsing section (default: function(){ this.hide() })
* **accordion** (bool) : Enable accordion behaviour by setting this option to 'true'
* **persist** (bool) : Enable persistence between page loads by setting this option to 'true'

Example usage of options:

```js
// Initializing collapse plugin
// with custom open/close methods,
// persistence plugin and accordion behaviour
$("#demo").collapse({
  open: function() {
    // The context of 'this' is applied to
    // the collapsed details in a jQuery wrapper
    this.slideDown(100);
  },
  close: function() {
    this.slideUp(100);
  },
  accordion: true,
  persist: true
});
```

### Events

#### Binding events

You can listen for the **open**, **close** and **toggle** events on a collapsed collection.

```js

$("#demo").bind("open", function(e, section) {
  console.log(section, " is open");
});

$("#demo").bind("close", function(e, section) {
  console.log(section, " is closed");
});

// Or just listen on a specific section
$("#demo h2").first().bind("close", function(e) {
  console.log("First section was opened");
});
```

You can also trigger these events manually to open and close sections as you wish

```js
$("#demo").trigger("open") // open all sections
$("#demo").trigger("close") // close all sections
$("#demo").last().trigger("toggle") // toggle last section
```

### API methods

If you're using vanilla JavaScript to instantiate the plugin, you'll get
access to the **open**, **close*, and **toggle** methods.

```js
var demo = new jQueryCollapse($("#demo")); // Initializing plugin
demo.open(); // Open all sections
demo.close(); // Close all sections
demo.open(0); // Open first section
demo.open(1); // Open second section
demo.close(0); // Close first section
demo.toggle(1); / Toggle second section
```

## Contributing

Did you find a bug? Do you want to introduce a feature? Here's what to do (in the following order)

* Find a bug, or invent a feature.
* Write a test case
* Watch it fail (red light)
* Fix bug / introduce feature
* Watch it pass (green light)
* Refactor / Perfectionize!
* Do a pull request on Github and wait patiently...
* Rejoice!

Tests are written in CoffeeScript with a BDD flavour using mocha and the chai exception framework.

Testa are run with the karma test runner. To run the tests simply type 'karma start' from the project root.

Thanks in advance

