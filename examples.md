<font color='green' size='5'>minimac - usage examples</font><br />



# 1 Introduction #

> The purpose of this guide is to help you get a feel for writing macros with minimac. The examples are contrasted with other open source macro processors to help you explore which language might be best suited to your specific needs. The different macro processors each have their inherent advantages and disadvantages which may or may not be relevant to your application.

> _Note: the initial examples are likely to be overly contrived, I hope to eventually replace them with ones that better illustrate the salient features of each tool._

> The minimac examples are usually given in three contrasting versions:

  * a version that uses variables to the exclusion of the minimac's explicit stack
  * a version that mixes variable and stack usage
  * a version that emphasizes usage of minimac's the explicit stack

> Minimac macros that affect the explicit stack have stack comments of the form: '( before -- after )'.

> The open source macro languages used for comparison are:

  * Gnu m4 http://www.gnu.org/software/m4/
  * ML/1 http://www.ml1.org.uk/
  * Gema http://gema.sourceforge.net/new/index.shtml
  * gpp http://en.nothingisreal.com/wiki/GPP

# 2 examples #

## 2.1 simple text replacement and counter ##

> This example uses the generation of HTML headers to illustrate simple text replacement and how to implement a counter.

### 2.1.1 reference ###

> http://en.wikipedia.org/wiki/M4_(computer_language)

### 2.1.2 m4 version ###

<tt>
<blockquote>define(<code>`</code>H2_COUNT', 0)dnl<br />
=><br />
<br />
define(<code>`</code>H2', <code>`</code>define(<code>`</code>H2_COUNT', incr(H2_COUNT))'dnl<br />
<code>`</code><code>&lt;h2&gt;</code>H2_COUNT. $1<code>&lt;/h2&gt;</code>')dnl<br />
=><br />
<br />
H2(First Section)<br />
=><code>&lt;h2&gt;1. First Section&lt;/h2&gt;</code><br />
<br />
H2(Second Section)<br />
=><code>&lt;h2&gt;2. Second Section&lt;/h2&gt;</code><br />
<br />
H2(Conclusion)<br />
=><code>&lt;h2&gt;3. Conclusion&lt;/h2&gt;</code><br />
</tt></blockquote>

### 2.1.3 minimac versions ###

#### 2.1.3.1 variables-oriented version ####

<tt>
<blockquote><code>`</code>H2_COUNT<code>`</code>0<code>`</code><br />
=><br />
<br />
<code>`</code>H2_TEXT<code>`</code><code>&lt;h2&gt;</code>$H2_COUNT$. CONTENT$<code>&lt;/h2&gt;</code><code>`</code><br />
=><br />
<br />
<code>`</code>H2<code>`</code>H2_COUNT& {++}$H2_TEXT<code>`</code><br />
=><br />
<br />
<code>`</code>CONTENT<code>`</code>First Section<code>`</code> H2<br />
=><code>&lt;h2&gt;1. First Section&lt;/h2&gt;</code><br />
<br />
<code>`</code>CONTENT<code>`</code>Second Section<code>`</code> H2<br />
=><code>&lt;h2&gt;1. First Section&lt;/h2&gt;</code><br />
<br />
<code>`</code>CONTENT<code>`</code>Conclusion<code>`</code> H2<br />
=><code>&lt;h2&gt;1. Conclusion&lt;/h2&gt;</code><br />
</tt></blockquote>

#### 2.1.3.2 hybrid version ####

<tt>
<blockquote><code>`</code>H2_TEXT<code>`</code><code>&lt;h2&gt;</code>$H2_COUNT$. {?}$<code>&lt;/h2&gt;</code><code>`</code><br />
=><br />
<br />
<code>`</code>H2_COUNT<code>`</code>0<code>`</code><br />
=><br />
<br />
<code>`</code>H2 ( title -- ) <code>`</code>H2_COUNT& {++}$H2_TEXT<code>`</code><br />
=><br />
<br />
First\ Section& H2<br />
=><code>&lt;h2&gt;1. First Section&lt;/h2&gt;</code><br />
<br />
Second\ Section& H2<br />
=><code>&lt;h2&gt;2. Second Section&lt;/h2&gt;</code><br />
<br />
Conclusion& H2<br />
=><code>&lt;h2&gt;3. Conclusion&lt;/h2&gt;</code><br />
</tt></blockquote>

#### 2.1.3.3 stack-oriented version ####

<tt>
<blockquote><code>`</code>H2 ( count title -- count+1 ) <code>`</code><code>&lt;h2&gt;</code>${over}${?}$. {?}${++}$<code>&lt;/h2&gt;</code><code>`</code><br />
<br />
<code>1&amp;</code> ~First Section~& H2<br />
=><code>&lt;h2&gt;1. First Section&lt;/h2&gt;</code><br />
<br />
~Second Section~& H2<br />
=><code>&lt;h2&gt;2. Second Section&lt;/h2&gt;</code><br />
<br />
Conclusion& H2<br />
=><code>&lt;h2&gt;3. Conclusion&lt;/h2&gt;</code><br />
</tt></blockquote>

## ...more examples to come... ##

# Appendix A - BSD Documentation License #

Redistribution and use in source (text) and 'compiled' forms (SGML, HTML, PDF, PostScript, RTF and so forth) with or without modification, are permitted provided that the following conditions are met:

  1. Redistributions of source code (text) must retain the above copyright notice, this list of conditions and the following disclaimer as the first lines of this file unmodified.
  1. Redistributions in compiled form (transformed to other DTDs, converted to PDF, PostScript, RTF and other formats) must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.

THIS DOCUMENTATION IS PROVIDED BY THE COPYRIGHT HOLDER "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

This notice shall appear on any product containing this material.


---

**minimac - a minimalist macro processor**

_Copyright (c) 2009 Mark W. Humphries. All rights reserved._