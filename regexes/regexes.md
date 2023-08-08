[Home](../README.md) > [Regexes](regexes.md)

## Regexes
List of examples about regular expressions.

Another example involving regexes, but not listed here, is [this one](../type_2_grammars/grammars.md###Simple-Grammar-for-Regex-Parsing).

> Note: all the regexes presented are valid ECMAScript regexes, and can be used with JavaScript. Other regex engines may have different syntaxes.

### The Backreference Trick
A textbook example of a Type-2 grammar is this one: `a^n b^m a^n`, with `n` and `m` greater than or equal to 1.
This grammar is not regular, thus it should not be possible to recognize it by using "strict" regular expressions. However, most regex engines allow the use of backreferences, and we can exploit this feature they offer to recognize this grammar, and generally all the grammars in which the self-embedding is "symmetric" (meaning that the same substring that is embedded at the beginning of the string is embedded at the end of the string, and are not interleaved with other substrings).

In our example, we can use the regex `^(a+)(b+)(\1)$` to recognize the grammar. The regex is composed of three groups:
 - `(a+)` matches one or more `a` at the beginning of the string, and stores them in the first group
 - `(b+)` matches one or more `b`, and stores them in the second group
 - `(\1)` matches the same substring as the one matched by the first group, and checks that it is at the end of the string (thus doing the "symmetry" check trick).
`^` and `$` are used to match the beginning and the end of the string, respectively, completing the trick.

In practice, a regular expression engine that supports backreferences will "remember" the first group, and will check that the same substring is at the end of the string. As an example, with the following test strings we would get the outcome shown below:
 - `aba`: match
 - `aaabbbaaa`: no match
 - `aaabbaaaa`: no match (the symmetry is broken)
 - `aaabbaa`: no match (the symmetry is broken)
 - `abab`: no match (the last `b` should not be there).

### Use of Regexes to Strip Code Comments
Idea: demonstrate how it is possible to use regexes to strip all comments from a piece of code reliably, without the need of a parser. (Demonstrate it for C-style comments, but it can be extended to other languages as well).

---
## Useful Links
- [Regex101](https://regex101.com/): a website that allows you to test and debug your regexes, and provides a lot of useful information about them
- [Regular-Expressions.info](https://www.regular-expressions.info/): a website that contains a lot of information about regexes, their syntax, a thorough explanation of how each of their features work, and much more.
