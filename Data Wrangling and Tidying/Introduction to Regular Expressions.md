# Introduction to Regular Expressions
Regular Expressions is a sequence of characters that define a search pattern. Usually this pattern is used by string searching algorithms for "find" or "find and replace" operations on strings, or for input validation.

## Literals
Literals are the simplest form of a regular expression. They will simply match the exact text. For example, the regular expression `python` will match the string `python` and not `pythons` or `Python`.

## Alternation `|`
The `|` character is used to define alternatives. For example, the regular expression `python|java` will match the string `python` or `java`.

## Character Sets `[]`
Character sets are used to match only one out of several characters. For example, the regular expression `[ae]` will match `a` or `e`.

## Wildcard `.`
The `.` character is used to match any character except a newline. For example, the regular expression `py.` will match `pyt`, `pyc`, `pyo`, `py!`, etc.

Let's saw we want to match any 9-letter word. We can use the regular expression `.........`.

### escape character `\`
The `\` character is used to escape special characters. For example, the regular expression `.` will match any character except a newline, but the regular expression `\.` will match a period.

## Ranges `[-]`
Ranges are used to match a single character out of a range of characters. For example, the regular expression `[a-z]` will match any lowercase letter.

**Note:** Just like character sets, within any characer set `[]` we only match one character.

## Shorthand Character Classes
Shorthand character classes are used to match common sets of characters. 
- `\w` matches any word character (equivalent to `[a-zA-Z0-9_]`)
- `\d` matches any digit (equivalent to `[0-9]`)
- `\s` matches any whitespace character (equivalent to `[\t\n\r\f\v]`)

For example, the regular expression `\d\s\w\w\w\w\w\w\w` will match a digit, followed by a whitespace character, followed by 7 word characters. Thus, the regular expression will match `3 monkeys`.

**Note:** The uppercase versions of these shorthand character classes are used to match the opposite of the lowercase versions.
- `\W` matches any non-word character (equivalent to `[^a-zA-Z0-9_]`)
- `\D` matches any non-digit (equivalent to `[^0-9]`)
- `\S` matches any non-whitespace character (equivalent to `[^\t\n\r\f\v]`)

## Grouping `()`
Grouping is used to define subexpressions within a regular expression.

For example, when using alternation, the regular expression `I love (cats|dogs)` will match the strings `I love cats` and `I love dogs`.

## Quantifiers `{}`
Quantifiers are used to define the quantity of the character or expression to match.

- `\w{3}` matches any three word characters
- `\w{1,3}` matches between one and three word characters

The regex `roa{3}r` will match the characters ro followed by 3 as, and then the character r, such as in the string roaaar.

## Quantifiers - Optional `?`
The `?` character is used to match the preceding character 0 or 1 times. For example, the regular expression `colou?r` will match `color` and `colour`.

The regex `The monkey ate a (rotten )?banana` will match both `The monkey ate a rotten banana` and `The monkey ate a banana`.

## Quantifiers - Zero or More `*`
The `*` character is used to match the preceding character 0 or more times. For example, the regular expression `go*gle` will match `ggle`, `gogle`, `google`, `gooogle`, etc.

## Quantifiers - One or More `+`
The `+` character is used to match the preceding character 1 or more times. For example, the regular expression `go+gle` will match `gogle`, `google`, `gooogle`, etc.

## Anchors `^` and `$`
The `^` character is used to match the start and the `$` character is used to match the end of a string.

For example, the regular expression `^Monkeys: my mortal enemy$` will match the string `Monkeys: my mortal enemy` and not `I love Monkeys: my mortal enemy`.

The `^` ensures that the matched string starts with `Monkeys` and the `$` ensures that the matched string ends with `enemy`.

## Resources
### RegExr Regular Expression Builder
[RegExr](https://regexr.com/) is an online tool to learn, build, and test regular expressions.