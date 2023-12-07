---
title: "Easy Sphinx Extensions with Nested Parsing"
date: 2023-12-06
tags: [ documentation, programming, sphinx, LaTeX ]
---

I've recently started the process of converting some software documentation
that was originally written for [LaTeX] and converting it to a newer
documentation system called [Sphinx]. I like the LaTeX implementation much,
but the Sphinx documentation provides some important advantages. First,
Sphinx is designed to provide html output that will be easier for users to
find and reference (while also providing pdf files similar to LaTeX).
Second, with the [Breathe] extension, Sphinx can pull documentation
directly from source code. Third, the [reStructuredText] format that Sphinx
uses is often easier to write than LaTeX format, particularly when dealing
with code segments.

However, one thing that Sphinx's reStructuredText format is missing is a
way to easily define new "commands" to introduce customized elements in
the text. LaTeX provides `\newcommand` and other ways to provide customized
commands and elements.

reStructuredText does not provide anything equivalent, but fortunately
Sphinx has a powerful extension mechanism that can associate Python code to
custom reStructuredText elements.

This blog post is not about how to define a Sphinx extension; the
[documentation from the Sphinx] project is a good place to start for that.
Rather, this blog addresses a specific part of Sphinx extensions that I
find particularly challenging. The extension routines must return a [tree
structure defining the elements of the document] (which in turn are used to
geneate output in various forms like html, tex, and so on). For a newbie
such as myself, it's more than I really want to learn.

A much easier approach is to have the Python code build new
reStructuredText. That is, just have the custom directive or role generate
the reStructuredText that should be inserted much like a LaTeX
`\newcommand` is defined by providing new LaTeX that will be inserted when
the command is executed.

Unfortunately, Sphinx is missing "nested parsing" in its extension
interface. This blog describes how to implement nested parsing in Sphinx.

## Nested Parse in a Directive

Sphinx extensions allow you to implement directives (commands on their own
line starting with `.. directivename::`) and roles (inline commands of the
form ``:rolename:`arguments` ``). (Other things, such as domains, can also
be defined in a Sphinx extension, but only directives and roles are covered
here.) Directives and roles are defined differently. This section will
describe directives.

All Sphinx extensions are defined as Python modules/files. The basics of
creating a directive extension are to create a subclass of
`docutils.parsers.rst.Directive` with a `run` method and then define a
`setup` function that registers that class as a directive. The details of
creating the extension are beyond the scope of this blog, but the [hello
world tutorial] in the Sphinx documentation is a good place to get started,
and the [docutils directive reference] contains some information on
implementing a `Directive` class.

The behavior of the directive is defined by the `run` method of the
`Directive` subclass. The `run` method takes the inputs to the directive
and returns a [docutils document tree]. This document tree is, in my
opinion, the difficult part of creating a directive. There are many
different node types to connect together in just the right way to work.
Frankly, I just don't want to learn it.

Like I said in the intro, I think it is much easier to construct a string
in reStructuredText and then replace that as the code for the directive.
Following some advice from [a stackoverflow post], I created this helper
class.

``` python
import docutils.nodes
import docutils.statemachine
import sphinx.util.nodes

class NestedParseNodes:
  def __init__(self):
    self.source = docutils.statemachine.ViewList()

  def add_line(self, sourceline, filename, linenumber):
    self.source.append(sourceline, filename, linenumber)

  def get_nodes(self, directive):
    node = docutils.nodes.section()
    node.document = directive.state.document

    sphinx.util.nodes.nested_parse_with_titles(directive.state,
                                               self.source,
                                               node)
    return node.children
```

To use this class, a directive `run` method constructs this parser and
calls `add_line` to add lines one at a time. When finished, the `get_nodes`
method returns the resulting nodes.

Here is an example that uses this class to implement a "Fun Fact" box.

``` python
import docutils.nodes
import docutils.parsers.rst

class FunFact(docutils.parsers.rst.Directive):
    has_content = True
    def run(self):
        current_source = self.state.document.current_source
        current_line = self.lineno
        parse = NestedParseNodes()
        parse.add_line('.. admonition:: Fun Fact', current_source, current_line)
        parse.add_line('', current_source, current_line)
        for line in self.content:
            parse.add_line('   ' + line, current_source, current_line)
        return parse.get_nodes(self)
```

The only other thing needed for the extension is to register this class as
directive in the `setup` method.

``` python
def setup(app):
    app.add_directive('funfact', FunFact)
```

Once you load this extension (by editing the extensions in the `conf.py`
class), a directive named `funfact` is added. Here is an example of its
use.

``` restructuredtext
.. funfact::

   You don't have to waste money on expensive binoculars. If you want
   something to appear bigger, just get closer.
```

Based on the implementation above, this will have a nested parse that
coverts this text to the following expression.

``` restructuredtext
.. admonition:: Fun Fact

   You don't have to waste money on expensive binoculars. If you want
   something to appear bigger, just get closer.
```

And this in turn creates output like the following.

<div class="admonition-fun-fact admonition"
     style="margin: 20px 0px;
            padding: 10px 30px; 
            background-color: #EEE;
            border: 1px solid #CCC;">
