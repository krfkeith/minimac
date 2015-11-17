<font color='green' size='5'>minimac - a minimalist macro processor</font><br />



# 1 Introduction #

## 1.1 Synopsis ##

> Thanks for you interest in minimac.

> Minimac is a minimalist macro processor that is intended as a front end preprocessor for little language compilers.

> Minimac is open source software. The project's home page on Google Code is http://code.google.com/p/minimac/

> Minimac is written in C, and distributed under a BSD license.

## 1.2 Current Status ##

> This project was started in April 2009. The software is currently in alpha release, it meets the author's current needs. Changes or additions may be made as flaws are found or new user requirements are submitted.

> This Wiki is an initial draft of the users manual.

> All feedback is very welcome, I've set up a Google Group for discussing the project: http://groups.google.com/group/minimac-discuss

## 1.3 Notation ##

> In this manual all output from minimac is prefixed with '**=>**'.

## 1.4 Alternatives to minimac ##

> If minimac is not suited to your needs, the following open source macro-processors are possible alternatives:
    * Gnu m4  http://www.gnu.org/software/m4/
    * ML/1    http://www.ml1.org.uk/
    * Gema    http://gema.sourceforge.net/new/index.shtml
    * gpp     http://en.nothingisreal.com/wiki/GPP

> The Wikipedia page on General Purpose Macro Processors gives an overview of this category of software: http://en.wikipedia.org/wiki/General_purpose_macro_processor

# 2 Invoking minimac #

> Minimac consists of a single OS-independent Ansi C source file, the naming convention is <i>mm-<version info>.c</i>
> To compile minimac refer to your C compiler instructions, a typical compile on a Unix based sytem would look something like this:
<tt>
<blockquote>cc -Wall -o mm mm-0.2.2-alpha.c<br>
</tt></blockquote>

> The minimac executable is named 'mm'. Minimac reads from standard input and writes the processed text to standard output, error messages are written to standard error.
> Minimac currently does not recognize any command-line switches or options.

> Minimac exits 0 on success, and 1 if an error occurs.

> Since Minimac is a filter program, a good way to learn minimac is to interactively type the examples in this manual from your shell console into Minimac's standard input.

# 3 Defining macros #

## 3.1 Defining a basic substitution macro ##

> A macro definition is composed of a name and a body.
> A macro definition begins with the backquote character **`** (upper left key on most keyboards) followed by the macro name. A second backquote begins the body of the definition, and a third backquote ends the definition.

> The following example defines the macro 'foo' that expands to `Hello world!'

<tt><b>
<blockquote><code>`</code>foo<code>`</code>Hello World!<code>`</code><br />
=><br />
<br />
foo<br />
=>Hello World!<br />
</b></tt></blockquote>

> Any text between the first space after end of the name and the body of the definition is ignored. The following examples are equivalent:

<tt><b>
<blockquote><code>`</code>foo<code>`</code>Hello World!<code>`</code><br />
<code>`</code>foo <code>`</code>Hello World!<code>`</code><br />
<code>`</code>foo This is an example <code>`</code>Hello World!<code>`</code><br />
<code>`</code>foo ( -- ) <code>`</code>Hello World!<code>`</code><br />
</b></tt></blockquote>

> Macro names are case sensitive:

<tt><b>
<blockquote><code>`</code>foo<code>`</code>Hello<code>`</code><br />
=><br />
<br />
FOO<br />
=>FOO<br />
<br />
foo<br />
=>Hello<br />
</b></tt></blockquote>

> The diagnostic built-in **{.m}** displays a list of the names and current values of user-defined macros:

<tt><b>
<blockquote><code>`</code>1st<code>`</code>my first macro<code>`</code><br />
=><br />
<br />
<code>`</code>2nd<code>`</code>another macro<code>`</code><br />
=><br />
<br />
{.m}<br />
=>-------`<br />
=>Macros (2):<br />
=><code>d[0] =&gt; name[1st] value[my first macro]</code><br />
=><code>d[1] =&gt; name[2nd] value[another macro]</code><br />
=>-------<br />
</b></tt></blockquote>

> The **{toggle!}** built-in can be used to change the reserved character from the default of '**`**' to another character.

## 3.2 Redefining a macro ##

> A new definition of a macro replaces the previous definition from that point onwards.

<tt><b>
<blockquote><code>`</code>foo<code>`</code>Hello<code>`</code><br />
=><br />
<br />
foo<br />
=>Hello<br />
<br />
<code>`</code>foo<code>`</code>Goodbye<code>`</code><br />
=><br />
<br />
foo<br />
=>Goodbye<br />
</b></tt></blockquote>

