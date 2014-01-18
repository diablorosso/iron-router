API Documentation
====

Helperfunction: "highlightIfActive" (*new*)
----
Checks if the current route matches the routeNames route
and returns the string *activeName* (e.g. css class name) on match.
Matches also subparts to the current route so the tree path will also
marked active. (see examples)

The returned strings can be configured by parameter to this function or by
using a definition in the data section for this route. (see examples)

By default 'active' is returned on match and '' if not.

Syntax:
```javascript
highlightIfActive(routeName, activeName, inactiveName)
```
The *activeName* is returned if the route path from *routeName* matches the current route path. If not, *inactiveName* is returned.

Examples:
in mytemplate.html
```html
    <template name="test">
       <a class="{{highlightIfActive 'sampleroute'}}" href="{{pathFor 'sampleroute'}}">Link</a>
    </template>
```
If *sampleroute* is setup like this
```javascript
Router.map(function () {
  /**
   * The route's name is "sampleroute"
   * The route's path is "/user/login"
   */
  this.route('sampleroute', {
    path: '/user/login'
  });
```
The template will generate following html
if the current route is '/user/login' (*reminder:* string 'active' is default value)
```html
    <a class="active" href="/user/login">Link</a>
```
and this if current route is '/somewhere/else' (*reminder:* empty string '' is default value)
```html
    <a class="" href="/user/login">Link</a>
```
Also for subpath matches, it will return the *activeName*.
Let's say current route is '/user/login/forgotpassword'
```html
    <a class="active" href="/user/login">Link</a>
```
is returned since '/user/login' is part of '/user/login/forgotpassword'

This is usefull if you use html menu navigation elements and want the parent node also to be shown as active

The returned string for *activeName* and *inactiveName* could be configured in several ways.
You may name them on template call:
in mytemplate.html
```html
    <template name="test">
       <a class="{{highlightIfActive 'sampleroute' activeName='highlight' inactiveName='normal'}}" href="{{pathFor 'sampleroute'}}">Link</a>
    </template>
```
Now this will be rendered to either
```html
    <a class="highlight" href="/user/login">Link</a>
```
or
```html
    <a class="normal" href="/user/login">Link</a>
```
Both parameters are optional, so it's enough to specify only one of them.

To avoid that you need to specify the class name in all your templates, you may define them on route's definition
in it's data section like this:
```javascript
Router.map(function () {
  /**
   * The route's name is "sampleroute"
   * The route's path is "/user/login"
   */
  this.route('sampleroute', {
    path: '/user/login'
    data: {
      activeName: 'highlight',
      inactiveName: 'normal'
    },
  });
```
With this a template call like
```html
    <template name="test">
       <a class="{{highlightIfActive 'sampleroute'}}" href="{{pathFor 'sampleroute'}}">Link</a>
    </template>
```
will also return either
```html
    <a class="highlight" href="/user/login">Link</a>
```
or
```html
    <a class="normal" href="/user/login">Link</a>
```
Both parameters are optional, so it's enough to specify only one of them.




Route function: "getFromData"  (*new*)
----
get an option stored in "data" by *keynameÜ or return *default* value.
Syntax:
```javascript
**getFromData**(*keyname*, *defaultValue*);
```
The *defaultValue* is returned when *keyname* is not found in data. Otherwise the value for *keyname* is returned.
Example:
```javascript
var myValue = route.getFromData('foo', 'bar');
```
Will return the value from route.data.foo or 'bar', if it not exists