<p class="admonition-title">Fun Fact</p>
<p>You don’t have to waste money on expensive binoculars. If you want
something to appear bigger, just get closer.</p>
</div>

## Nested Parse in a Role

Similar to directives, a role in an extension is created by defining a
function that implements the role and then registering that function with
the role name. Documentation of implementing role extensions seems to be
missing from the Sphinx documentation, but [a blog by Doug Hellmann] does a
good job describing the process.

Once again, the role's function must return a [docutils document tree], but
it would be much easier to simply generate reStructuredText in a string.
The code provided for the nested parse in a directive does not work from
within a role, so a new helper is needed.

I couldn't find any suggestions online for this (perhaps my
Google-feng-shui was bad). In the end, I traced through the Sphinx Python
code until I found the section of code that seemed to implement the inline
parsing. I copied that code and made some modifications to create the
following convenience function.

``` python
def role_nested_parse(rawtext, lineno, inliner):
  remaining = rawtext
  processed = []
  unprocessed = []
  messages = []
  while remaining:
    match = inliner.patterns.initial.search(remaining)
    if match:
      groups = match.groupdict()
      method = inliner.dispatch[groups['start'] or groups['backquote']
                                or groups['refend'] or groups['fnend']]
      before, inlines, remaining, sysmessages = method(inliner, match, lineno)
      unprocessed.append(before)
      messages += sysmessages
      if inlines:
        processed += inliner.implicit_inline(''.join(unprocessed), lineno)
        processed += inlines
        unprocessed = []
      else:
        break
    remaining = ''.join(unprocessed) + remaining
    if remaining:
      processed += inliner.implicit_inline(remaining, lineno)
    return processed, messages
```

As you would expect, the function defined for the role can create a string
that provides the reStructuredText to insert at the role. This string along
with the line number and "inliner" object passed to the role's function are
given to the above `role_nested_parse` to generate the [docutils document
tree].

Below is a simple example implementing a role using this nested parse. It
is creating a role named `doi` that takes as an argument a digital object
identifier (DOI) and surrounds that in a link with a URL to the [DOI
Foundation's name resolver] that will take the user who clicks it to the
publication page of that DOI.

``` python
def doi_role(name, rawtext, text, lineno, inliner, options={}, content=[]):
    return sphinx_nested_parse.role_nested_parse(
        '`doi:%s <https://dx.doi.org/%s>`_' % (text, text), lineno, inliner)

def setup(app):
    app.add_role('doi', doi_role)
```

As an example, if the Sphinx document, using this extension, encounters
``:doi:`10.1007/978-3-319-20119-1_34` ``, it will internally create the
replacement text `` `doi:10.1007/978-3-319-20119-1_34
<https://dx.doi.org/10.1007/978-3-319-20119-1_34>`_``. This in turn will
create the following link in the code: <a class="reference external"
href="https://dx.doi.org/10.1007/978-3-319-20119-1_34">doi:10.1007/978-3-319-20119-1_34</a>

## Putting it Together

To make them easier to use, I've put together these two features in a
simple [sphinx_nested_parse.py] Python file. This file can be loaded as a
Python module and used within a Sphinx extension such as this
[my_sphinx_extension.py] extension.

Putting all of that together, here is a simple [example Sphinx project]
using an extension with this nested parsing.

## Final Words

I mentioned earlier that I am by no means an expert in Sphinx extensions. I
have been using Sphinx for less than a year at this time and have only made
a few custom commands.

That said, I feel that nested parsing is a glaring feature gap in the
Sphinx extension API. I have no idea if this is the best way to implement
it, and I don't know how robust this implementation will be to future
changes to the Sphinx API.

Hopefully, these changes will continue to work or will be replaced with
something better. If you know of a better method, please let me know.

[LaTeX]: https://www.latex-project.org/
[Sphinx]: https://www.sphinx-doc.org/en/master/
[Breathe]: https://breathe.readthedocs.io/en/latest/
[reStructuredText]: https://www.sphinx-doc.org/en/master/usage/restructuredtext/index.html
[documentation from the Sphinx]: https://www.sphinx-doc.org/en/master/development/index.html
[tree structure defining the elements of the document]: https://docutils.sourceforge.io/docs/ref/doctree.html
[hello world tutorial]: https://www.sphinx-doc.org/en/master/development/tutorials/helloworld.html
[docutils directive reference]: https://docutils.sourceforge.io/docs/howto/rst-directives.html
[docutils document tree]: https://docutils.sourceforge.io/docs/ref/doctree.html
[a stackoverflow post]: https://stackoverflow.com/questions/34350844/how-to-add-rst-format-in-nodes-for-directive
[a blog by Doug Hellmann]: https://doughellmann.com/posts/defining-custom-roles-in-sphinx/
[DOI Foundation's name resolver]: https://dx.doi.org/
[sphinx_nested_parse.py]: https://github.com/kmorel/sphinx-nested-parse/blob/main/_ext/sphinx_nested_parse.py
[my_sphinx_extension.py]: https://github.com/kmorel/sphinx-nested-parse/blob/main/_ext/my_sphinx_extension.py
[example Sphinx project]: https://github.com/kmorel/sphinx-nested-parse/tree/main