## 3.3 Nested macros ##

> Macro expansion is delayed to the last possible moment.

<tt><b>
<blockquote><code>`</code>friend<code>`</code>George<code>`</code><br />
=><br />
<br />
<code>`</code>greet<code>`</code>Hello friend<code>`</code><br />
=><br />
<br />
greet<br />
=>Hello George<br />
<br />
<code>`</code>friend<code>`</code>Ann<code>`</code><br />
=><br />
<br />
greet<br />
=>Hello Ann<br />
</b></tt></blockquote>

## 3.4 Self-referential macros ##

> The caret '**`^`**' character in the body of a macro expands to the latest definition of the same name as the macro which contains it.

<tt><b>
<blockquote><code>`</code>greet<code>`</code>hello<code>`</code><br />
=><br />
<br />
greet<br />
=>hello<br />
<br />
<code>`</code>greet<code>`</code>Well ^ there!<code>`</code><br />
=><br />
<br />
greet<br />
=> Well hello there!<br />
</b></tt></blockquote>

> The **{self!}** built-in can be used to change the reserved character from the default of '**`^`**'.

## 3.5 Embedded macro names ##

> To expand a macro name that is adjacent to a non-whitespace character, place a dollar '**$**' character between the name and the non-whitespace character:

<tt><b>
<blockquote><code>`</code>what<code>`</code>car<code>`</code><br />
=><br />
<br />
<code>`</code>who<code>`</code>George<code>`</code><br />
=><br />
<br />
<code>`</code>thing<code>`</code>who$'s what<code>`</code><br />
=><br />
<br />
thing<br />
=>George's car<br />
</b></tt></blockquote>

> The **{embed!}** built-in can be used to change the reserved character from the default of '**$**' to another character.

## 3.6 Escaping ##

> The backslash '**\**' character is used as an escape character, i.e. the character immediately following the backslash is interpreted literally.
` [...STD there's more to it than this, a more thorough explanation is required here...] `

