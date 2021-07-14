# Makimo Android Style Guide

This section presents coding convention used at Makimo for applications written
in Kotlin/Android.

The convention is based on [the official Kotlin style
guide](https://kotlinlang.org/docs/reference/coding-conventions.html).  In case
of omitted aspects of code style or some differences between the company policy
and Kotlin style guide, the following document gives preferred coding rules.

## Kotlin

### 1. Line length

Try not to exceed 100 characters per line. In some cases it is allowed to
use up to 120 characters. If your code line is longer than 120 characters,
please look at [the official Kotlin style
guide](https://kotlinlang.org/docs/reference/coding-conventions.html) to
reformat a piece of code.

### 2. Brackets around control flow statements and loops

Always use curly brackets around `if` and `when` when expression is multiline
as well as around loop statements. Put the opening curly brackets at the end of
the line and the closing one on a separate line aligned to the beginning of the
code block.

```kotlin
for (el in elements) {
    // do sth
}
```

```kotlin
if (bar) {
    // do sth
} else {
    // do sth else
}
```

```kotlin
when {
    x -> 1
    y -> 2
    else -> 3
}
```

In case of one-liners do not use brackets.

```kotlin
if (bar) doSth()
```

Also look at the official Kotlin style guide paragraph about
[formating](https://kotlinlang.org/docs/reference/coding-conventions.html#formatting)

### 3. Type declaration

Do not use type declarations in case of values and variables.

```kotlin
val x: Int = 5 // Bad
var y = "OK" // Good
```

### 4. Scope functions

Try to group code with scope functions such as `apply` if possible.

```kotlin
// Bad
val intent = Intent(this, SomeActivity::class.java)
intent.putExtra(VAL_1, 1)
intent.putExtra(VAL_2, 2)

// Good
val intent = Intent(this, SomeActivity::class.java).apply {
    putExtra(VAL_1, 1)
    putExtra(VAL_2, 2)
}
```

Other examples of scope functions are: `with`, `run`, `also` and `let`.

Look at the official Kotlin style guide section about [using scope functions]
(https://kotlinlang.org/docs/reference/coding-conventions.html#using-scope-functions-applywithrunalsolet)

### 5. Expressions

Many Kotlin's structure are expressions so try to use theirs advantages whenever possible.

#### 5.1 Control flow structures

```kotlin
// Bad
if (bar) {
    return 1
} else {
    return 2
}

// Good
return if (bar) {
    1
} else {
    2
}
```

```kotlin
// Bad
when (value) {
    x -> return 1
    y -> return 2
    else -> return 3
}

// Good
return when (value) {
    x -> 1
    y -> 2
    else -> 3
}
```

#### 5.2 Functions

```kotlin
// Bad
fun foo(x: Int): Int {
    return 2 * x
}

// Bad (unless context requires explicit type)
fun foo(x: Int): Int = 2 * x

// Good
fun foo(x: Int) = 2 * x
```

```kotlin
// Bad
fun foo(x: Int, y: String): Bar {
    val o = Bar()
    o.x = x
    o.y = y
    return o
}

// Good
fun foo(x: Int, y: String) = Bar().apply{
    this.x = x
    this.y = y
}
```

## XML

### 1. Naming `layouts`

Good naming is an absolute must for an Android project. In order to make
searching for Android layout files easier, we use prefixes corresponding
to their function. Accordingly:

* `activity_` - Activity layout files;

* `fragment_` - Fragment layout files;

* `layout_` - Raw layouts; imported via `<layout name="...">`;

* `dialog_` - Dialog layout files;

* `item_` - List view item layout files;

* `view_` - View layout files, used by widgets and small self-contained widgets;


### 2. Naming `drawables`

* `btn_` - Custom button layout;

* `ic_` - Icon layout;

* `bkg_` - Background layout for a view;

* `view_` - Generic view layout (when other type does not fit);

* `selector_` - Selectors defining  different states of components;


### 3. Naming colors

Color names should correspond to the color they represent, *not* functionality
it is related to, such as `appBarColor` or `navBarBackgroundColor`.
Those are too hard to remember. 

It does not matter whether the color values ​​are written in uppercase or lowercase, 
But it's important to be consistent and stick to a chosen notation.

```xml
<!-- Bad -->
<color name="lightCerulean">#7bd3f7</color>
<color name="cerulean">#019FD9</color>
<color name="tealBlue">#007bb2</color>
<color name="pinkishRed">#F31F32</color>
```

```xml
<!-- Good -->
<color name="lightCerulean">#7bd3f7</color>
<color name="cerulean">#019fd9</color>
<color name="tealBlue">#007bb2</color>
<color name="pinkishRed">#f31f32</color>
```

```xml
<!-- Good -->
<color name="lightCerulean">#7BD3F7</color>
<color name="cerulean">#019FD9</color>
<color name="tealBlue">#007BB2</color>
<color name="pinkishRed">#F31F32</color>
```


### 4. Naming strings

Strings in most applications are divided into one-time specific strings
and generic informational strings, such as error-messages, confirmation
buttons, retry labels and so on. Accordingly, `strings` should reflect
this division. Examples of generic strings used throughout the application:

```xml
<string name="next">Next</string>
<string name="cancel">Cancel</string>
<string name="error_while_loading_data">There was a problem loading the data. Check your internet connection.</string>
```

Examples of specific strings:

```xml
<string name="next_event_date">Next event date: %1$s</string>
<string name="policy_number">Policy number: %1$s</string>
```

### 5. Naming dimens

When possible, dimensions should be divisible by 2, e.g.: 4, 8, 12, 16, 20,  24, 32, 50.
Most projects should use a finite set of standard paddings; e.g.:

```xml
<dimen name="xsmall_padding">4dp</dimen>
<dimen name="small_padding">8dp</dimen>
<dimen name="normal_padding">16dp</dimen>
<dimen name="large_padding">24dp</dimen>
<dimen name="xlarge_padding">32dp</dimen>
```

If the application contains a large number of dimens and naming them starts to be challenging 
it's a good practice to name them based on their value; eg.;

```xml
<dimen name="dim_8">8dp</dimen>  
<dimen name="dim_16">16dp</dimen>  
<dimen name="dim_24">24dp</dimen>  
<dimen name="dim_32">32dp</dimen>  
```

In the same way, you can define dimensions for text; eg.:

```xml
    <dimen name="h0">48sp</dimen>
    <dimen name="h1">24sp</dimen>
    <dimen name="h2">20sp</dimen>
    <dimen name="h3">18sp</dimen>  
	
	<dimen name="sp_4">4sp</dimen>
    <dimen name="sp_8">8sp</dimen>
    <dimen name="sp_10">10sp</dimen>
    <dimen name="sp_14">14sp</dimen>
```

### 6. Naming styles

Similar to naming colors, styles should be as generic as possible 
and correspond to their appearance or properties, not functionality.

```xml
<!-- Good -->
    <style name="RedButton">
        <item name=""></item>
    </style>
```

```xml
<!-- Bad -->
    <style name="NoInternetConnectionButton">
        <item name=""></item>
    </style>
```

It's also a good practice to separate the common base of different variants of styles, in such a case 
we should specify the variant name after the `ParentName.` beginning with a capital letter; eg.;

```xml
    <style name="RedButton">
        <item name=""></item>
    </style>

    <style name="RedButton.Large" parent="RedButton">
        <item name=""></item>
    </style>
	
	<style name="RedButton.Small" parent="RedButton">
        <item name=""></item>
    </style>
```