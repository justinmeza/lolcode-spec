# LOLCODE Specification 1.3

DRAFT &mdash; 12 July 2007*

*The goal of this specification is to advance the LOLCODE 1.2 specification with generally agreed-upon proposals from the original forum and site. These proposals add language constructs and functionality that make LOLCODE more similar to what programmers have come to expect from other modern programming languages.*

*Date reflects latest documented proposal.

Archived references:

* [Original goals for the 1.3 specification proposal](https://web.archive.org/web/20130113074443/http://lolcode.com/proposals/1.3/1.3)

---

## Formatting

### Whitespace

*(from 1.2)*

* Spaces are used to demarcate tokens in the language, although some keyword constructs may include spaces.

* Multiple spaces and tabs are treated as single spaces and are otherwise irrelevant.

* Indentation is irrelevant.

* A command starts at the beginning of a line and a newline indicates the end of a command, except in special cases.

* A newline will be Carriage Return (/13), a Line Feed (/10) or both (/13/10) depending on the implementing system. This is only in regards to LOLCODE code itself, and does not indicate how these should be treated in strings or files during execution.

* Multiple commands can be put on a single line if they are separated by a comma (,). In this case, the comma acts as a virtual newline or a soft-command-break.

* Multiple lines can be combined into a single command by including three periods (...) or the unicode ellipsis character (u2026) at the end of the line. This causes the contents of the next line to be evaluated as if it were on the same line.

* Lines with line continuation can be strung together, many in a row, to allow a single command to stretch over more than one or two lines. As long as each line is ended with three periods, the next line is included, until a line without three periods is reached, at which point, the entire command may be processed.

* A line with line continuation may not be followed by an empty line.
Three periods may be by themselves on a single line, in which case, the empty line is "included" in the command (doing nothing), and the next line is included as well.

* A single-line comment is always terminated by a newline. Line continuation (...) and soft-command-breaks (,) after the comment (`BTW`) are ignored.

* Line continuation and soft-command-breaks are ignored inside quoted strings. An unterminated string literal (no closing quote) will cause an error.

### Comments

*(from 1.1)*

Single line comments are begun by `BTW`, and may occur either after a line of code, on a separate line, or following a line of code following a line separator (,).

All of these are valid single line comments:

```
I HAS A VAR ITZ 12          BTW VAR = 12
```

```
I HAS A VAR ITZ 12,         BTW VAR = 12
```

```
I HAS A VAR ITZ 12
                BTW VAR = 12
```

Multi-line comments are begun by `OBTW` and ended with `TLDR`, and should be started on their own lines, or following a line of code after a line separator.

These are valid multi-line comments:

```
I HAS A VAR ITZ 12
            OBTW this is a long comment block
                 see, i have more comments here
                 and here
            TLDR
I HAS A FISH ITZ BOB
```

```
I HAS A VAR ITZ 12,  OBTW this is a long comment block
      see, i have more comments here
      and here
TLDR, I HAS A FISH ITZ BOB
```

### File Creation

*(from 1.2)*

All LOLCODE programs must be opened with the command `HAI`. `HAI` should then be followed with the current LOLCODE language version number (1.3, in this case). There is no current standard behavior for implementations to treat the version number, though.

A LOLCODE file is closed by the keyword `KTHXBYE` which closes the `HAI` code-block.

---

## Variables

### Memory

All variables are merely references to locations in memory. It is assumed that when a variable is no longer referenced, that variable's allocated space will be freed sometime in the future, or on program exit.

### Scope

*(to be revisited and refined)*

All variable scope, as of this version, is local to the enclosing function or to the main program block. Variables are only accessible after declaration, and there is no global scope.

### Naming

*(from 1.1)*

Variable identifiers may be in all small or lowercase letters (or a mixture of the two). They must begin with a letter and may be followed only by other letters, numbers, and underscores. No spaces, dashes, or other symbols are allowed. Variable identifiers are CASE SENSITIVE – "cheezburger", "CheezBurger" and "CHEEZBURGER" would all be different variables.

### Declaration

*(updated from 1.2)*

To declare a variable, the keyword is `I HAS A` followed by the variable name. To assign the variable a value within the same statement, you can then follow the variable name with `ITZ <value>`.

```
I HAS A <variable> ITZ <value>
```

This instantiates and initializes a variable. If the value is a literal, the variable is initialized to the appropriate object type (YARN, TROOF, NOOB, NUMBR, NUMBAR). If the value is an identifier or expression, the variable is initialized to the resulting expression.

```
I HAS A <variable> ITZ A <type>
```

This instatiates a variable, and initializes it to a default value.

Built-In default values:

* YARN - ""
* TROOF - FAIL
* NUMBR - 0
* NUMBAR - 0.0
* NOOB - NOOB

```
I HAS A <variable>
```

This instatiates a variable and initializes it to NOOB. It is shorthand for I HAS A <variable> ITZ NOOB

Function types are declared/initialized using the HOW DUZ I / IF U SAY SO blocks, however they behave the same as variables. For example:

```
HOW DUZ I var YR stuff
	BTW implement
IF U SAY SO

I HAS A var ITZ 0  BTW Throws an error AS var is already taken

var R 0 BTW FUNIKSHUN var no longer exists, it's now NUMBR-0
```
### Deallocation

```
<variable> R NOOB
 ```

This above code ensures that a variable no longer references anything. The reference still exists on the current scope and still requires a small amount of memory. If this was the last reference to an object, it will be garbage collected in the future.

### Primitive Types

All primitive types are considered Immutable. All built in operations return new objects instead of references to old objects. The exceptions to this rule are WIN, FAIL and NOOB. Every TROOF reference is either the WIN or FAIL object. Every NOOB reference is to the NOOB instance.

### SRS (serious) Cast

The SRS operation can be used to interpret a YARN (or something castable to a YARN) as an identifier. This operator may be used anywhere that a regular identifier is expected.

```
SRS <expression>
 ```

Examples:

```
I HAS A var ITZ 0
```

Is the same as:

```
I HAS A name ITZ "var"
I HAS A  SRS name ITZ 0
```

The A becomes optional in variable declaration so:

```
I HAS A SRS name ITZ 0
```

is the same as

```
I HAS SRS name ITZ 0
```

### Assignment

*(from 1.2)*

Assignment of a variable is accomplished with an assignment statement, `<variable> R <expression>`

```
I HAS A VAR            BTW VAR is null and untyped
VAR R "THREE"          BTW VAR is now a YARN and equals "THREE"
VAR R 3                BTW VAR is now a NUMBR and equals 3
```

---

## Types

*(from 1.2)*

The variable types that LOLCODE currently recognizes are: strings (YARN), integers (NUMBR), floats (NUMBAR), and booleans (TROOF) (Arrays (BUKKIT) are reserved for future expansion.) Typing is handled dynamically. Until a variable is given an initial value, it is untyped (NOOB). ~~Casting operations operate on TYPE types, as well.~~

### Untyped

The untyped type (NOOB) cannot be implicitly cast into any type except a TROOF. A cast into TROOF makes the variable FAIL. Any operations on a NOOB that assume another type (e.g., math) results in an error.

Explicit casts of a NOOB (untyped, uninitialized) variable are to empty/zero values for all other types.

### Booleans

The two boolean (TROOF) values are WIN (true) and FAIL (false). The empty string (""), an empty array, and numerical zero are all cast to FAIL. All other values evaluate to WIN.

### Numerical Types

A NUMBR is an integer as specified in the host implementation/architecture. Any contiguous sequence of digits outside of a quoted YARN and not containing a decimal point (.) is considered a NUMBR. A NUMBR may have a leading hyphen (-) to signify a negative number.

A NUMBAR is a float as specified in the host implementation/architecture. It is represented as a contiguous string of digits containing exactly one decimal point. Casting a NUMBAR to a NUMBR truncates the decimal portion of the floating point number. Casting a NUMBAR to a YARN (by printing it, for example), truncates the output to a default of two decimal places. A NUMBAR may have a leading hyphen (-) to signify a negative number.

Casting of a string to a numerical type parses the string as if it were not in quotes. If there are any non-numerical, non-hyphen, non-period characters, then it results in an error. Casting WIN to a numerical type results in "1" or "1.0"; casting FAIL results in a numerical zero.

### Strings

String literals (YARN) are demarked with double quotation marks ("). Line continuation and soft-command-breaks are ignored inside quoted strings. An unterminated string literal (no closing quote) will cause an error.

Within a string, all characters represent their literal value except the colon (:), which is the escape character. Characters immediately following the colon also take on a special meaning.

* :) represents a newline (\n)
* :> represents a tab (\t)
* :o represents a bell (beep) (\g)
* :" represents a literal double quote (")
* :: represents a single literal colon (:)