<tt><b>
<blockquote><code>`</code>test<code>`</code>bcd<code>`</code><br />
=><br />
<br />
<code>`</code>embedded<code>`</code>a$test$e<code>`</code><br />
=><br />
<br />
embedded<br />
=>abcde<br />
<br />
<code>`</code>escaped<code>`</code>a\$test\$e<code>`</code><br />
=><br />
<br />
escaped<br />
=>a$test$e<br />
</b></tt></blockquote>

> The **{escape!}** built-in can be used to change the reserved character from the default of '**\**' to another character.

` [...STD should we use a character other than '\' as the default escape character?...] `

## 3.7 Ignoring whitespace within a definition ##

> The left-bracket '**`[`**'  character within a definition causes runs of whitespace to be replaced by runs of the dollar '**$**' character. The right-bracket '**`]`**' resumes normal whitespace expansion.

> The **{wsoff!}** and **{wson!}** built-ins can be used to change the reserved characters from their defaults of '**`[`**' and '**`]`**' to other characters.

` [...STD should we use a character other than brackets as the default characters?...] `

## 3.8 Quoting ##

> A string surrounded by the tilde character '**~**' is output as a literal string (i.e. without expansion, but with escaping).

<tt><b>
<blockquote><code>`</code>hello<code>`</code>hi there!<code>`</code><br />
=><br />
~hello~<br />
=>hello<br />
</b></tt></blockquote>

> The **{quote!}** built-in can be used to change the reserved character from the default of '**~**' to another character.

# 4 Built-in functions #

## 4.1 How to use to built-ins ##

> Built-in are special names that invoke useful behavior or side-effects when expanded. For example the increment built-in, whose name is '**{++}**', is used to increment a number.

## 4.2 The macro argument stack ##

> When a built-in requires one or more arguments, these are taken from the macro argument stack.

> The contents of the argument stack can be displayed at any time for diagnostic purposes by invoking the '**{.s}**' built-in.

<tt><b>
<blockquote>{.s}<br />
=><br />
=>-------<br />
=>Items on the stack (0):<br />
=>-------<br />
</b></tt></blockquote>

> To push an item, usually a token, onto the stack, append the ampersand character '**&**' to it, whitespace immediately following the '**&**' is ignored.

<tt><b>
<blockquote>5&<br />
=><br />
<br />
{.s}<br />
=><br />
=>-------<br />
=>Items on the stack (1):<br />
<code>=&gt;stack[0] =&gt; [5]</code><br />
=>-------<br />
</b></tt></blockquote>

> Macro names can be pushed onto the stack:

<tt><b>
<blockquote><code>`</code>foo<code>`</code>this is just a test<code>`</code><br />
=><br />
<br />
foo&<br />
=><br />
<br />
{.s}<br />
=><br />
=>-------<br />
=>Items on the stack (2):<br />
<code>=&gt;stack[1] =&gt; [foo]</code><br />
<code>=&gt;stack[0] =&gt; [5]</code><br />
=>-------<br />
</b></tt></blockquote>

> Use the expand `**{?}**' built-in to pop the top item off and expand it.

<tt><b>
<blockquote>{?}<br />
=>this is just a test<br />
<br />
{.s}<br />
=><br />
=>-------<br />
=>Items on the stack (1):<br />
<code>=&gt;stack[0] =&gt; [5]</code><br />
=>-------<br />
<br />
{?}<br />
=>5<br />
<br />
{.s}<br />
=>-------<br />
=>Items on the stack (0):<br />
=>-------<br />
</b></tt></blockquote>

> Literal (i.e. unexpanded) strings can also be pushed onto the stack:

<tt><b>
<blockquote>~another test~&<br />
=><br />
{.s}<br />
=><br />
=>-------<br />
=>Items on the stack (1):<br />
<code>=&gt;stack[0] =&gt; [another test]</code><br />
=>-------<br />
</b></tt></blockquote>

> The **{push!}** built-in can be used to change the reserved character from the default of '**&**' to another character.

## 4.3 Stack effect notation ##

> The following section is a catalog of the minimac's built-ins, included with each built-in's description is a stack effect diagram, in a notation similar to that used by the Forth language, a stack-based language which inspired much of minimac's philosophy and design.

> The stack effect diagram documents the items that are required to be on the stack before the built-in is invoked, and the after-effect on the stack of the built-in's invocation.

> For example, the stack effect for the '**{+}**' built-in is '( n1 n2 -- n1+n2 )', meaning that it expects two (decimal) integers on the stack, which it will remove, and leave their sum in their place.

> In both the before and after sections of the stack diagram the rightmost item is considered to be at the top of the stack.

> The stack effect diagram '( -- )' denotes a built-in that has no expectations of the stack nor after-effects on the stack.

# 5 Catalog of built-ins #

## 5.1 Integer arithmetic ##

|<b>operator</b>|<b>stack effect</b>|<b>description</b>|
|:--------------|:------------------|:-----------------|
|<b>{+}</b>     |( n1 n2 -- n1+n2 ) |add               |
|<b>{-}</b>     |( n1 n2 -- n1-n2 ) |subtract          |
|<b>{<code>*</code>}</b>|( n1 n2 -- n1\*n2 )|multiply          |
|<b>{/}</b>     |( n1 n2 -- n1/n2 ) |divide            |
|<b>{%}</b>     |( n1 n2 -- n1%n2 ) |mod               |

> Example:

<tt><b>
<blockquote>5& 3& {.s}<br />
=><br />
=>-------<br />
=>Items on the stack (2):<br />
<code>=&gt;stack[1] =&gt; [3]</code><br />
<code>=&gt;stack[0] =&gt; [5]</code><br />
=>-------<br />
<br />
{+} {.s}<br />
=><br />
=>-------<br />
=>Items on the stack (1):<br />
<code>=&gt;stack[0] =&gt; [8]</code><br />
=>-------<br />
</b></tt></blockquote>

### {++} - increment ###

> {++} ( n -- n+1 ) or ( name -- )

> If the top stack item is a name, its body is incremented,
> otherwise the top stack item is treated as a literal and incremented in place.

> Example:
> > ...coming soon

### {--} - decrement ###


> {--} ( n -- n-1 ) or ( name -- )

> If the top stack item is a name, its body is decremented,
> otherwise the top stack item is treated as a literal and decremented in place.

> Example:
> > ...coming soon

## 5.2 Integer comparison ##


> All comparison operators return canonical truth values, -1 for true, 0 for false.

|<b>operator</b>|<b>stack effect</b>|<b>description</b>| |
|:--------------|:------------------|:-----------------|:|
|<b>{=}</b>     |( n1 n2 -- -1|0 )  |equal             |
|<b>{<}</b>     |( n1 n2 -- -1|0 )  |less-than         |
|<b>{<=}</b>    |( n1 n2 -- -1|0 )  |less-than or-equal|
|<b>{>}</b>     |( n1 n2 -- -1|0 )  |greater-than      |
|<b>{>=}</b>    |( n1 n2 -- -1|0 )  |greater-than or-equal|

## 5.3 Variables ##

### {@} - fetch ###

> {@} ( name -- value )

> Fetch the value of a macro variable.

### {!} - store ###

> {!} ( value name -- )

> Store a value into a macro variable.

## 5.4 Control flow ##

### {case} - case conditional ###

> {case} ( x y action -- ???|x )

> The {case} built-in is the building block for constructing what are variously known as case or switch statements, where a value x is compared to various values and if a match is found a corresponding block of code is executed.

> If x and y are identical, removes x and y from the stack, and expands the action string at the top of stack, the remainder of the enclosing definition is no longer expanded. Conversely, if x and y do not match, x remains on the stack and expansion of the remainder of the enclosing definition resumes.

> Example:
<tt><b>
<blockquote><code>`</code>test ( x -- )<code>`</code><code>[</code> a& this& {case} b& that& {case} other (${.}$) <code>]</code><code>`</code><br />
=><br />
<br />
<code>`</code>this<code>`</code>I just performed the first case's action<code>`</code><br />
=><br />
<br />
<code>`</code>that<code>`</code>I just performed the second case's action<code>`</code><br />
=><br />
<br />
<code>`</code>other<code>`</code>No matching case was found<code>`</code><br />
=><br />
<br />
a& test<br />
=>I just performed the first case's action<br />
<br />
b& test<br />
=>I just performed the second case's action<br />
<br />
z& test<br />
=>No matching case was found (z)<br />
<br />
</b></tt></blockquote>

### {continue} - conditional expansion ###

> {continue} ( x -- x| )

> Expands the rest of the definition only if the integer value of x is not zero. Conversely if x is zero, x is dropped from the stack, and the remainder of the enclosing definition is no longer expanded.

> Example: See {repeat}

### {exit} - exiting to the OS ###

> {exit} ( n -- ) or {exit} ( -- )

> Exits to the OS using the top of stack as the exit code. If the stack is empty when {exit} is invoked a default exit code of 0 is used.

### {?} - expand ###

> {?} ( action -- ??? )

> Expand the top of stack.

> See also:
    * {?>} - expand to stack

### {?>} - expand to stack ###

> {?>} ( action -- ??? output )

> Expands the top of stack, all output intended for stdout (or the current diversion) is instead collated in a buffer which is then pushed onto the stack.

> Example:
<tt><b>
<blockquote><code>`</code>enquote ( s -- "s" ) <code>`</code>~"${?}$"~& {?>}<code>`</code><br />
=><br />
<br />
~hello there~& enquote {.s}<br />
=><br />
=>-------<br />
=>Items on the stack (1):<br />
<code>=&gt;stack[0] =&gt; ["hello there"]</code><br />
=>-------<br />
</b></tt></blockquote>

> See also:
    * {?} - expand

### {if} - if conditional ###

> {if} ( n action -- ???| )

> Expands the action string on the top of the stack if n is non-zero.

> Example:
<tt><b>
<blockquote><code>`</code>HI<code>`</code>Hello!<code>`</code><br />
=><br />
<br />
<code>`</code>?HI<code>`</code>HI& {if}<code>`</code><br />
=><br />
<br />
-1& ?HI<br />
=>Hello!<br />
<br />
0& ?HI<br />
=><br />
</b></tt></blockquote>

### {ifelse} - if-else conditional ###

> {ifelse} ( n if-action else-action -- ??? )

> Expands the if-action if n is non-zero, otherwise expands the else-action.

> Example:
<tt><b>
<blockquote><code>`</code>HI<code>`</code>Hello!<code>`</code><br />
=><br />
<br />
<code>`</code>BYE<code>`</code>Goodbye!<code>`</code><br />
=><br />
<br />
<code>`</code>GREET<code>`</code>HI& BYE& {ifelse}<code>`</code><br />
=><br />
<br />
-1& GREET<br />
=>Hello!<br />
<br />
0& GREET<br />
=>Goodbye!<br />
</b></tt></blockquote>

### {repeat} - looping, tail recursion ###

> {repeat} ( -- )

> Repeats the expansion of the enclosing definition.

> Example:

<tt><b>
<blockquote><code>`</code>stars ( u -- ) <code>`</code><code>[</code> {continue} {--} <code>*</code> {repeat} <code>]</code><code>`</code><br />
=><br />
<br />
3& stars<br />
=><code>***</code><br />
</b></tt></blockquote>

### {?repeat} - conditional looping, tail recursion ###

> {?repeat} ( n -- )

> Repeats the expansion of the enclosing definition if the top of stack is non-zero.

## 5.5 Input and Output ##

### {.} - print ###

> {.} ( str -- )

> Print the top of stack without expansion.

> See also:
    * {?} - expand

### {divert} - output diversion ###

> {divert} ( filename -- )

> Temporarily diverts the output stream to a file. Overwrites the file if it already exists.

> See also:
    * {divert+} - output diversion append
    * {?>} - expand to stack

### {divert+} - output diversion append ###

> {divert+} ( filename -- )

> Temporarily diverts the output stream to a file. Appends to the file if it already exists.

> See also:
    * {divert} - output diversion

### {include} - file inclusion ###

> {include} ( filename -- )

> Causes the file named to be read and expanded.

### {undivert} - cease current output diversion ###

> {undivert} ( -- )

> Ceases the most recent output diversion started by {divert} or {divert+}.

### {verbatim} - file inclusion (without expansion) ###

> {verbatim} ( filename -- )

> Causes the file named to be read and sent directly to the output stream without expansion.

## 5.6 Parameter Stack ##

|<b>operator</b>|<b>stack effect</b>|<b>description</b>|
|:--------------|:------------------|:-----------------|
|<b>{drop}</b>  |( x -- )           |drop top of stack |
|<b>{dup}</b>   |( x -- x x )       |duplicate top of stack|
|<b>{?dup}</b>  |( x|0 -- x| )      |duplicate top of stack if non-zero|
|<b>{over}</b>  |( x1 x2 -- x1 x2 x1 )|duplicate next of stack|
|<b>{swap}</b>  |( x1 x2 -- x2 x1 ) |swap the top two items of the stack|

## 5.7 Auxiliary stack ##

|<b>{pop}</b>|( -- x ) aux:( x -- )|pop from auxiliary stack|
|:-----------|:--------------------|:-----------------------|
|<b>{push}</b>|( x -- ) aux:( -- x )|push to auxiliary stack |

## 5.8 Strings ##

### {append} - append to a string variable ###

> {append} ( str name -- )

### {cat} - concatenate strings ###

> {cat} ( str1 str2 -- str1str2 )

### {len} - string length ###

> {len} ( str -- u )

### {lower} - convert to lowercase ###

> {lower} ( str -- str' )

### {replace} - substring replacement ###

> {replace} ( str fromStr toStr  -- str' )

> Replaces all occurences of _fromStr_ within _str_ to _toStr_.

### {same?} - test for string equality ###

> {same?} ( str1 str2 -- 0|-1 )

### {substr} - extract a substring ###

> {substr} ( str start length -- str' )

### {tr} - translate characters ###

> {tr} ( str from to -- str' )

### {upper} - convert to uppercase ###

> {upper} ( str -- str' )

## 5.9 Bitwise ##

> If provided with canonical truth values, i.e. -1 for true and 0 for false, the bitwise operators can also serve as logical operators.

|<b>operator</b>|<b>stack effect</b>|<b>description</b>|
|:--------------|:------------------|:-----------------|
|<b>{and}</b>   |( u1 u2 -- u1&u2 ) |bitwise and       |
|<b>{not}</b>   |( u -- ~u )        |bitwise not       |
|<b>{or}</b>    |( u1 u2 -- u1`|`u2 )|bitwise or        |
|<b>{xor}</b>   |( u1 u2 -- u1`|`u2 )|bitwise xor       |
|<b>{>>}</b>    |( u1 u2 -- u1`>>`u2 )|bitwise shift right|
|<b>{<<}</b>    |( u1 u2 -- u1'<<'u2 )|bitwise shift left|

## 5.10 Diagnostic utilities ##

> All diagnostic utilities write their output to the standard error stream.

### {.a} - display auxiliary stack contents ###

> {.a} ( -- )

### {.m} - display macro definitions ###

> {.m} ( -- )

### {.s} - display stack contents ###

> {.s} ( -- )

## 5.11 Reserved special characters ##

> Operators that for checking and changing minimac's reserved special characters.

|<b>operator</b>|<b>stack effect</b>|<b>description</b>|
|:--------------|:------------------|:-----------------|
|<b>{embed@}</b>|(-- c)             |fetch <i>embed</i> character|
|<b>{embed!}</b>|( c --)            |store <i>embed</i> character|
|<b>{escape@}</b>|(-- c)             |fetch <i>escape</i> character|
|<b>{escape!}</b>|( c --)            |store <i>escape</i> character|
|<b>{push@}</b> |(-- c)             |fetch <i>literal push</i> character|
|<b>{push!}</b> |( c --)            |store <i>literal push</i> character|
|<b>{quote@}</b>|(-- c)             |fetch <i>quote</i> character|
|<b>{quote!}</b>|( c --)            |store <i>quote</i> character|
|<b>{self@}</b> |(-- c)             |fetch  <i>self-reference</i> character|
|<b>{self!}</b> |( c --)            |store s<i>elf-reference</i> character|
|<b>{toggle@}</b>|(-- c)             |fetch <i>define toggle</i> character|
|<b>{toggle!}</b>|( c --)            |store <i>define toggle</i> character|
|<b>{wsoff@}</b>|(-- c)             |fetch <i>whitespace off</i> character|
|<b>{wsoff!}</b>|( c --)            |store <i>whitespace off</i> character|
|<b>{wson@}</b> |(-- c)             |fetch <i>whitespace on</i> character|
|<b>{wson!}</b> |( c --)            |store <i>whitespace on</i> character|

## 5.12 Miscellaneous ##

### {#} - comment ###

> {#} ( -- )

> Treats the remainder of the line as a comment.

### {env} - get the value of an OS environment variable ###

> {env} ( env-var -- value )

### {last} - last dictionary definition ###

> {last} ( -- name )

> Returns the name of the most recent user-defined macro.

### {remove} - file deletion ###

> {remove} ( filename -- )

### {shell} - execute an operating system command ###

> {shell} ( str -- return-code )

### {undefine} - remove a user-defined macro from the dictionary ###

> {undefine} ( macroname -- )

### {version} - version string ###

> {version} ( -- str )

> Pushes a string containing the minimac processor's version.

# 6 Sample Extensions #

> Examples of possible extension macros are supplied in the _extend-<version info>.mm_ file available for download at http://code.google.com/p/minimac/downloads/list

# 7 Sample macros #

> The classic '99 bottles of beer' example is available for download in the file _99bottles.mm at:
> http://code.google.com/p/minimac/downloads/list_

> Examples are also available in the _examples_ wiki page at: http://code.google.com/p/minimac/w/list

# 8 Quirks and limitations #

  * assumes two's complement arithmetic
  * names are limited to 64 characters

# Appendix A - <b>minimac</b> Quick Reference #

## Special characters: ##

|<b>default</b>|<b>function</b>|<b>storing</b>|<b>retrieving</b>|<b>affects definition?</b>|<b>affects expansion?</b>|
|:-------------|:--------------|:-------------|:----------------|:-------------------------|:------------------------|
|<b>`</b>      |define         |{toggle!}     |{toggle@}        |yes                       |yes                      |
|<b>$</b>      |embed          |{embed!}      |{embed@}         |no                        |yes                      |
|<b>\</b>      |escape         |{escape!}     |{escape@}        |yes                       |yes                      |
|<b>&</b>      |push           |{push!}       |{push@}          |no                        |yes                      |
|<b>^</b>      |self-reference |{self!}       |{self@}          |yes                       |no                       |
|<b>~</b>      |quote          |{quote!}      |{quote@}         |yes                       |yes                      |
|<b><code>[</code></b>|white space off|{wsoff!}      |{wsoff@}         |yes                       |no                       |
|<b><code>]</code></b>|white space on |{wson!}       |{wson@}          |yes                       |no                       |

## Buit-ins: ##

...unfinished see rest of manual...

## Extensions: ##

...unfinished see rest of manual...

<br />
> ...unfinished - more to come

# Appendix B - BSD Documentation License #

Redistribution and use in source (text) and 'compiled' forms (SGML, HTML, PDF, PostScript, RTF and so forth) with or without modification, are permitted provided that the following conditions are met:

  1. Redistributions of source code (text) must retain the above copyright notice, this list of conditions and the following disclaimer as the first lines of this file unmodified.
  1. Redistributions in compiled form (transformed to DTDs, converted to PDF, PostScript, RTF and other formats) must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.

THIS DOCUMENTATION IS PROVIDED BY THE COPYRIGHT HOLDER "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

This notice shall appear on any product containing this material.


---

**minimac - a minimalist macro processor**

_Copyright (c) 2009 Mark W. Humphries. All rights reserved._