#CSS Coding Guidelines

* [JavaScript hooks](#javascript)
* [Components](#components)
  * [component-name](#componentName)
  * [component-name--modifier-name](#componentName--modifierName)
  * [component-name__descendant-name](#componentName-descendantName)
  * [component-name.is-state-of-component](#is-stateOfComponent)
* [Variables](#variables)
* [Formatting](#formatting)
  * [Spacing](#spacing)
  * [Quotes](#quotes)
* [Performance](#performance)
  * [Specificity](#specificity)


## JavaScript hooks

syntax: `js-<targetName>`

JavaScript-specific classes reduce the risk that changing the structure or theme of components
will inadvertently affect any required JavaScript behaviour and complex functionality.
In practice this looks like this:

```html
<a href="/login" class="btn btn--primary js-login"></a>
```

**Again, JavaScript-specific classes should not, under any circumstances, be styled.**


## Components

BEM Syntax: `<componentName>[--modifierName|__descendantName]`

Component driven development offers several benefits when reading and writing HTML and CSS:

* It helps to distinguish between the classes for the root of the component, descendant elements,
and modifications.
* It keeps the specificity of selectors low.
* It helps to decouple presentation semantics from document semantics.

### ComponentName

The component's name can be split by one hyphen.

```css
.my-component { /* ... */ }
```

```html
<article class="my-component">
  ...
</article>
```

### componentName--modifierName

A component modifier is a class that modifies the presentation of the base component in some form.
Modifier names can be split by one hyphen and be separated from the component name by two hyphens.
The class should be included in the HTML _in addition_ to the base component class.

```css
/* Core button */
.btn { /* ... */ }
/* Primary button style */
.btn--primary { /* ... */ }
```

```html
<button class="btn btn--primary">...</button>
```
### componentName__descendantName

A component descendant is a class that is attached to a descendant node of a component.
It's responsible for applying presentation directly to the descendant on behalf of a particular component.
Descendant names can be split by one hyphen.

```html
<article class="tweet">
  <header class="tweet__header">
    <img class="tweet__avatar" src="{$src}" alt="{$alt}">
    ...
  </header>
  <div class="tweet__body">
    ...
  </div>
</article>
```

### componentName.is-stateOfComponent

Use `is-stateName` for state-based modifications of components.
The state name must be split by one hyphen.
**Never style these classes directly; they should always be used as an adjoining class.**

JS can add/remove these classes.
This means that the same state names can be used in multiple contexts,
but every component must define its own styles for the state (as they are scoped to the component).

```css
.tweet { /* ... */ }
.tweet.is-expanded { /* ... */ }
```

```html
<article class="tweet is-expanded">
  ...
</article>
```

## Variables

Syntax: `[ComponentName]-<property>-<value>`

```CSS
@HighlightMenu-color-grayLight: rgb(51, 51, 50);
```

## Formatting

The following are some high level page formatting style rules.

### Spacing

CSS rules should be comma separated but live on new lines:

**Right:**
```css
.content,
.content__edit {
  ...
}
```

**Wrong:**
```css
.content, .content__edit {
  ...
}
```

CSS blocks should be separated by a single new line. not two. not 0.

**Right:**
```css
.content {
  ...
}
.content__edit {
  ...
}
```

**Wrong:**
```css
.content {
  ...
}

.content__edit {
  ...
}
```

### Quotes

Quotes are optional in CSS and LESS.
We use double quotes as it is visually clearer that the string is not a selector or a style property.

**Right:**
```css
background-image: url("/img/you.jpg");
font-family: "Helvetica Neue Light", "Helvetica Neue", Helvetica, Arial;
```

**Wrong:**
```css
background-image: url(/img/you.jpg);
font-family: Helvetica Neue Light, Helvetica Neue, Helvetica, Arial;
```

## Performance

### Specificity

Although in the name (cascading style sheets) cascading can introduce unnecessary performance overhead
for applying styles. Take the following example:

**Wrong:**
```css
ul.user-list li span a:hover { color: red; }
```

Styles are resolved during the renderer's layout pass.
The selectors are resolved right to left, exiting when it has been detected the selector does not match.
Therefore, in this example every a tag has to be inspected to see if it resides inside a span and a list.
As you can imagine this requires a lot of DOM walking and and for large documents can cause a significant
increase in the layout time. For further reading checkout:
https://developers.google.com/speed/docs/best-practices/rendering#UseEfficientCSSSelectors

The Specificity Graph should trend upward.