The colon may also introduce more verbose escapes enclosed within some form of bracket.

* :(`<hex>`) resolves the hex number into the corresponding Unicode code point.
* :{`<var>`} interpolates the current value of the enclosed variable, cast as a string.
* :[`<char name>`] resolves the `<char name>` in capital letters to the corresponding Unicode [normative name](http://www.unicode.org/Public/4.1.0/ucd/NamesList.txt).

### Types

The TYPE type only has the values of TROOF, NOOB, NUMBR, NUMBAR, YARN, and TYPE, as bare words. They may be legally cast to TROOF (all true except for NOOB) or YARN.

*TYPEs are under current review. Current sentiment is to delay defining them until user-defined types are relevant, but that would mean that type comparisons are left unresolved in the meantime.*

---

## Operators

### Calling Syntax and Precedence

Mathematical operators and functions in general rely on prefix notation. By doing this, it is possible to call and compose operations with a minimum of explicit grouping. When all operators and functions have known arity, no grouping markers are necessary. In cases where operators have variable arity, the operation is closed with `MKAY`. An `MKAY` may be omitted if it coincides with the end of the line/statement, in which case the EOL stands in for as many `MKAYs` as there are open variadic functions.

Calling unary operators then has the following syntax:

```
<operator> <expression1>
```

The `AN` keyword can optionally be used to separate arguments, so a binary operator expression has the following syntax:

```
<operator> <expression1> [AN] <expression2>
```

An expression containing an operator with infinite arity can then be expressed with the following syntax:

```
<operator> <expr1> [[[AN] <expr2>] [AN] <expr3> ...] MKAY
```

### Math

The basic math operators are binary prefix operators.

```
SUM OF <x> AN <y>       BTW +
DIFF OF <x> AN <y>      BTW -
PRODUKT OF <x> AN <y>   BTW *
QUOSHUNT OF <x> AN <y>  BTW /
MOD OF <x> AN <y>       BTW modulo
BIGGR OF <x> AN <y>     BTW max
SMALLR OF <x> AN <y>    BTW min
```

`<x>` and `<y>` may each be expressions in the above, so mathematical operators can be nested and grouped indefinitely.

Math is performed as integer math in the presence of two NUMBRs, but if either of the expressions are NUMBARs, then floating point math takes over.

If one or both arguments are a YARN, they get interpreted as NUMBARs if the YARN has a decimal point, and NUMBRs otherwise, then execution proceeds as above.

If one or another of the arguments cannot be safely cast to a numerical type, then it fails with an error.

### Boolean

Boolean operators working on TROOFs are as follows:

```
BOTH OF <x> [AN] <y>          BTW and: WIN iff x=WIN, y=WIN
EITHER OF <x> [AN] <y>        BTW or: FAIL iff x=FAIL, y=FAIL
WON OF <x> [AN] <y>           BTW xor: FAIL if x=y
NOT <x>                       BTW unary negation: WIN if x=FAIL
ALL OF <x> [AN] <y> ... MKAY  BTW infinite arity AND
ANY OF <x> [AN] <y> ... MKAY  BTW infinite arity OR
```

`<x>` and `<y>` in the expression syntaxes above are automatically cast as TROOF values if they are not already so.

### Comparison

Comparison is (currently) done with two binary equality operators:

```
BOTH SAEM <x> [AN] <y>   BTW WIN iff x == y
DIFFRINT <x> [AN] <y>    BTW WIN iff x != y
```

Comparisons are performed as integer math in the presence of two NUMBRs, but if either of the expressions are NUMBARs, then floating point math takes over. Otherwise, there is no automatic casting in the equality, so `BOTH SAEM "3" AN 3` is FAIL.

There are (currently) no special numerical comparison operators. Greater-than and similar comparisons are done idiomatically using the minimum and maximum operators.

```
BOTH SAEM <x> AN BIGGR OF <x> AN <y>   BTW x >= y
BOTH SAEM <x> AN SMALLR OF <x> AN <y>  BTW x <= y
DIFFRINT <x> AN SMALLR OF <x> AN <y>   BTW x > y
DIFFRINT <x> AN BIGGR OF <x> AN <y>    BTW x < y
```

If `<x>` in the above formulations is too verbose or difficult to compute, don't forget the automatically created IT temporary variable. A further idiom could then be:

```
<expression>, DIFFRINT IT AN SMALLR OF IT AN <y>
```

*Suggestions are being accepted for coherently and convincingly english-like prefix operator names for greater-than and similar operators.*

### Concatenation

An indefinite number of YARNs may be explicitly concatenated with the `SMOOSH...MKAY` operator. Arguments may optionally be separated with `AN`. As the `SMOOSH` expects strings as its input arguments, it will implicitly cast all input values of other types to YARNs. The line ending may safely implicitly close the `SMOOSH` operator without needing an `MKAY`.

### Casting

Operators that work on specific types implicitly cast parameter values of other types. If the value cannot be safely cast, then it results in an error.

An expression's value may be explicitly cast with the binary `MAEK` operator.

```
MAEK <expression> [A] <type>
```

Where `<type>` is one of TROOF, YARN, NUMBR, NUMBAR, or NOOB. This is only for local casting: only the resultant value is cast, not the underlying variable(s), if any.

To explicitly re-cast a variable, you may create a normal assignment statement with the `MAEK` operator, or use a casting assignment statement as follows:

```
<variable> IS NOW A <type>         BTW equivalent to
<variable> R MAEK <variable> [A] <type>
```

---

## Input/Output

### Terminal-Based

The print (to STDOUT or the terminal) operator is `VISIBLE`. It has infinite arity and implicitly concatenates all of its arguments after casting them to YARNs. It is terminated by the statement delimiter (line end or comma). The output is automatically terminated with a carriage return (:)), unless the final token is terminated with an exclamation point (!), in which case the carriage return is suppressed.

