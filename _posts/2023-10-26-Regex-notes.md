---
title: Regex Notes
date: 2023-10-26 12:13:00 +0530
categories: [tech_blog, Regex]
tags: [firstblog, RegEx]     # TAG names should always be lowercase
---

A regular expression (or RE) specifies a set of strings that matches it; the functions in this module let you check if a particular string matches a given regular expression (or if a given regular expression matches a particular string, which comes down to the same thing). And the best part is that re is universal to all languages, so learning it can help with string manipulation in all languages
Here’s a complete list of the ***metacharacters*** used in RE:
`. ^ $ * + ? { } [ ] \ | ( )`

## `[]` class:
>used to specify a list or range of characters to match in the string. All the characters mentioned in the `[]` class are searched individually and values containing any one or more of the mentioned characters will be given out.
**Syntax**:

|`[a-z]`| it will search the string and see if it contains any letters from `a` to `z`.|
|----|-------|
|`[abc]`| it will search the string for a, b or c character individually and return true if it find any|
|`[^abc]`| it means to find a match containing neither a, b nor c.[^1]|
|`[a-zA-Z]`|we can also combine two or more ranges. here we are searching if the string contains ***any*** alphabet, either lower or upper case.|


the metacharacters we discussed above are not applicable inside the class except `\`


## () Group:
>we can define several sub-patterns within our pattern with the help of `()` brackets. the groups can be capturing or non capturing(i.e. we can choose if we want certain sub-pattern to be included in our resultant output. We can also choose to name these groups. We can also look forward and look behind.
### group ? usage:

|symbol|usage|
|---|---|
|`?<name>`(named capturing group)|use in a group to name that group e.g. `(?<digits>\d+)` |
|`?=`(positive look forward)|it is used to look forward i.e. to see if this group exists after the capturing group but do not include it in the resulting groups. e.g. if we want to search a word followed by space but not capture the space we would use: `(?<word>\w)(?= )`|
|`?<=`(positive look behind)|it is used to look behind i.e. to see if this group exists before the capturing group but do not include in the resulting group e.g.e.g. if we want to search a word following a space but not capture the space we would use: `(?<= )(?<word>\w)`|
|`?:`(non-capturing group)|it is used to find but not capture the group|
|`?!`(negative look forward)|it is used to look forward i.e. to see that this does not exists after the capturing group but do not include it|
|`?<!`(negative look behind)|it is used to look forward i.e. to see that this does not exists before the capturing group but do not include it|
|`\num`(back reference)|capturing groups in regex are numbered from 1, we can use `\1` to reference the first group and it will try match exact same characters again. e.g. `(\d)\1` will look for numbers such as 00, 11,....99 [[Pasted image 20231012001712.png]]|

### Accessing the group elements:
we can then access these groups using the `group()` method of match object see:[[RE-Regular Expressions#match object methods]] . 
We can access using numbers: `matchobj.group(1)` 
or we can also use the group name to acess the group: `matchobj.group("groupname")`. 
note: `matchobj.group(0)` or `matchobj.group()` will fetch all groups.

## `\` backslash:
> used to change meaning of various characters by using it before them and sometimes to escape all the metacharacters so that we can match them in patterns. e.g. if we want to search `[` in our string then we will search for `\[` not `[` as the re engine will read it as a metacharacter with the backslash.

Syntax:

| Regular Expression | Description |
| ----------------- | ----------- |
| `\d`                | Matches any decimal digit; this is equivalent to the class [0-9]. |
| `\D`                | Matches any non-digit character; this is equivalent to the class [\^0-9]. |
| `\s`                | Matches any whitespace character; this is equivalent to the class [ \t\n\r\f\v]. |
| `\S`                | Matches any non-whitespace character; this is equivalent to the class [^ \t\n\r\f\v]. |
| `\w`                | Matches any alphanumeric character; this is equivalent to the class [a-zA-Z0-9_]. |
| `\W`                | Matches any non-alphanumeric character; this is equivalent to the class [^a-zA-Z0-9_]. |
|`\$`| backslash followed by any metacharacter simply means the metacharacter literally.|
|`\b`| word boundary is used to match a word completely not just to substring of a complete word but only to that complete word. e.g. if we want to match "python"  but not "Cpython" then we use `\bpython\b` instead of `python`|
|`\B`||

[^2] 

## `?` in regex

1. **Quantifier**: In regular expressions, `?` makes the preceding element optional.

   ```python
   re.search(r'colou?r', 'color')
   ```

2. **Lazy Quantifier**: `?` after quantifiers makes them lazy.

   ```python
   re.search(r'a*?b', 'aaaaab')
   ```

3. **Non-Capturing Group**: `(?:...)` creates a non-capturing group.
![[RE-Regular Expressions#group ? usage]]
## repeating characters:
### `*`(zero or more):
> it tells re engine to check if we have repetition of the previous character ***zero or more times***. 
> e.g. `ca*t` will match `'ct'` (0 `'a'` characters), `'cat'` (1 `'a'`), `'caaat'` (3 `'a'` characters), and so forth.

### `+`(one or more):
> it checks if the previous character matches ***one or more times***.
> e.g. `ca+t` will match `'cat'` (1 `'a'`), `'caaat'` (3 `'a'`s), but won’t match `'ct'`

### `?`(zero or one):
> it kind of tells the engine that the previous character is optional, we can and cannot have it our result.
> e.g. `home-?brew` matches either `'homebrew'` or `'home-brew'`.

### `{m,n}`:
> this is the most complicated quantifier, it tells the engine to search the string for repetition of the previous character a minimum of m times and maximum of n times.
> e.g. `a/{1,3}b` will match `'a/b'`, `'a//b'`, and `'a///b'`. It won’t match `'ab'`, which has no slashes, or `'a////b'`, which has four. [^3]

## the python problem:
> python does not have specific syntax in place for re, it instead uses string for that purpose but that creates a problem as python also uses backslash as an escape sequence to change the predefined meaning of certain tasks.
> e.g.  suppose we want to search for a word : `\section`

|Characters|Stage|
|---|---|
|`\section`|Text string to be matched|
|`\\section`|Escaped backslash for [`re.compile()`](https://docs.python.org/3/library/re.html#re.compile "re.compile")|
|`"\\\\section"`|Escaped backslashes for a string literal|
so we need to use \ four times in order for the re engine to read it, two for python and two for re engine, this problem can be avoided with the help of raw strings, just like format strings we can use raw strings which ignore the predefined meaning of the characters.
e.g.

|Regular String|Raw string|
|---|---|
|`"ab*"`|`r"ab*"`|
|`"\\\\section"`|`r"\\section"`|
|`"\\w+\\s+\\1"`|`r"\w+\s+\1"`|

## RE methods:

|Method/Attribute|Purpose|
|---|---|
|`match(_pattern_, _string_, _flags=0_)`|Determine if the RE matches at the beginning of the string. returns `None` or `match` object|
|`search(_pattern_, _string_, _flags=0_)`|Scan through a string, looking for any location where this RE matches. returns `None` or `match` object|
|`findall(_pattern_, _string_, _flags=0_)`|Find all substrings where the RE matches, and returns them as a list. returns an iterable list|
|`finditer(_pattern_, _string_, _flags=0_)`|Find all substrings where the RE matches, and returns them as an [iterator](https://docs.python.org/3/glossary.html#term-iterator).|

## RE Objects:
### `re.pattern`:
>returned by re.compile() method which takes in a pattern as input. this object suuports all the methods mentioned in [[RE-Regular Expressions#RE methods]]. We can create this object and just it instead of re to find the pattern in various strings.|

### `re.Match`:
>returned by successful matches and searches.
> has boolean value of `True` i.e. match() or search() returns true if found a match else returns false

#### match object methods:

|Method/Attribute|Purpose|
|---|---|
|`group()`|Return the string matched by the RE [^4]|
|`start()`|Return the starting position of the match|
|`end()`|Return the ending position of the match|
|`span()`|Return a tuple containing the (start, end) positions of the match|


## Flags:

|Flag|Meaning|
|---|---|
|[`ASCII`](https://docs.python.org/3/library/re.html#re.ASCII "re.ASCII"), [`A`](https://docs.python.org/3/library/re.html#re.A "re.A")|Makes several escapes like `\w`, `\b`, `\s` and `\d` match only on ASCII characters with the respective property.|
|[`DOTALL`](https://docs.python.org/3/library/re.html#re.DOTALL "re.DOTALL"), [`S`](https://docs.python.org/3/library/re.html#re.S "re.S")|Make `.` match any character, including newlines.|
|[`IGNORECASE`](https://docs.python.org/3/library/re.html#re.IGNORECASE "re.IGNORECASE"), [`I`](https://docs.python.org/3/library/re.html#re.I "re.I")|Do case-insensitive matches.|
|[`LOCALE`](https://docs.python.org/3/library/re.html#re.LOCALE "re.LOCALE"), [`L`](https://docs.python.org/3/library/re.html#re.L "re.L")|Do a locale-aware match.|
|[`MULTILINE`](https://docs.python.org/3/library/re.html#re.MULTILINE "re.MULTILINE"), [`M`](https://docs.python.org/3/library/re.html#re.M "re.M")|Multi-line matching, affecting `^` and `$`.|
|[`VERBOSE`](https://docs.python.org/3/library/re.html#re.VERBOSE "re.VERBOSE"), [`X`](https://docs.python.org/3/library/re.html#re.X "re.X") (for ‘extended’)|Enable verbose REs, which can be organized more cleanly and understandably.|




## Footnotes:
[^1]:  the `^` symbol has special meaning inside the `[]` class outside it has some different meaning and also it has to be used in the beginning inside the `[]` otherwise it does not have any meaning and will be treated as any other character or alphabet and will be searched inside the string for a match.
[^2]: we can use these predefined sets inside the class `[]`. e.g. [\\s, .] means that  we are searching for any whitespace character, `,`  or `.` in the string.
[^3]: `{0,}` is the same as `*`, `{1,}` is equivalent to `+`, and `{0,1}` is the same as `?`. It’s better to use `*`, `+`, or `?` when you can, simply because they’re shorter and easier to read.
[^4]: Match object also supports `[]` indexing, so we can fetch the same results using [] as with group function.
