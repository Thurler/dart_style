The dart_style package defines an automatic, opinionated formatter for Dart code. It replaces the whitespace in your program with what it deems to be the best formatting for it. Resulting code should follow the [Dart style guide][] but, moreso, should look nice to most human readers, most of the time.

[dart style guide]: https://dart.dev/guides/language/effective-dart/style

The formatter handles indentation, inline whitespace, and (by far the most difficult) intelligent line wrapping. It has no problems with nested collections, function expressions, long argument lists, or otherwise tricky code.

The formatter will never break your code&mdash;you can safely invoke it automatically from build and presubmit scripts.

**This is a fork of the original dart_style, created from differing opinions on how code should be formatted. You can find a (non-exhaustive) list of changes below.**

## Using the formatter

Because this is a fork, it will not work with the default invocation `dart format` - that one is reserved for the community one - WHICH YOU SHOULD USE if you're developing a package that will be maintained along with the community, instead of for personal / company use.

After cloning this repository, you can use it as a formatter by activating it with `dart pub`:

    $ cd dart_style/
    $ dart pub global activate --source path .

You can now invoke this formatter with `dartformat` or `dartfmt`, after you include the default installation directory in your `PATH`:

    $ export PATH="$PATH":"$HOME/.pub-cache/bin"
    $ cd my_project/
    $ dartformat -w .

Consider adding the `dartformat` command to your editor's pre-save script so you can benefit from automatic formatting when saving your code!

## Example changes

#### Indentation size shenanigans

When a function body statement contains a constructor call that won't fit in max column width, keep the indentation with 2 spaces instead of adding a block of 4 spaces (adding up to 6 spaces):

```dart
String get someMember => SomeConstructor(
  argument1: value1,
  argument2: value2,
  argument3: value3,
);
```
vs
```dart
String get someMember => SomeConstructor(
      argument1: value1,
      argument2: value2,
      argument3: value3,
    );
```

When a ternary statement cannot fit in max column width, indent it with 2 spaces instead of 4:

```dart
String veryLongValue = someCondition
  ? 'veeeeryLongValueA'
  : 'veeeeryLongValueB';
```
vs
```dart
String veryLongValue = someCondition
    ? 'veeeeryLongValueA'
    : 'veeeeryLongValueB';
```

#### If condition indentation

When an if statement's conditions cannot all fit in max column width, spread each condition on a separate new line instead of piggybacking off of the line containing the `if(` and the `) {` tokens.

```dart
if (
  someCondition1 &&
  someCondition2 &&
  someCondition3 &&
  someCondition4 &&
  someCondition5
) {
  // do something
}
```
vs
```dart
if (someCondition1 &&
    someCondition2 &&
    someCondition3 &&
    someCondition4 &&
    someCondition5) {
  // do something
}
```

#### Switch-case curly braces consistency

A switch-case statement's case curly braces behavior should be consistent with the rest of the syntax: start the scope on the same line, and end on a new line:

```dart
switch (argument1) {
  case 'A': {
    // do something
  }
  break;
  case 'B': {
    // do something else
  }
  break;
}
```
vs
```dart
switch (argument1) {
  case 'A':
    {
      // do something
    }
    break;
  case 'B':
    {
      // do something else
    }
    break;
}
```

#### Super constructors indentation

When a super constructor's invocation is not preceded by member assignments, AND the invocation doesn't fit in max column width, keep the indentation with 2 spaces instead of adding a block of 4 spaces (adding up to 6 spaces):

```dart
SomeClass({
  required this.argument1,
}) : super(
  otherArgument1: 'value1',
  otherArgument2: 'value2',
);
```
vs
```dart
SomeClass({
  required this.argument1,
}) : super(
      otherArgument1: 'value1',
      otherArgument2: 'value2',
    );
```

#### Method cascading indentation

When a statement contains a cascade of method invocations, avoid putting each method call in a separate line, as it only works best when the methods are short and simple. Instead, the programmer can choose when to force an indentation by providing a comma to the list of arguments for a method:

```dart
String finalValue = someObject.someMember.method1('someValue').method2(
  (String argument) => aSlightlyComplexStatementThatNeedsBreak(argument),
).method3('yetAnotherValue');

String finalValue2 = someObject.someMember.method1('someValue').method4(
  'anotherValue',
).method3('yetAnotherValue');
```
vs
```dart
String finalValue = someObject.someMember
    .method1('someValue')
    .method2(
      (String argument) => aSlightlyComplexStatementThatNeedsBreak(argument),
    )
    .method3('yetAnotherValue');

String finalValue2 = someObject.someMember
    .method1('someValue')
    .method2('anotherValue')
    .method3('yetAnotherValue');
```

Long cascades are bad anyway, don't use them. :)