```
VISIBLE <expression> [<expression> ...][!]
```

There is currently no defined standard for printing to a file.

To accept input from the user, the keyword is

```
GIMMEH <variable>
```

which takes YARN for input and stores the value in the given variable.

*`GIMMEH` is defined minimally here as a holdover from 1.0 and because there has not been any detailed discussion of this feature. We count on the liberal casting capabilities of the language and programmer inventiveness to handle input restriction. `GIMMEH` may change in a future version.*

---

## Statements

### Expression Statements

A bare expression (e.g. a function call or math operation), without any assignment, is a legal statement in LOLCODE. Aside from any side-effects from the expression when evaluated, the final value is placed in the temporary variable `IT`. `IT`'s value remains in local scope and exists until the next time it is replaced with a bare expression.

### Assignment Statements

Assignment statements have no side effects with `IT`. They are generally of the form:

```
<variable> <assignment operator> <expression>
```

The variable being assigned may be used in the expression.

### Flow Control Statements

Flow control statements cover multiple lines and are described in the following section.

---

## Flow Control

### Conditionals

#### If-Then

The traditional if/then construct is a very simple construct operating on the implicit `IT` variable. In the base form, there are four keywords: `O RLY?`, `YA RLY`, `NO WAI`, and `OIC`.

`O RLY?` branches to the block begun with `YA RLY` if `IT` can be cast to WIN, and branches to the `NO WAI` block if `IT` is FAIL. The code block introduced with `YA RLY` is implicitly closed when `NO WAI` is reached. The `NO WAI` block is closed with `OIC`. The general form is then as follows:

