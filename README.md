# Grammar::PrettyErrors

Generate pretty errors when your parse fails.

# Synopsis

Input:

```perl6
use Grammar::PrettyErrors;

grammar G does Grammar::PrettyErrors {
  rule TOP {
    'orange'+ % ' '
  }
}

G.parse('orange orange orange banana');
```

Output:

```
--errors--
  1 │▶orange orange orange banana
                           ^

Uh oh, something went wrong around line 1.

Unable to parse TOP.
```

# Description

`Grammar::PrettyErrors` provides a role that can
be applied to a grammar to provide pretty error
messages when a parse fails.  It works by wrapping
the `<ws>` token and keeping track of a highwater
mark (the position), and the name of the most
recent rule that was encountered.

This technique is described by moritz in his
excellent book [0] (see below).

# Classes, Roles, Methods

## Grammar::PrettyErrors (Role)

### ATTRIBUTES

* `quiet` -- Bool, default false: save errors, don't throw them.

* `colors` -- Bool, default true: make output colorful.

* `error` -- a `PrettyError` object (below).

### METHODS

* `new` -- wraps the `<ws>` token as described above, it also takes
  additional named arguments (to set the ATTRIBUTEs above).

## PrettyError (class)

### METHODS

* `report` -- returns the text of a report with the context for an error.

# EXAMPLES

```
grammar G does Grammar::PrettyErrors { ... }

# Throw an exception with a message when a parse fails.
G.parse('orange orange orange banana');

# Same thing
G.new.parse('orange orange orange banana');

# Pass options to the constructor
my $g = G.new(:quiet, :!colors);
$g.parse('orange orange orange banana');

# Report a parse error
say .report with $g.error;
```

# SEE ALSO

* [0] [Parsing with Perl 6 Regexes and Grammars](https://www.apress.com/us/book/9781484232279) and the accompanying [code](https://github.com/Apress/perl-6-regexes-and-grammars/blob/master/chapter-11-error-reporting/03-high-water-mark.p6)

* Grammar::ErrorReporting

# AUTHOR

Brian Duggan