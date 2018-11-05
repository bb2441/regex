<script type="text/javascript"
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
# Regular expressions

## BB2241


---

layout: false


## Regular expressions

* A pattern matching language for searching replacing
* A mini programming language of its own
* Use by many other "host" languages (e.g. Python Perl, PHP)


---

## In Python

* the `re` module 

Official documentation: https://docs.python.org/3/library/re.html

Other resources: https://automatetheboringstuff.com/chapter7/

Here: three functions

* the `re.search` funtion
* the `re.findall` funtion
* the `re.sub` funtion


---

## `re.findall`

```
$ pydoc re.findall
Help on function findall in re:

re.findall = findall(pattern, string, flags=0)
    Return a list of all non-overlapping matches in the string.
    
    If one or more groups are present in the pattern, return a
    list of groups; this will be a list of tuples if the pattern
    has more than one group.
    
    Empty matches are included in the result.

```

* `re.findall` returns a list of matching substrings

```
>>> import re
>>> re.findall('tic', 'tac toc')
[]
>>> re.findall('tac', 'tac toc')
['tac']
>>> re.findall('t[ao]c', 'tac toc')
['tac', 'toc']

```

Try this out in https://regex101.com

---
Here `[]` defines a character class

Examples:

*  `[a]` matches the single character `a`
*  `[abc]` matches any one of the characters `a`, `b`, or `c`
*  `[a-z]` matches any one character in the range `a-z`
*  `[^a-z]` matches any character **not** in the range `a-z`

There are some macros for common classes

* `\d` single digit, equivalent to `[0-9]`
* `\w` single word character, equivalent to `[_0-9a-zA_Z]`
* `\s` single space or tab

```
>>> re.findall('\d', 'Agent 007')
['0', '0', '7']

```
```
>>> re.findall('[^\d]', 'Agent 007')
['A', 'g', 'e', 'n', 't', ' ']

```
```
>>> re.findall('\w', 'Agent 007')
['A', 'g', 'e', 'n', 't', '0', '0', '7']

```
```
>>> re.findall('[^\w]', 'Agent 007')
[' ']

```

---

Specifying a count: `{}`

* `\d{2}` two digits, equivalent to `\d\d`
* `\d{1,3}`  one to three digits
* `*`: any number
* `+`: any number greater than zero

```
>>> re.findall('\d{2}', 'Agent 007')
['00']

```
```
>>> re.findall('\d{1,3}', 'Agent 007')
['007']

```
```
>>> re.findall('\d+', 'Agent 007')
['007']

```

???

What happened to `'07'`? The search consumes the first two zeros in 
the string and then continues with the `'7'`

`\w` does not include blanks


---

Specifying an optional character

* `?`: previous charactar is optional

Example: find singular and plural occurances of a word

```python
>>> re.findall('eggs?', "Don't put all eggs in one basket egghead")
['eggs', 'egg']

```

---


### The `re.search` function

```
$ pydoc re.search
Help on function search in re:

re.search = search(pattern, string, flags=0)
    Scan through string looking for a match to the pattern, returning
    a match object, or None if no match was found.

```

* returns a Match object or the `None` object (no match)
* the Match object has methods
* `'()'` in `pattern` saves matched parts that we can back-reference later


Example

```
>>> re.search('([.\d]+) kcal', 'Energy: 1.23 kcal').group()
'1.23 kcal'

```
```
>>> re.search('([.\d]+) kcal', 'Energy: 1.23 kcal').group(1)
'1.23'

```
```
>>> re.search('([.\d]+) kcal', 'Energy: 1.23 kcal').groups()
('1.23',)

```

Here

* `+`: one or more of the previous character (digit or dot)


---

Optional matches

* `|` has the meaning of logical OR in matches

```
>>> re.findall('cat|dog', 'raining cats and dogs')
['cat', 'dog']

```

---

### The `re.sub` function

* Make substitutions based on the result of pattern matches

```
$ pydoc re.sub
Help on function sub in re:

re.sub = sub(pattern, repl, string, count=0, flags=0)
    Return the string obtained by replacing the leftmost
    non-overlapping occurrences of the pattern in string by the
    replacement repl.  repl can be either a string or a callable;
    if a string, backslash escapes in it are processed.  If it is
    a callable, it's passed the match object and must return
    a replacement string to be used.

```

Examples:

* Replace all Donald with a HTML link

```python
>>> re.sub(r'(Donald)', r'<a href="#">\1</a>', "Walt Disney's Donald Duck")
'Walt Disney\'s <a href="#">Donald</a> Duck'

```

---

### String formatting

* For the final task
* Converting objects of any type to string output

C-style

```
>>> x = 2
>>> y = 4
>>> "Two numbers: %d and %d" % (x, y)
'Two numbers: 2 and 4'

```
Using the `format` method defined for strings
```
>>> "Two numbers: {} and {}".format(x, y)
'Two numbers: 2 and 4'

```
Using f-strings (Python >= 3.6)
```
>>> f"Two numbers: {x} and {y}" 
'Two numbers: 2 and 4'

```

For more information on string formatting http://pyformat.info is a good
resource


---

## Summary

* `[]` character class
* Predifined classes `\s` (white space), `\w` (alphanumemric), `\d` (digit)
* Modifiers `^` (not), `+` (one or more), `*` (zero or more), `?` (zero or
* one), `|` (or)
* Count: `{n}` (repeated `n` times)
* `()` grouping matches for back referencing, grouped matches are referenced with `\1\, \2...`

`re` module functions

* `re.findall(pattern, string)`: returns all matches of `pattern` in `string` as a list of strings.
* `re.search(pattern, string)`:  returns the first match of `pattern` in `string` as a Match object
* `re.sub(pattern, repl, string)`:  substitutes matches of `pattern` in `string` with replacement string `repl`


Cheet sheet: https://www.debuggex.com/cheatsheet/regex/python

Web-tool for testing regex's: https://regex101.com/