```
<expression>
O RLY?
  YA RLY
    <code block>
  NO WAI
    <code block>
OIC
```

while an example showing the ability to put multiple statements on a line separated by a comma would be:

```
BOTH SAEM ANIMAL AN "CAT", O RLY?
  YA RLY, VISIBLE "J00 HAV A CAT"
  NO WAI, VISIBLE "J00 SUX"
OIC
```

The elseif construction adds a little bit of complexity. Optional `MEBBE <expression>` blocks may appear between the YA RLY and NO WAI blocks. If the `<expression>` following `MEBBE` is WIN, then that block is performed; if not, the block is skipped until the following `MEBBE`, `NO WAI`, or `OIC`. The full expression syntax is then as follows:

```
<expression>
O RLY?
  YA RLY
    <code block>
 [MEBBE <expression>
    <code block>
 [MEBBE <expression>
    <code block>
  ...]]
 [NO WAI
    <code block>]
OIC
```

An example of this conditional is then:

```
BOTH SAEM ANIMAL AN "CAT"
O RLY?
  YA RLY, VISIBLE "J00 HAV A CAT"
  MEBBE BOTH SAEM ANIMAL AN "MAUS"
    VISIBLE "NOM NOM NOM. I EATED IT."
OIC
```

#### Case

*(from 1.2)*

