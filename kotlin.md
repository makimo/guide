# Makimo Android Style Guide

This section presents coding convention used at Makimo for applications written
in Kotlin/Android.

The convention is based on [the official Kotlin style
guide](https://kotlinlang.org/docs/reference/coding-conventions.html).  In case
of omitted aspects of code style or some differences between the company policy
and Kotlin style guide, the following document gives a preferred coding rules.

## Kotlin

### 1. Line length

Try to do not exceed 100 characters per line. In some cases it is allowed to
use up to 120 characters. If your code line is longer than 120 characters,
please look at [the official Kotlin style
guide](https://kotlinlang.org/docs/reference/coding-conventions.html) to
reformat a piece of code.

### 2. Brackets around control flow statements and loops

Always use curly brackets around `if` and `when` when expression is multiline
as well as around loop statements. Put the opening curly brackets at the end of
the line and the closing one on separate line aligned to the beginning of the
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

Look also at official Kotlin style guide paragraf about
[formating](https://kotlinlang.org/docs/reference/coding-conventions.html#formatting)

### 3. Type declaration

Do not use type declarations in case of values and variables.

```kotlin
val x: Int = 5 // Bad
var y = "OK" // Good
```

### 4. Scope functions

Try to group code with scope functions such `apply` if possible.

```kotlin
// BAD
val intent = Intent(this, SomeActivity::class.java)
intent.putExtra(VAL_1, 1)
intent.putExtra(VAL_2, 2)

// GOOD
val intent = Intent(this, SomeActivity::class.java).apply {
    putExtra(VAL_1, 1)
    putExtra(VAL_2, 2)
}
```

Other examples of scope functions are: `with`, `run`, `also` and `let`.

Look at official Kotlin style guide section about [using scope functions]
(https://kotlinlang.org/docs/reference/coding-conventions.html#using-scope-functions-applywithrunalsolet)

### 5. Expressions

Many Kotlin's structure are expressions so try to use theirs advantages if
possible.

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

// Bad (unless context require explicit type)
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

#### 6. Classes

##### 6.1 Formatting class definition

If declaration is below line length limit, put it on a single line, but
leave one single line after opening braces:

```kotlin
// Good:
class Foo(val b: Bar) {
    
    fun foo() {
        // ...
    }
}

// Bad:
class Foo(val b: Bar) {
    fun foo() {
        // ...
    }
}
```

If declaration is long, break it like so:

```kotlin
class Foo(
    val a: ClassA,
    val b: ClassB,
    val c: ClassC,
    val d: ClassD
): SomeInterface {
    
    fun foo() {
        // ^ Again no the empty line above
    }
}
```

If you're declaring a `companion`, use one single line before and after:
```kotlin
class Foo {
    
    companion object {
        const val SOME_CONST = "some-const" // Notice no separator line here ^
    }
    
    val a = A()
}
```

## XML

### 1. Naming `layouts`

Good naming is an absolute must for an Android project. In order to make
searching for Android layout files easier, we use prefixes corresponding
to their function. Accordingly:

* `layout_` - Raw layouts; imported via `<layout name="...">`;

* `view_` - View layout files, used by widgets and small self-contained widgets;

* `activity_` - Activity layout files;

* `fragment_` - Fragment layout files;


### 2. Naming `drawables`

* `btn_` - Custom button layout;

* `ic_` - Icon layout;

* `bkg_` - Background layout for a view;

* `view_` - Generic view layout (when other type does not fit);


### 3. Naming colors

Color names should correspond to color they represent, *not* functionality
it is related to, such as `appBarColor` or `navBarBackgroundColor`.
Those are too hard to remember. An example of good naming:

```xml
<color name="lightCerulean">#7bd3f7</color>
<color name="cerulean">#019fd9</color>
<color name="tealBlue">#007bb2</color>
<color name="pinkishRed">#f31f32</color>
```

### 4. Naming strings

Strings in most applications are divided into one-time specific strings
and generic informational strings, such as error-messages, confirmation
buttons, retry labels and son on. Accordingly, `strings` should reflect
this divide. Examples of generic strings used throughout the application:

```xml
<string name="proceed">Dalej</string>
<string name="cancel">Anuluj</string>
<string name="data_downloaded">Dane zostały pobrane</string>
<string name="error_occurred">Wystąpił błąd podczas pobiernia danych</string>
```

Examples of specific strings:

```xml
<string name="reminder_date">Termin ważności: %1$s</string>
<string name="policy_number">Numer polisy</string>
<string name="coolant_type">Rodzaj płynu chłodniczego</string>
<string name="insurance_agent">Nazwa ubezpieczyciela</string>
```

### 5. Naming dimens

* All dimensions should be divisible by 2;
* When possible, dimensions should be multiples of 8, e.g.: 8, 16, 24, 32, 40, 48.
* Most projects should be two keylines - `keyline1`, `keyline2`.
* Most projects should use a finite set of standard paddings; e.g.:

```xml
<dimen name="xsmall_padding">4dp</dimen>
<dimen name="small_padding">8dp</dimen>
<dimen name="normal_padding">16dp</dimen>
<dimen name="large_padding">24dp</dimen>
<dimen name="xlarge_padding">32dp</dimen>
```
