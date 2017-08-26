---
title: "Regular Expressions"
date: 2017-08-21
tags: ["regex"]
draft: false
---

*Note: If you find any issues in the post, do let me know in the comments section.*

# Regular Expressions
* It is not a Programming language
* Everything is a character

## Character classes

### dot class
* `.`
* matches every character except line breaks.

### match Any
* `[\s\S]`
* matches every character including line breaks.

### word
* `\w`
* matches low ascii characters - alphanumeric and underscore
* equivalent to `[a-zA-Z0-9_]`

### not word
* `\W`
* matches anything other than a word
* equivalent to `[^a-zA-Z0-9_]`

### digit
* `\d`
* matches any digit from 0 to 9
* equivalent to `[0-9]`

## not digit
* `\D`
* matches any character other than digit
* equivalent to `[^0-9]`

## space
* `\s`
* matches spaces, tabs, line breaks

### not space
* `\S`
* matches any other than space, tabs, line breaks

### character set
* `[]`
* matches any character in the set

#### example

> *expression:* `[aeiou]`

> *phrase:* glib jocks vex dwarves!

> *result:* gl`i`b j`o`cks v`e`x dw`a`rv`e`s!


### negated character set
* `[^]`
* does not match the characters in the set

#### example

> *expression:* `[aeiou]`

> *phrase:* glib jocks vex dwarves!

> *result:* `g``l`i`b` `j`o`c``k``s` `v`e`x` `d``w`a`r``v`e`s``!`

### range character set
* `[a-z]`
* matches any character between the token mentions inclusive

#### example

> *expression:* `[g-i]`

> *phrase:* abcdefghijklmnopqrstuvwxyz

> *result:* abcdef`g``h``i`jklmnopqrstuvwxyz

## Anchor tags
* Using both will lead to exact word match

### beginning
* `^`
* marks the beginning of the string or beginning of the line if multiline is enabled

### ending
* `$`
* marks the end of the strings or end of the line if multiline is enabled

## Groups
### capturing group
* `()`
* groups multiple tokens for extracting a substring or back referencing

### non-capturing group
* `(?:)`
* groups multiple tokens but does not form a group

## Quantifiers & Alternations
### plus
* `+`
* matches one or more preceding tokens

#### example

> *expression:* `Fo+`

> *phrase:* abcdefghijklmnopqrstuvwxyz

> *result:* abcdef`g``h``i`jklmnopqrstuvwxyz

### star
* `*`
* matches 0 or more preceding characters

#### example

> *expression:* `Fo*`

> matches
```
Fo
Foo
Fooooo
Foooooooo
```

### alternation
* `|`
* acts like Boolean OR
* matches before or after tokens

#### example

> *expression:* `b(a|e|u)d`

> *phrase:* bad bud bod bed bid

> *matches:* `bad` `bud` bod `bed` bid

### optional
* `?`
* matches 0 or 1 of the preceding tokens essentially making it optional

#### example

> *expression:* `colou?r`

> *phrase:* color colour

> *matches:* `color` `colour`

### quantifiers
* `{3}` matches preceding character to exactly 3 times
* `{1,3}` matches preceding character between 1 and 3 inclusive
* `{3,}` matches preceding character 3 or more times

#### example

> *expression:* `be{2,4}`

> *phrase:* be beeee beeeee

> *matches:* be `beeee` `beeee`e