The LOLCODE keyword for switches is `WTF?`. The `WTF?` operates on `IT` as being the expression value for comparison. A comparison block is opened by `OMG` and must be a literal, not an expression. (A literal, in this case, excludes any YARN containing variable interpolation (`:{var}`).) Each literal must be unique. The `OMG` block can be followed by any number of statements and may be terminated by a `GTFO`, which breaks to the end of the the `WTF` statement. If an `OMG` block is not terminated by a `GTFO`, then the next `OMG` block is executed as is the next until a `GTFO` or the end of the `WTF` block is reached. The optional default case, if none of the literals evaluate as true, is signified by `OMGWTF`.

```
WTF?
  OMG <value literal>
    <code block>
 [OMG <value literal>
    <code block> ...]
 [OMGWTF
    <code block>]
OIC
```

```
COLOR, WTF?
  OMG "R"
    VISIBLE "RED FISH"
    GTFO
  OMG "Y"
    VISIBLE "YELLOW FISH"
  OMG "G"
  OMG "B"
    VISIBLE "FISH HAS A FLAVOR"
    GTFO
  OMGWTF
    VISIBLE "FISH IS TRANSPARENT"
OIC
```

In this example, the output results of evaluating the variable `COLOR` would be:

"R":

```
RED FISH
```

"Y":

```
YELLOW FISH
FISH HAS A FLAVOR
```

"G":

```
FISH HAS A FLAVOR
```

"B":

```
FISH HAS A FLAVOR
```

none of the above:

```
FISH IS TRANSPARENT
```

#### Loops

*Loops are currently defined more or less as they were in the original examples. Further looping constructs will be added to the language soon.*

Simple loops are demarcated with `IM IN YR <label>` and `IM OUTTA YR <label>`. Loops defined this way are infinite loops that must be explicitly exited with a GTFO break. Currently, the `<label>` is required, but is unused, except for marking the start and end of the loop.

*Immature spec – **subject to change**:*

Iteration loops have the form:

```
IM IN YR <label> <operation> YR <variable> [TIL|WILE <expression>]
  <code block>
IM OUTTA YR <label>
```

Where <operation> may be UPPIN (increment by one), NERFIN (decrement by one), or any unary function. That operation/function is applied to the <variable>, which is temporary, and local to the loop. The TIL <expression> evaluates the expression as a TROOF: if it evaluates as FAIL, the loop continues once more, if not, then loop execution stops, and continues after the matching IM OUTTA YR <label>. The WILE <expression> is the converse: if the expression is WIN, execution continues, otherwise the loop exits.

---

## Functions

*(updated from 1.2)*

### Definition

A function is demarked with the opening keyword `HOW IZ I` and the closing keyword `IF U SAY SO`. The syntax is as follows:

```
HOW IZ I <function name> [YR <argument1> [AN YR <argument2> ...]]
  <code block>
IF U SAY SO
```

Currently, the number of arguments in a function can only be defined as a fixed number. The `<argument>`s are single-word identifiers that act as variables within the scope of the function's code. The calling parameters' values are then the initial values for the variables within the function's code block when the function is called.

*Currently, functions do not have access to the outer/calling code block's variables.*

### Returning

Return from the function is accomplished in one of the following ways:

* `FOUND YR <expression>` returns the value of the expression.
* `GTFO` returns with no value (NOOB).
* in the absence of any explicit break, when the end of the code block is reached (`IF U SAY SO`), the value in `IT` is returned.

### Calling

A function of given arity is called with:

```
I IZ <function name> [YR <expression1> [AN YR <expression2> [AN YR <expression3> ...]]] MKAY
```

