# Key-Value
Key-Value, or K-V for short, is a small data description language that standardizes the notation of key-value pairs. K-V is extended by the [Confetti](https://github.com/rolancon/confetti) data format. The file extension for files that contain K-V data is _kv_.

The data in a K-V file consists of one or more key-value pairs, simply called pairs from now on, where the key gives context for (describes) the value.
The syntax of K-V for pairs is limited to the character set and [whitespace rules](https://github.com/rolancon/lazycode-minicode/blob/main/README.md#whitespace) of [Lazycode](https://github.com/rolancon/lazycode-minicode/blob/main/README.md#lazycode), the syntax of values is extended to the character set of [Minicode](https://github.com/rolancon/lazycode-minicode/blob/main/README.md#minicode).

## Terms

A single _term_ is called an _atom_. 

    term

The naming convention for a term is as follows: a _base term_ should start with a lower-case letter [a-z], potentially followed by one ore more lower case letters or digits [0-9]. More than one base term can be combined into a _compound term_ with a hyphen **-**. Examples of terms:

    t
    
    term1
    
    term-a
    
    term-a1

A pair contains of left-hand side term and a right-hand side term, connected with an equals sign **=**. E.g.:

    left-term = right-term

A set consists of two or more terms, separated with a comma **,**:

    term-a, term-b

The backslash **\\** newline escape can be used to break up a pair over two lines, with the value and equals sign on one line, and the value on the other:

    term-key = \
     term-value

The indendation of the second line (with one space) is optional, but considered good practice becase it gives another visual clue that the second line is subordinate to the first line.

## Comments

Single-line comments start with one semicolon **;** on a separate line, followed by the comment and a newline:

    ; This is an atom
    term

In case one would like to comment out the key part (and thereby the pair), just comment out the entire line:

    ;key = value

The value part of a pair can also be commented out separately by fronting it with a semicolon:

    key = ;value

which is identical to writing:

    key =

Multi-line comments start end with two consecutive semicolons **;;** on separate lines, with the comments and newlines nested in between the two pairs of semicolons:

    ;;
      This is a set containing two atoms:
      term-a
      term-b
    ;;
    term-a, term-b

## Key-value stores

Key-value stores are directly supported with a pair as follows, which models a key-value _pair_:

    key = value

Multiple pairs are then simply added one below the other:

    key1 = value-a
    key2 = value-b

Together these pairs form a set (a set containing pairs is called a _tuple_).

## Types

So far we've only seen examples of the term type, which is a special case of the string type. For atomic values, however,  K-V also support types which are quite similar to JSON types. They are:
- the **empty** type
- the **boolean** type
- the **number** type
- the **string** type.

The empty type has no syntactic encoding, but is denotated by an empty value, and is basically the same as the empty string value:

    empty = 

The boolean type contains two values:

    bool = -

for **false** values and

    bool = --

for **true** values.
The single hyphen **-**, which indicates false values, recalls the minus sign in front of numbers, which negates the value. The double hyphens **--**, which indicates true values, recalls the double minus sign in math, whcih equals the confirming plus sign _+_ in front of a value.

The number is very similar to the JSON number type. A number can optionally have a negative sign **-**, a decimal separator **.** and an exponent (lowercase **e**) with an optional negative sign **-**:

    num = 1
    num = 19
    num = -2
    num = 2.7
    num = 3.70
    num = -4.0
    num = 5e6
    num = -5e6
    num = 5e-6
    num = -5e-6
    num = 5.8e6
    num = -5.8e6
    num = 5.8e-6
    num = -5.8e-6

To support interoprability with JSON, the plus sign **+** and the uppercase letter **E** are allowed as alternatives to respectively leaving out the plus sign and using the lowercase letter **e** in exponent notation, even though they are not part of the Lazycode character set; they will be normalized away during the parsing phase in favor of K-V's own preferred syntax.

The string datatype can be denotated in several different ways. In all cases whitespace before and after the actual string is not included. 

It can be encoded as literal printable ASCII characters on a single line:
  
    str = this is 1 line with: numbers, letters and 2 symbols

The syntax of the string datatype is extended to support the character set of Minicode, which means uppercase letters and shifted symbols from the keyboard are also allowed in a value:

    str = These are just some lowercase and uppercase ASCII characters including number 1.1 and shifted symbol #. 

It can be split up over two different lines using the backslash **\\** newline separator:

    str = This line is split between this line \
     and the next line.

unless the backslash is doubled, which counts as an escape:

    str = This line is not split and the backslash is part of the string: \\

It can be surrounded by single quotes **'**. Two single quotes constitute an empty string:

    empty-str = ''

In the case that quoted string is not empty several other rules apply -->

The whitespace at the start and end of the string that is within the quotes is included.

    str = ' two extra spaces '

Several characters escaped with the backslash characters have special interpretation:

    single-quote = '\''
    backslash = '\\'
    newline = '\n'
    tab = '\t'
    carriage-return = '\r'
    vertical-tab = '\v'
    formfeed = '\f'
    ascii-control-character = '\x00'
    non-ascii-unicode-character = '\uffff'
    emoji-character = '\j10ffff'

A single newline separator cannot appear in the string, unless it is positioned immediately after the opening quote and followed by a newline, in that case the first newline is removed from the string:

    str = '\
     the backslash and newline from the previous line are not part of this string'

and character escapes are interpreted literally in such strings (raw strings). To be able to include single quotes in the text, the number of surrounding single quotes should be one higher than the number of consecutive enclosed single quotes:

    str = ''\
     This single quote ' and other characters like this backslash \ need no escape.''

And for characters that could be interpreted as other datatypes, string interpretation can be enforced by doubling the equals sign **=**:

    empty-str-not-null == 
    str-not-bool == yes


