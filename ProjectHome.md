Minimac is a minimalist general purpose text macro processor, its simplicity should make it particularly well suited as a front end preprocessor for little language compilers.

Project objectives and salient features:
  * simpler to use than m4
  * explicit argument stack, user functions are defined by concatenation (similar to the Forth language)
  * delays macro expansion to the last possible moment.

Minimac is written in ANSI C, the current source can be downloaded here:
http://code.google.com/p/minimac/downloads/list

The software is currently in alpha release, it meets the author's current needs. Changes or additions may be made as flaws are found or new user requirements are submitted.

The draft user manual is currently maintained as a wiki page here: http://code.google.com/p/minimac/wiki
Once it stabilizes I'll convert it to pdf.

Release announcements are posted on the project's Freshmeat page: http://freshmeat.net/projects/minimac-macro-processor

Questions, feedback, and offers to help are most welcome and can be posted on the project's Google Group: http://groups.google.com/group/minimac-discuss
If you have trouble figuring out how to code a particular macro functionality with minimac do not hesitate to ask.

Example 1: Simple interactive usage (Minimac's output has been prefixed with '=>'):

<tt><font size='3'>
<blockquote><i>define the macro <b>greet</b> to expand to 'Hello friend`:<br /></i>
<b><code>`</code>greet<code>`</code>Hello friend<code>`</code><br />
=><br />
<br /></b>
<i>test the macro <b>greet</b>:<br /></i>
<b>greet<br />
=>Hello friend<br />
<br /></b>
<i>define the macro <b>friend</b> to expand to 'George':<br /></i>
<b><code>`</code>friend<code>`</code>George<code>`</code><br />
=><br />
<br /></b>
<i>test the macro <b>friend</b>:<br /></i>
<b>friend<br />
=>George<br />
<br /></b>
<i>the expansion of <b>greet</b> now takes into account that friend is now a macro:<br /></i>
<b>greet<br />
=>Hello George<br />
<br /></b>
<i>redefine the macro <b>friend</b> to expand to 'Ann':<br /></i>
<b><code>`</code>friend<code>`</code>Ann<code>`</code><br />
=><br />
<br /></b>
<i>test the redefined macro <b>friend</b>:<br /></i>
<b>friend<br />
=>Ann<br />
<br /></b>
<i>the expansion of <b>greet</b> now takes into account that friend has been redefined:<br /></i>
<b>greet<br />
=>Hello Ann<br /></b>
</font></tt></blockquote>

Example 2: 99 bottles of beer (http://99-bottles-of-beer.net/)

> `````<b>drink!</b> ( u -- ) `````{continue}${#}<br />
> this-many-bottles of beer on the wall, this-many-bottles of beer.<br />
> Take one down and pass it around, fewer-bottles of beer on the wall.<br />

> {repeat}`````

> `````<b>oh-no!</b> ( -- ) `````No more bottles of beer on the wall, no more bottles of beer.`````

> `````<b>please!</b> ( u -- u ) `````Go to the store and buy some more, this-many-bottles of beer on the wall.`````

> `````<b>this-many-bottles</b> ( u -- u ) `````[{dup} 0& ~no more bottles~& {case} 1& ~1 bottle~& {case} {.} ~ bottles~]`````

> `````<b>fewer-bottles</b> ( u -- u-1 ) `````{--}$this-many-bottles`````

> 99& <b>drink!</b><br />
> <b>oh-no!</b><br />
> 99& <b>please!</b><br />