That is, an expression is formed by the function name followed by any arguments. Those arguments may themselves be expressions. The expressions' values are obtained before the function is called. The arity of the functions is determined in the definition.

The I parameter is used to distingish a function call on the current namespace vs. a function call on a bukkit (defined below).

---

## Arrays

*(updated from 1.2)*

BUKKITs are the container type. They may hold NUMBRs, NUMBARs, TROOFs, YARNs, functions (FUNKSHUN), and other BUKKITS. Each entity within a BUKKIT may be indexed by a NUMBR or a YARN. These indices, whether NUMBRs or YARNs, referring to functions, variables, or other BUKKITs, are generically called “slots”.

### Declaration

To create an empty object within the current object's scope:

```
I HAS A <object> ITZ A BUKKIT
```

This object will have the default behavior of all bukkits.

### Slot Creation / Assignment

One of the most obvious thing to do with a bukkit it place something in a slot.

```
<object> HAS A <slotname> ITZ <expression>
 ```

A slot may be declared/initialized more than once, however doing so only changes the value the slot references.

This places the value returned from expression (could be another object) into the slot identified by slotname. The slot name may be any identifier (or SRS BIZNUS cast). Note: This identifier may be a function. Example:

```
HOW IZ I blogin YR stuff
	VISIBLE stuff
IF U SAY SO

<object> HAS A blogin ITZ blogin
```

### Functions

To declare a function inside a bukkit's slot

```
HOW IZ <object> <slot> (YR <argument>)*
( <statements> )*
IF U SAY SO
```

So, the above code becomes

```
HOW IZ <object> blogin YR stuff
	VISIBLE stuff
IF U SAY SO
```

### Scope

Functions operate differently in the context of bukkits. When a function is called from an object, some scope rules and variable resolution change.

When an identifier is used in a function, the variable is looked up in the following manner:

* The function namespace
* The calling object's namespace (if called from object)
* The “global” namespace

*IT is always looked up from global namespace*

The function namespace is made up of all arguments and any variable declared using the identifier “I” it resides within the function's namespace.

```
HOW IZ I fooin YR bar
     BTW bar is on function namespace
     I HAS A bar2
     BTW bar2 is on the function namespace
IF U SAY SO
```

ME is an identifier used to access the calling object of a function. If there is no calling object, access to ME throws an exception.

Declaring a variable on the calling objects namespace is done as follows:

```
HOW IZ I fooin YR bar
    ME HAS A bar2
    BTW bar2 is now a slot on calling object
IF U SAY SO
```

ME can also be used to explicitly use a slot variable vs. a function namespace variable.

```
HOW IZ I fooin YR bar
     ME'Z bar R bar
     BTW sets calling object's bar slot to bar value
IF U SAY SO
```

### Alternate Syntax

There is an alternate way to define a new object/bukkit

```
O HAI IM <object> [IM LIEK <parent>]
  <code-block>
KTHX
```

Anything “I” inside the codeblock actually refers to <object>. This can simplify syntax, eg:

```
O HAI IM pokeman
	I HAS A name ITZ "pikachu"
	HOW IZ I pikachuin YR face
		BTW DEFINE
	IF U SAY SO
KTHX
```

Identifiers within the O HAI block are looked up via slot-access first. If they are not found, the global scope is then searched. If that fails, then an error is thrown.

### Slot Access

Bukkit slots are accessed using the slot operator ”-”.

```
<object> 'Z <slotname>
 ```

 or indirectly using the Srs operator

```
<object> 'Z SRS <expression>
```

Slot access is very important to function calls.  To call a function on an object:

```
<object> IZ <slotname> (YR <variable> (AN YR <variable)*)? MKAY
```

combined with the Srs operator allows the following:

```
HOW IZ I getin YR object AN YR varName
	I HAS A funcName ITZ SMOOSH "get" AN varName MKAY
	FOUND YR object IZ SRS funcName MKAY
IF U SAY SO
```

This will call get<varName> on object.

### Special Slots

Every bukkit contains a few slots that have special meaning

* parent
* omgwtf
* izmakin

`parent` refers to a bukkit's “parent” object and is described below.

