# Android Style Guide

This is Cheerz's Android Style Guide.

It is based on [Kotlin's coding conventions](https://kotlinlang.org/docs/reference/coding-conventions.html)

It was inspired by [GitHub's Ruby guide](https://web.archive.org/web/20160410033955/https://github.com/styleguide/ruby) and [Airbnb's Ruby guide](https://github.com/airbnb/ruby).

## Table of Contents
  * [IDE setup](#ide-setup)
  * [Source code organization](#source-code-organization)
  * [Naming rules](#naming-rules)
  * [Formatting](#formatting)

## IDE setup

To configure IntelliJ/Android Studio according to this style guide, you can either import the settings file in `Android Studio settings` folder or follow the steps:

### Applying Kotlin style guide

* Go to `Settings` | `Editor` | `Code Style` | `Kotlin`
* Click `Set from…` link in the upper right corner
* Select `Predefined style` | `Kotlin style guide` from the menu.
<sup>[[link](#applying-kotlin-style-guide)]</sup>

### Setup hard wrap

* Still in `Settings` | `Editor` | `Code Style` | `Kotlin`
* Click `Wrapping and Braces` tab
* Set `Hard wrap at` field to `120`
<sup>[[link](#setup-hard-wrap)]</sup>

### Setup blank lines

* Still in `Settings` | `Editor` | `Code Style` | `Kotlin`
* Click `Blank Lines` tab
* In setction `Keep Maximum Blank lines`
* * Set `In declarations:` to `1`
* * Set `In code:` to `1`
* * Set `Before '}':` to `1`
* In setction `Minimun Blank lines`
* * Set `After class header:` to `1`
* * Set `Arround 'when' branches with {}` to `0`
<sup>[[link](#setup-blank-lines)]</sup>

### Setup imports

* Still in `Settings` | `Editor` | `Code Style` | `Kotlin`
* Click `Imports` tab
* In setction `Top-level name import`
* * Select entry `Use single line imports`
* In section `Java Statics and Enum Members`
* * Select entry `Use single line imports`
* In section `Packages to Use Import with '*'`
* * Remove `import java.util.*` entry by clicking the `-` button
<sup>[[link](#setup-imports)]</sup>

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
<sup>[[link](#class-layout)]</sup>

### Interface implementation layout

When implementing an interface, keep the implementing members in the same order as members of the interface.
<sup>[[link](#interface-implementation-layout)]</sup>

### Overload layout

Always put overloads next to each other in a class.
<sup>[[link](#overload-layout)]</sup>

## Naming rules

### Naming modules

Names of modules are **always lower case** (`app`).
Using multi-word names is generally discouraged, but if you do need to use multiple words, use **snake case** (`selection-store`).

Names of modules should not be prefixed using company name (e.g. `cz-app`).
<sup>[[link](#naming-modules)]</sup>

### Naming packages

Names of packages are **always lower case** and do NOT use underscores (`com.cheerz.network`).
Using multi-word names is generally discouraged, but if you do need to use multiple words, you can either simply concatenate them together (`com.cheerz.mypackage`).
<sup>[[link](#naming-packages)]</sup>

### Naming test methods

In tests (and only in tests), it's acceptable to use method names with spaces enclosed in backticks. Underscores in method names are also allowed in test code.
<sup>[[link](#naming-test-methods)]</sup>

```kotlin
class MyTestCase {
     @Test fun `ensure everything works`() { ... }
     
     @Test fun ensureEverythingWorks_onAndroid() { ... }
}
```

### Choosing good names

**Names should be meaningful and concise.**

The names should make it clear what the purpose of the entity is, so it's best to avoid using generic words (`Manager`, `Wrapper`, etc.) as names.

The name of a class is usually a noun or a noun phrase explaining what the class _is_: `List`, `PersonReader`.

The name of a method is usually a verb or a verb phrase saying what the method _does_: `close`, `readPersons`.
The name should also suggest if the method is mutating the object or returning a new one. For instance `sort` is sorting a collection in place, while `sorted` is returning a sorted copy of the collection.
<sup>[[link](#choosing-good-names)]</sup>

<a name="naming-acronyms"></a>
When using an acronym as part of a declaration name, capitalize it if it consists of two letters (`IOStream`); capitalize only the first letter if it is longer (`XmlFormatter`, `HttpInputStream`).
<sup>[[link](#choosing-good-names)]</sup>

### Naming constants

#### Activities (or Fragment) arguments

Activity (namely Fragment) arguments names should be **prefixed** by `ARG_`
Example : `ARG_PRODUCT_TAG`

Implementation examples :
**Activity**
```kotlin
class MyActivity : AppCompatActivity() {
    ...

    companion object {
        private const val ARG_MIN_PHOTO_COUNT = "com.printklub.polabox.customization.KEY_MIN_PHOTO_COUNT"
        private const val ARG_SELECTION_ID = "com.printklub.polabox.customization.KEY_SELECTION_ID"
        private const val ARG_SELECTION_MODE = "com.printklub.polabox.customization.KEY_SELECTION_MODE"
        private const val ARG_PRODUCT_TAG = "com.printklub.polabox.customization.KEY_PRODUCT_TAG"

        fun startIntent(
            context: Context,
            model: Kustomization.Model,
            selectionId: String,
            selectionMode: GallerySelectionMode
        ) = Intent(context, MyActivity::class.java)
            .apply {
                putExtra(ARG_MIN_PHOTO_COUNT, selectionId)
                putExtra(ARG_SELECTION_ID, model.minPagesCount)
                putExtra(ARG_SELECTION_MODE, model.productTag)
                putExtra(ARG_PRODUCT_TAG, selectionMode)
            }
    }
}
```

**Fragment**
```kotlin
class MyFragment : Fragment() {
    ...

    companion object {
        private const val ARG_SELECTION_ID = "com.printklub.polabox.customization.ARG_SELECTION_ID"
        private const val ARG_PHOTO_MIN_COUNT = "com.printklub.polabox.customization.ARG_PHOTO_MIN_COUNT"

        fun newInstance(minCountPhoto: Int, selectionId: String) = MyFragment()
            .apply { 
                arguments = bundleOf(
                    ARG_PHOTO_MIN_COUNT to minCountPhoto,
                    ARG_SELECTION_ID to selectionId
                )
            }
    }
}
```

#### Keys of key/value pairs

Given a set of keys and values, we may want to have constants defined for the keys. The constants must be **prefixed** by `KEY_`. This prefix does not apply in the special case of arguments of fragments or activities.
Example : `KEY_PRODUCT_TAG`

Implementation example :
```kotlin
const val KEY_MAGNET_PRODUCT_TAG = "com.printklub.polabox.customization.KEY_MAGNET_PRODUCT_TAG"
const val KEY_DIBOND_PRODUCT_TAG = "com.printklub.polabox.customization.KEY_DIBOND_PRODUCT_TAG"

val productTags = mapOf<String, String>(
    KEY_MAGNET_PRODUCT_TAG to "magnet-retro"
    KEY_DIBOND_PRODUCT_TAG to "metallic-print"
)

val dibondTag = productTags(KEY_DIBOND_PRODUCT_TAG)

```

## Formatting

### Indentation

Use 4 spaces for indentation. Do not use tabs.
<sup>[[link](#indentation)]</sup>

### Line Length

Keep each line of code to a readable length. Unless you have a reason to, keep lines to fewer than `120` characters.
<sup>[[link](#line-length)]</sup>

### Horizontal whitespace

Put spaces around binary operators (`a + b`). Exception: don't put spaces around the "range to" operator (`0..i`).

Do not put spaces around unary operators (`a++`)

Put spaces between control flow keywords (`if`, `when`, `for` and `while`) and the corresponding opening parenthesis.

Do not put a space before an opening parenthesis in a primary constructor declaration, method declaration or method call.

```kotlin
class A(val x: Int)

fun foo(x: Int) { ... }

fun bar() {
    foo(1)
}
```

Never put a space after `(`, `[`, or before `]`, `)`.

Never put a space around `.` or `?.`: `foo.bar().filter { it > 2 }.joinToString()`, `foo?.bar()`

Put a space after `//`: `// This is a comment`

Do not put spaces around angle brackets used to specify type parameters: `class Map<K, V> { ... }`

Do not put spaces around `::`: `Foo::class`, `String::length`

Do not put a space before `?` used to mark a nullable type: `String?`

As a general rule, avoid horizontal alignment of any kind. Renaming an identifier to a name with a different length should not affect the formatting of either the declaration or any of the usages.

### Function and expression body formatting

<a name="formating-single-line-expression"></a>
Prefer using an expression body for functions with the body consisting of a single expression.
<sup>[[link](#formating-single-line-expression)]</sup>

```kotlin
fun foo(): Int { // bad
    return 1
}
```
```kotlin
fun foo() = 1    // good
```

<a name="formating-multiline-expression-body"></a>
If the function has an expression body that doesn't fit in the same line as the declaration, put the `=` sign on the first line. Indent the expression body by 4 spaces.
<sup>[[link](#formating-multiline-expression-body)]</sup>

```kotlin
fun f(x: String) =
    x.length
```

### Formatting control flow statements

<a name="formating-multiline-condition-statement"></a>
If the condition of an `if` or `when` statement is multiline, always use curly braces around the body of the statement. Put the closing parentheses of the condition together with the opening curly brace on a separate line:
<sup>[[link](#formating-multiline-condition-statement)]</sup>

```kotlin
if (!component.isSyncing &&
    !hasAnyKotlinRuntimeInScope(module)
) {
    return createKotlinNotConfiguredPanel(module)
}
```

<a name="formating-affirmative-condition-statement"></a>
Prefer affirmative statements to negative ones.
<sup>[[link](#formating-affirmative-condition-statement)]</sup>

```kotlin
if (!statment) {  // Bad
    doIfFalse()
} else {
    doIfTrue()
}
```
```kotlin
if (statment) {   // Good
    doIfTrue()
} else {
    doIfFalse()
}
```

<a name="formating-curly-brace"></a>
Put the `else`, `catch`, `finally` keywords, as well as the `while` keyword of a do/while loop, on the same line as the preceding curly brace:
<sup>[[link](#formating-curly-brace)]</sup>

```kotlin
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

```kotlin
val anchor = owner
    ?.firstChild
    .siblings(forward = true)
    .dropWhile { it is PsiComment || it is PsiWhiteSpace }
```

The first call in the chain usually should have a line break before it, but it's OK to omit it if the code makes more sense that way.
<sup>[[link](#chained-call-wrapping)]</sup>

### New lines

<a name="new-line-after-conditional"></a>
Add a new line after conditionals, blocks, case statements, etc.
<sup>[[link](#new-line-after-conditional)]</sup>

```kotlin
if (robot.isAwesome) {
    doSomehting()
}

doSomehtingElse()
```

<a name="newline-between-methods"></a>
Include one, but no more than one, new line between methods.
<sup>[[link](#newline-between-methods)]</sup>

```kotlin
// Bad
fun doSomething() { ... }


fun doSomethingElse() { ... }
```
```kotlin
// Good
fun doSomething() { ... }

fun doSomethingElse() { ... }
```

## Code idioms

### Early return

Prefer early return syntax over big `if`/`else` blocks.

* Nesting code is reduced which makes the functions easier to read.
* `if`/`else` statements will be closer together and you’ll be doing less hunting for opening and closing brackets.
* The function reads more linear. Human brains are better at parsing linear things.
* Often, you won't have to read the whole method to understand its behavior.
<sup>[[link](#early-return)]</sup>

```kotlin
// Bad
fun doSomething(condition: Boolean) {

    if (condition) {
    
        // A
        // lot 
        // of 
        // code 
        // here

    } else {
        throw Exception(...)
    }
}
```

```kotlin
// Good
fun doSomething(condition: Boolean) {

    if (!condition) {
       throw Exception(...)
    }
    
    // A
    // lot 
    // of 
    // code 
    // here
}
```

```kotlin
// Bad
fun doSomething(someCondition: Boolean, name: String?, intValue: Int): String {

    var result = "SUCCESS"

    if (someCondition) {
        if (!name.isNullOrBlank()) {
            if (intValue != 0) {
                // Do Something here
            } else {
                result = "BAD_VALUE"
            }
        } else {
            result = "BAD_NAME"
        }
    } else {
        result = "BAD_CONDITION"
    }

    return result
}
```

```kotlin
// Good
fun doSomething(someCondition: Boolean, name: String?, intValue: Int): String {

    if (!someCondition) {
        return "BAD_CONDITION"
    }

    if (name.isNullOrBlank()) {
        return "BAD_NAME"
    }

    if (intValue == 0) {
        return "BAD_VALUE"
    }

    // Do Something
    
    return "SUCCESS"
}
```

<sup>[[link](#early-return)]</sup>

### Exhaustive when 

Kotlin language does not provide a nice way to force the exhaustivity of `when` statements that do not return a value.
To check exhaustivity at compile-time, we must use the following syntax:

```kotlin
// This is okay
when (state) {
    State.PENDING -> handlePending()
    State.SUCCESS -> handleSuccess()
    State.FAILURE -> handleFailure()
}.let { } // Hack to exhaust all when cases
```

Note: Although we could use many syntax to do this (`run`, `also`, etc.), the only one authorized in the code base is the `let` one (with a comment). <br>
=> `}.let { } // Hack to exhaust all when cases`

The following statement will not be resilient to new state cases and we might (and we will) forget to handle them.

```kotlin
// This is dangerous
when (state) {
    State.PENDING -> handlePending()
    State.SUCCESS -> handleSuccess()
    State.FAILURE -> handleFailure()
}
```
<sup>[[link](#exhaustive-when)]</sup>

## Framework specificities

### Interacting with a RecyclerView's adapter

Two ways of getting a RecyclerView's adapter can be found.

```kotlin
// 1st kind : storing the adapter
class MyFragment: Framgent() {
    private val recyclerView: RecyclerView
    private lateinit var adapter: MyAdapter

    override fun onCreate() {
        val newAdapter = MyAdapter()
        adapter = newAdapter
        recyclerView.adapter = newAdapter
    }

    private fun doStuffOnAdapter() {
        // The adapter can be got directly from the field of this fragment
    }
}

// 2nd kind : getting the adapter in the RecyclerView
class MyFragment: Framgent() {
    private val recyclerView: RecyclerView

    override fun onCreate() {
        val newAdapter = MyAdapter()
        recyclerView.adapter = newAdapter
    }

    private fun getAdapter() = recyclerView.adapter as MyAdapter

    private fun doStuffOnAdapter() {
        // The adapter can be got from the getAdapter() method
    }
}
```

Either the first and second type of getting the adapter is accepted in the project. In the first case, be careful that the stored adapter is always the one set in the RecyclerView. In the second case, keep in mind that it requires more computational work.