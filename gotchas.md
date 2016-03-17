#Common gotchas with frontend code

Below is a list of gotchas that are very easy to miss when writing frontend code at MAS. They're worth having a quick glance over before submitting PR's.

##Colour variables

When using colours, make sure you always reference them via a variable. We have typically been naming colour variables using the [Name that Color](http://chir.ag/projects/name-that-color/) tool. Check in `_variables.scss` to see if that colour exists first, and if not, then add it in there.

Once it's in the main colour palette, you should create a 'map' variable to avoid accessing it directly. This makes the component you're working on easier to maintain, as you can create a more descriptive name. Example below.

```scss
// _variables.scss

// This is a new entry to the main colour palette
$color-cornflower-blue: #6195ED; // generated from http://chir.ag/projects/name-that-color/#6195ED

// This component I'm working on
$component-heading-color: $color-cornflower-blue;
```

***Note also that if you are adding lots of new colours, then there's a chance we should go back to the design team and encourage them to re-use an existing colour. This helps maintainability and limits the number of colours used on the site.***


##Clear HTML formatting

Although code editors will often automatically format the code to their settings, it's worth looking out for things like this:

```html
<span>
  <a href="http://wwww.google.com">This is a great link</a></span>
```

The closing `span` tag there should be dropped onto its own line for readability.


##Favour classnames over elements in CSS

In general, we try to avoid styling elements directly. For example:

```scss
.component-name nav
```

should become

```scss
.component-name__navigation
```

This decouples the CSS from the markup and means that the `nav` could change to another element without having to change the CSS.

There are some instances where this isn't appropriate. The main one is when the content you're styling is generated by the CMS, which will output plain `p` tags. In thisS case it's absolutely fine to target them by element.

##Limit SCSS Nesting

There a few schools of thought on this. We use a BEM naming convention (as detailed in the [main frontend styleguide](https://github.com/moneyadviceservice/frontend-code-standards)).

This ultimately generates very specific classnames to your component, giving you confidence that changing a style will only affect the component you're working on, and avoid spilling out to other parts of the site causing regression bugs. For this reason, we tend to favour very limitied nesting. For example:

```scss
.component-name {
  .component-name__heading {
    .component-name__heading-cta {

    }
  }
}
```

should become:

```scss
.component-name {

}

.component-name__heading {

}

.component-name__heading-cta {

}
```

This both reduces the file size of the outputted CSS, and makes the markup more flexible. You could move the elements around in the DOM without the need to change the CSS.


## Use mixins for font sizing

Favour using the `@body(size, line-height)` mixin for sizing fonts. This provides `rem` units where applicable, and a `px` fallback.

## Use `$baseline-unit` for vertical rhythm

We consider vertical rhythm an important feature for readability. For this reason we try to use a similar base unit for spacing elements vertically. Where possible, use ***no less*** than one `$baseline-unit` and multiples of it when using top & bottom margins, paddings, positions. For example:

```scss
.component-name__heading {
  margin-top: $baseline-unit * 4;
}
```