`omgwtf` refers to a method that is called when slot access fails. This method should return a variable (that will be placed in the unknown slot) or throw an exception. The default implementation of canhas is to always throw an exception.

`izmakin` refers to a method that will be run after a bukkit is fully prototyped but before the prototyping method returns. This allows a bukkit creator to perform some logic every time that bukkit is prototyped, and guarantees a “well-formed” bukkit.

### Inheritance / Prototyping

To create an object based upon an existing object:

```
I HAS A <object> ITZ LIEK A <parent>
```

Behavior of this sort of inheritance is described further below.

To define inheritance using alternate syntax, do the following.

```
O HAI IM <object> [IM LIEK <parent>]
  <code block>
KTHX
```

Inheritance implies a few things, one of which is inheritance of slots (described below). Another thing inheritance does is automatically create a “parent” slot on the new object. The “parent” slot refers to the object that this object was inherited from, or its prototype. The parent slot is treated specially by the Bukkit. An interesting side effect of this is that a Bukkit may change its “parent”/“prototype” by changing its parent slot. More on this later.

#### Inheritance of slots

Declaring a variable within the current object adds that variable to the object.

Accessing a variable from within the current object looks for that variable within the current object. If it is not found, it searches for the variable within the parent object (using the parent slot), and on up the chain of parents until it reaches an object where the parent slot is NOOB or it reaches a parent object is has already searched before.

Assigning a variable within the object first searches for it within the current object. If it has been declared within the current object, then it is set. If that fails, it attempts to access it within the parent object. Search continues in up the chain of parents. If the variable name is found up the inheritance chain, then that variable is declared and created within the current object (where the search started), and the value is set. If the variable search fails and the variable was never previously assigned, then it's a declaration error.

In this way, a child object has all of the values and methods of its ancestors, unless it replaces them within itself.

#### Functions and Inheritance

No matter where a FUNKSHUN is stored in a slot, during a Slot-Access Function call, the Function obtains variables from the object it was accessed from.

```
<object> IZ <functionSlotName> MKAY
```

In this case, the function will pull variables from <object>.

SO:

```
HOW IZ I funkin YR shun ?
	VISIBLE SMOOSH prefix AN shun MKAY
IF U SAY SO

O HAI IM parentClass
	I HAS A prefix ITZ "parentClass-"
	I HAS A funkin ITZ funkin    BTW Pulls funk from global scope
KTHX


O HAI IM testClass IM LIEK parentClass
	I HAS A prefix ITZ "testClass-"
KTHX

parentClass IZ funkin YR "HAI" MKAY        BTW parentClass-HAI
testClass IZ funkin YR "HAI" MKAY            BTW testClass-HAI
```

#### Mixin Inheritance

LOLCode supports a form of multiple-inheritance (I'm calling mixin-inheritance) via a new use of the SMOOSH operator.

```
I HAS A <object> ITZ A <parent> SMOOSH <mixin> (AN <mixin>)*
```

or

```
O HAI IM <object> IM LIEK <parent> SMOOSH <mixin> (AN <mixin>)*
    <statement-block>
KTHX
```

A mixin may be any bukkit type. When declaring a new object using mixins, All slots defined on the mixin are copied into the newly construct bukkit in reverse order of declaration. So

```
I HAS A ZipFileRiver ITZ A River SMOOSH FileStuffz AN ZipStuffz
```

This copies all slots from ZipStuffz into ZipFileRiver, then all slots from FileStuffz into ZipFileRiver, then replaces the parent slot with a reference to River.

Mixin-Inheritance is static. It can only pull in slots that are defined when the mixin takes place. If the FileStuffz or ZipStuffz objects change after the ZipFileRiver object is defined, the ZipFileRiver class does not see the change.

Here's a method of performing Mixin-Inheritance after a bukkit has been created.

Example:

```
BTW burger is on the stack

I HAS A cheezburger ITZ A burger SMOOSH cheeze


BTW Let's do the same thing, but assume cheezburger2 already exists

BTW Makes sure all of cheeze and its parent slots are copied into slice
I HAS A slice ITZ A bukkit SMOOSH cheeze

slice'Z parent R burger'Z parent
BTW slice & burger are now commons sub-classes

cheezburger2'Z parent R slice
BTW cheezburger2 now has same functionality as cheezburger
```
