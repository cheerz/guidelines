# Android Style Guide

This is Cheerz's Android Style Guide.

It is based on [Kotlin's coding conventions](https://kotlinlang.org/docs/reference/coding-conventions.html)

It was inspired by [GitHub's Ruby guide](https://web.archive.org/web/20160410033955/https://github.com/styleguide/ruby) and [Airbnb's Ruby guide](https://github.com/airbnb/ruby).

## Table of Contents
  1. [Applying the style guide](#applying-the-style-guide)
  1. [Source code organization](#source-code-organization)
  1. [Naming rules](#naming-rules)
  1. [Formatting](#formatting)

## Applying the style guide

To configure the IntelliJ/Android Studio formatter according to this style guide, 
* Go to `Settings` | `Editor` | `Code Style` | `Kotlin`
* Click `Set from…` link in the upper right corner
* Select `Predefined style` | `Kotlin style guide` from the menu.<sup>[[link](#applying-the-style-guide)]</sup>

## Source code organization

### Class layout

Generally, the contents of a class is sorted in the following order:

* Property declarations and initializer blocks
* Secondary constructors
* Method declarations
* Companion object
* Nested classes

For Android/framework classes (e.g. `Activity` or `Fragment`) put framework methods first.
Also, put related stuff together, so that someone reading the class from top to bottom would be able to follow the logic of what's happening.
Higher-level stuff should go first (after framework methods if there are any).

Put nested classes next to the code that uses those classes.
If classes are intended to be used externally and aren't referenced inside the class, put them in the end, after the companion object.<sup>[[link](#class-layout)]</sup>

### Interface implementation layout

When implementing an interface, keep the implementing members in the same order as members of the interface.<sup>[[link](#interface-implementation-layout)]</sup>

### Overload layout

Always put overloads next to each other in a class.<sup>[[link](#overload-layout)]</sup>

## Naming rules

### Naming modules

Names of modules are **always lower case** (`app`).
Using multi-word names is generally discouraged, but if you do need to use multiple words, use **snake case** (`selection-store`).

Names of modules should not be prefixed using company name (e.g. `cz-app`).<sup>[[link](#naming-modules)]</sup>

### Naming packages

Names of packages are **always lower case** and do NOT use underscores (`com.cheerz.network`).
Using multi-word names is generally discouraged, but if you do need to use multiple words, you can either simply concatenate them together (`com.cheerz.mypackage`).<sup>[[link](#naming-packages)]</sup>

### Naming test methods

In tests (and only in tests), it's acceptable to use method names with spaces enclosed in backticks. Underscores in method names are also allowed in test code.<sup>[[link](#naming-test-methods)]</sup>

```java
class MyTestCase {
     @Test fun `ensure everything works`() { ... }
     
     @Test fun ensureEverythingWorks_onAndroid() { ... }
}
```

## Formatting

### Indentation

Use 4 spaces for indentation. Do not use tabs.<sup>[[link](#indentation)]</sup>

### Line Length

Keep each line of code to a readable length. Unless you have a reason to, keep lines to fewer than 120 characters.<sup>[[link](#line-length)]</sup>

### Horizontal whitespace

Put a space after `//`

```java
//this is a bad comment
```
```java
// This is a good comment
```

### Function and expression body formatting

<a name="formating-single-line-expression"></a>Prefer using an expression body for functions with the body consisting of a single expression.<sup>[[link](#formating-single-line-expression)]</sup>

```java
fun foo(): Int { // bad
    return 1
}
```
```java
fun foo() = 1    // good
```

<a name="formating-multiline-expression-body"></a>If the function has an expression body that doesn't fit in the same line as the declaration, put the `=` sign on the first line. Indent the expression body by 4 spaces.<sup>[[link](#formating-multiline-expression-body)]</sup>

```java
fun f(x: String) =
    x.length
```

### Formatting control flow statements

<a name="formating-multiline-condition-statement"></a>If the condition of an `if` or `when` statement is multiline, always use curly braces around the body of the statement. Put the closing parentheses of the condition together with the opening curly brace on a separate line:<sup>[[link](#formating-multiline-condition-statement)]</sup>

```java
if (!component.isSyncing &&
    !hasAnyKotlinRuntimeInScope(module)
) {
    return createKotlinNotConfiguredPanel(module)
}
```

<a name="formating-affirmative-condition-statement"></a>Prefer affirmative statements to negative ones.<sup>[[link](#formating-affirmative-condition-statement)]</sup>
```java
if (!statment) {  // Bad
    doIfFalse()
} else {
    doIfTrue()
}
```
```java
if (statment) {   // Good
    doIfTrue()
} else {
    doIfFalse()
}
```

<a name="formating-curly-brace"></a>Put the `else`, `catch`, `finally` keywords, as well as the `while` keyword of a do/while loop, on the same line as the preceding curly brace:<sup>[[link](#formating-curly-brace)]</sup>

```java
if (condition) {
    // body
} else {
    // else part
}

try {
    // body
} finally {
    // cleanup
}
```

### Chained call wrapping

When wrapping chained calls, put the `.` character or the `?.` operator on the next line, with a single indent:

```java
val anchor = owner
    ?.firstChild
    .siblings(forward = true)
    .dropWhile { it is PsiComment || it is PsiWhiteSpace }
```

The first call in the chain usually should have a line break before it, but it's OK to omit it if the code makes more sense that way.<sup>[[link](#chained-call-wrapping)]</sup>

### New lines

<a name="new-line-after-conditional"></a>Add a new line after conditionals,
blocks, case statements, etc.<sup>[[link](#new-line-after-conditional)]</sup>

```java
if (robot.isAwesome) {
    doSomehting()
}

doSomehtingElse()
```

<a name="newline-between-methods"></a>Include one, but no more than one, new
line between methods.<sup>[[link](#newline-between-methods)]</sup>

```java
// Bad
fun doSomething() { ... }


fun doSomethingElse() { ... }
```
```java
// Good
fun doSomething() { ... }

fun doSomethingElse() { ... }
```