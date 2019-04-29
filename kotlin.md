# Makimo Kotlin Style Guide

This section presents coding convention used at Makimo for applications written in Kotlin.

The convention is based on [the official Kotlin style guide](https://kotlinlang.org/docs/reference/coding-conventions.html).
In case of omitted aspects of code style or some differences between the company policy
and Kotlin style guide, the following document gives a preferred coding rules.

## Content

### 1. Line length

Try to do not exceed 100 characters per line. In some cases it is allowed to use up to
120 characters. If your code line is longer than 120 characters, please look at
[the official Kotlin style guide](https://kotlinlang.org/docs/reference/coding-conventions.html)
to reformat a piece of code.

### 2. Brackets around control flow statements and loops

Always use curly brackets around `if` and `when` when expression is multiline as well as around loop statements. Put the opening curly brackets at the end of the line and the closing one on separate line
aligned to the beginning of the code block.

```
    for (el in elements) {
        // do sth
    }
```

```
    if (bar) {
        // do sth
    } else {
        // do sth else
    }
```

```
    when {
        x -> 1
        y -> 2
        else -> 3
    }
```

In case of one-liners do not use brackets.

```
    if (bar) doSth()
```

Look also at official Kotlin style guide paragraf about [formating](https://kotlinlang.org/docs/reference/coding-conventions.html#formatting)

### 3. Type declaration

Do not use type declarations in case of values and variables.

```
    val x: Int = 5 // Bad
    var y = "OK" // Good
```

### 4. Scope functions

Try to group code with scope functions such `apply` if possible.

```
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

Many Kotlin's structure are expressions so try to use theirs advantages if possible.

#### 5.1 Control flow structures
```
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

```
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

```
    // Bad
    fun foo(x: Int): Int {
        return 2 * x
    }

    // Bad (unless context require explicit type)
    fun foo(x: Int): Int = 2 * x

    // Good
    fun foo(x: Int) = 2 * x
```

```
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
