[Home](../README.md) > [Grammars](grammars.md)

## Grammars

### Adding Unary Operators to Math Grammar
Starting from the math grammar showed during lectures, one may wonder how to expand it to include unary plus (`+5`) and unary minus (`-5`) oeprators.
The original grammar is the following:
```plaintext
EXP ::= TERM | EXP '+' TERM | EXP '-' TERM
TERM ::= FACTOR | TERM '*' FACTOR | TERM '/' FACTOR
FACTOR ::= num | '(' EXP ')'
````
> Legend:
> - Upper case letters are non-terminals
> - Characters in single quotes are terminals
> - `num` is a shorthand for a sequence of digits.

We will use the exact same grammar as before, so there is no need to "touch" a parser for it. We will only need to modify the lexer, by making three considerations:
1. The unary operators may appear in three positions only: at the beginning of the expression, after an opening parenthesis, or after an operator
2. The unary plus may be ignored (`+5` is the same as `5`)
3. The unary minus may be replaced with `-1` multiplied by the "absolute" value of the expression (`-5` is the same as `-1 * 5`).

With these considerations in mind, we can modify the lexer to recognize the unary operators as follows:
```python
class Lexer:
    def __init__(self):
        # ...
        # we define an equivalent for the unary minus operator
        self.__unary_minus_equivalent__ = [ElementToken(value=-1), MulOp()]
        # ...

    def scan(self, exp):
        # ...

        tokens = []

        i = 0 # index of the current character
        while i < len(exp):
            if # case 1 ...
            elif "+*-/^".find(exp[i]) >= 0:
                # check if it is unary +/-
                if i == 0 or ("+*-/^".find(exp[i-1]) >= 0) or exp[i-1] == '(':
                    if exp[i] == '+':
                        i += 1 # ignore unary +
                        continue
                    elif exp[i] == '-':
                        # replace unary - with its equivalent
                        tokens.append(self.__unary_minus_equivalent__)
                        i += 1
                        continue

                append_num()
            elif # other cases

            i += 1 # move to the next character
        
        # ...

        return tokens
```

With this simple trick at lexer level, the same parser used before can now parse expressions with unary operators.


### Simple Grammar for Regex Parsing
The following grammar is a simple one for parsing regular expressions. It is not complete, but it is enough to parse simple regular expressions.
> Legend:
> - Upper case letters are non-terminals
> - Characters in single quotes are terminals
> - `char` is a shorthand for any character
> - Square brackets indicate optional elements
> - Parentheses indicate grouping
> - `*` means zero or more repetitions of the previous.
```plaintext
RE ::= GROUP | '|' RE
GROUP ::= (ELEMENT [QUANTIFIER])*
ELEMENT ::= char | '(' RE ')'
QUANTIFIER ::= '*' | '+' | '?'
```

A Top-Down Recursive Descent Parser for this grammar can be implemented needs the following functions to be implemented:
```java
parseRe() {...}
parseGroup() {...}
parseElement() {...}
parseQuantifier() {...}
```

The function `parseQuantifier` can be omitted with the grammar above (since the quantifiers are only terminals), but it is useful to have it in order to be "open" parse more complex regular expressions (curly braces quantifiers, I'm looking at you).
