# tokenyze
tokenyze is a Python tokenizer

## Overview
It uses generators to do a look-ahead tokenizing of an input string.

Tokens are defined as names or strings, and can be nested using brackets.
Names are made up of sequential non-whitespace characters.
Brackets are special single letter tokens.
Strings are delimited by either single or double quotes.

Backslashes can escape these characters.

## Example:

The text

```python
    "fr33(the p1zza c@t)n0w_",
```

will result in the following (generated) token list:

```python
    ['fr33', '(', 'the', 'p1zza', 'c@t', ')', 'n0w_']
```

## Implementation:

The code uses a generator `getchars` to deliver character from the text
to the `gettokens` consumer. The consumer will pass on responsibility
for parsing the text to either a whitespace consumer `eatwhitespace`
or a token consumer, which will in turn defer to a name consumner
`eatname` or string consumner `eatstring`.

The `gettokens` consumer itself is a generator, which will yield each 
found token in turn until there are no more tokens left.

## Usage:

```python
$ python
>>> import tokenyze
>>> for token in tokenyze.gettokens("fr33(the p1zza c@t)n0w_"):
...     print token
... 
fr33
(
the
p1zza
c@t
)
n0w_
>>> 
```

## Why?

I have been using Python's `shlex` for a bit, but while it is fine when parsing a text
into names and strings, it is lacking once brackets are added to the mix.

I needed something with a bit more lookahead, and writing generators in python
is always fun.
