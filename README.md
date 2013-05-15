# jquery-contextmenu [![Build Status](https://travis-ci.org/mar10/jquery-contextmenu.png?branch=master)](https://travis-ci.org/mar10/jquery-contextmenu)

A jQuery plugin that provides a context menu (based on the standard [jQueryUI menu] widget).

  * Themable using [jQuery ThemeRoller](http://jqueryui.com/themeroller/).
  * Supports delegation (i.e. can be bound to elements that don't exist at the
    time the context menu is initialized).
  * Exposes events from [jQueryUI menu]: `blur`, `create`, `focus`, `select`.
  * Optional support for touch devices.


## Status

Beta. Please report issues.


## Demo

[Live demo page](http://wwwendt.de/tech/demo/jquery-contextmenu/demo/).


## Example

Say we have some HTML elements that we want to attach a popup menu to:

```html
<div id="container">
    <div class="hasmenu">AAA</div>
    <div class="hasmenu">BBB</div>
    <div class="hasmenu">CCC</div>
</div>
```

now we can enable a context menu like so:

```js
$("#container").contextmenu({
	delegate: ".hasmenu",
	menu: [
		{title: "Copy", cmd: "copy", uiIcon: "ui-icon-copy"},
		{title: "Paste", cmd: "paste", disabled: true },
		{title: "----"},
		{title: "More", children: [
			{title: "Sub 1", cmd: "sub1"},
			{title: "Sub 2", cmd: "sub1"}
			]}
		],
	select: function(event, ui) {
		var menuId = ui.item.find(">a").attr("href");
		alert("select " + menuId + " on " + $(event.relatedTarget).text());
	}
});
```

To attach menus to *all* elements on the page that have `class="hasmenu"`,
we use `document` as context:
```js
$(document).contextmenu({
    delegate: ".hasmenu",
    ...
});
```

### Initialize menu from an existing `<ul>` element

In this case `menu` must point to the markup:

```js
$(document).contextmenu({
    delegate: ".hasmenu",
    menu: "#options",
    select: function(event, ui) {
    	...
    }
});
```

We also have to provide some HTML markup that defines the context menu 
structure (see [jQueryUI menu] for details):

```html
<ul id="options" class="ui-helper-hidden">
    <li><a href="#copy"><span class="ui-icon ui-icon-copy"></span>Copy</a>
    <li class="ui-state-disabled"><a href="#paste">Paste</a>
    <li>----
    <li><a>More</a>
        <ul>
            <li><a href="#sub1">Sub 1</a>
            <li><a href="#sub2">Sub 2</a>
        </ul>
</ul>
```


## API documentation
### Options
<dl>
<dt>delegate</dt>
<dd>
    Type: <code>String</code><br>
    A selector to filter the elements that trigger the context menu.    
</dd>
<dt>ignoreParentSelect</dt>
<dd>
    Type: <code>Boolean</code>, default: <code>true</code><br>
    If <code>true</code>, a click on a menu item that contains a sub-menu, will 
    <em>not</em> trigger the <code>select</code> event.
</dd>
<dt>menu</dt>
<dd>
    Type: <code>Object[] | String | jQuery | function</code><br>
    jQuery object or selector (or function returning such) of HTML markup that 
    defines the context menu structure (see 
    <a href="http://jqueryui.com/menu/">jQueryUI menu</a> for details).

    If an array of objects is passed, it will be interpreted used to generate
    such markup on the fly.

    If a function is passed, it must return one of the formats described above.
</dd>
<dt>preventSelect</dt>
<dd>
    Type: <code>Boolean</code>, default: <code>false</code><br>
    Prevent accidental text selection of potential menu targets on doubleclick 
    or drag.
</dd>
<dt>taphold</dt>
<dd>
    Type: <code>Boolean</code>, default: <code>false</code><br>
    Open menu on <a href="http://api.jquerymobile.com/taphold/">taphold events</a>, 
    which is especially useful for touch devices (but may require external 
    plugins to generate <code>taphold</code> events).
</dd>
</dl>


### Methods
<dl>
<dt>close()</dt>
<dd>
    Close context menu if open.<br>
    Call like <code>$(...).contextmenu("close");</code>.
</dd>
<dt>open(target)</dt>
<dd>
    Open context menu on a specific target (target must match the options.delegate filter).<br>
    Call like <code>$(...).contextmenu("open", target);</code>.
</dd>
</dl>


### Events
jquery-contextmenu exposes events from [jQueryUI menu]: `blur`, `create`, `focus`, `select`.
However, since the `event.target` parameter contains the menu item, we additionally pass the element 
that was right-clicked in `event.relatedTarget`.

Events may be handled by defining a handler option:
```js
$("#container").contextmenu({
    [...]
    select: function(event, ui) {
        var menuId = ui.item.find(">a").attr("href"),
            target = event.relatedTarget;
        alert("select " + menuId + " on " + $(target).text());
    }
});
```

Alternatively a handler may be bound, so this is equivalent:
```js
$("#container").bind("contextmenuselect", function(event, ui) {
    var menuId = ui.item.find(">a").attr("href"),
        target = event.relatedTarget;
    alert("select " + menuId + " on " + $(target).text());
}
```

<dl>
<dt>beforeOpen(event)</dt>
<dd>
    Triggered just before the popup menu is opened.<br>
    Return <code>false</code> to prevent opening.
</dd>
<dt>blur(event, ui)</dt>
<dd>
    Triggered when the menu loses focus.
</dd>
<dt>close(event)</dt>
<dd>
    Triggered when the menu is closed.
</dd>
<dt>create(event, ui)</dt>
<dd>
    Triggered when the menu is created.
</dd>
<dt>focus(event, ui)</dt>
<dd>
    Triggered when a menu gains focus or when any menu item is activated.
</dd>
<dt>init(event)</dt>
<dd>
    Triggered when the contextmenu widget is initialized.
</dd>
<dt>open(event)</dt>
<dd>
    Triggered when the menu is opened.
</dd>
<dt>select(event, ui)</dt>
<dd>
    Triggered when a menu item is selected.<br>
    Return <code>false</code> to prevent closing the menu.
</dd>
</dl>

[![Bitdeli Badge](https://d2weczhvl823v0.cloudfront.net/mar10/jquery-contextmenu/trend.png)](https://bitdeli.com/free "Bitdeli Badge")

[jQueryUI menu]: http://jqueryui.com/menu/
