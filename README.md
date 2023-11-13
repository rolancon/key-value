# Key-Value
Key-Value, or K-V for short, is a small data description language that standardizes the notation of key-value pairs. K-V is extended by the [Confetti](https://github.com/rolancon/confetti) data format. The file extension for files that contain K-V data is _kv_.

The data in a K-V file consists of one or more key-value pairs, simply called pairs from now on, where the key gives context for (describes) the value.
The syntax of K-V for pairs is limited to the character set and [whitespace rules](https://github.com/rolancon/lazycode-minicode/blob/main/README.md#whitespace) of [Lazycode](https://github.com/rolancon/lazycode-minicode/blob/main/README.md#lazycode), the syntax of values is extended to the character set of [Minicode](https://github.com/rolancon/lazycode-minicode/blob/main/README.md#minicode).

K-V can be processed and converted to JSON by [Confetti](https://github.com/rolancon/confetti). 
 
One of the syntactic principles of K-V, aligned with the 'lazyness' of Lazycode, is that operators and terms can be preceded or followed by none, one or multiple spaces, and that lines can be preceded or followed by none, one or multiple newlines, without affecting the interpretation. The superfluous spaces and newlines will be normalized away during parsing: see the [whitespace](https://github.com/rolancon/lazycode-minicode/blob/main/README.md#whitespace) section of Lazycode.

## Terms

A single _term_ is called an _atom_. 

    term

The naming convention for a term is as follows: a _base term_ should start with a lower-case letter [a-z], potentially followed by one ore more lower case letters or digits [0-9]. More than one base term can be combined into a _compound term_ with a hyphen **-**. Examples of terms:

    t
    
    term1
    
    term-a
    
    term-a1

A special convention is that a single hyphen _-_ can be used for an anonymous (unnamed) term:

    -

A pair contains of left-hand side term and a right-hand side term, connected with an equals sign **=**. E.g.:

    left-term = right-term

As explained in the intro, any number of spaces might appear around terms and operators like _=_, so the following notation has identical meaning:

        left-term=right-term

Since a compound term still counts as one term, the following is not valid:

    incorrect - term

The backslash **\\** newline escape can be used to break up a pair over two lines, with the value and equals sign on one line, and the value on the other:

    term-key = \
     term-value

The indendation of the second line (with one space) is optional, but considered good practice becase it gives another visual clue that the second line is subordinate to the first line.

A set consists of two or more pairs, separated with newlines:

    term-a = value1
    term-b = value2

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
     This is a set containing two pairs:
     term-a = value1
     term-b = value2
    ;;
    term-a = value1
    term-b = value2
    
The indentation inside the multi-line comment (with one space) is optional but considered good practice, because it visually emphasizes that this is not data. Multi-line comments can also serve as documentation for what is below it.

## Key-value stores

Key-value stores are directly supported with a pair as follows, which models a key-value _pair_:

    key = value

Multiple pairs are then simply added one below the other:

    key1 = value-a
    key2 = value-b

Together these pairs form a set (a set containing pairs is called a _tuple_).

## Types

So far we've only seen examples of the term type for terms:

    some-term

which is a special case of the string type (see below).

### Minicode character

The Minicode character type consists of a single Minicode character (including whitespace characters space and newline), like:

    char = 1
    char = A
    char = a
    char = _
    char = +

### String

The string datatype consists of an ordered collection of one or more Lazycode or Minicode characters, and can be denoted in several different ways. In all cases whitespace before and after the actual string is not included. 

It can be encoded as literal printable ASCII characters on a single line (this only uses characters from the character set of Lazycode):
  
    str = this is 1 line with numbers, letters and 2 symbols.

The syntax of the string datatype is extended to support the character set of Minicode, which means uppercase letters and shifted symbols from the keyboard are also allowed in a value:

    str = These are just some lowercase and uppercase ASCII characters including number 1.1 and shifted symbol #. 

It can be split up over two different lines using the backslash **\\** newline separator:

    str = This line is split between this line \
     and the next line.

unless the backslash is doubled, which counts as an escape:

    str = This line is not split and the backslash is part of the string: \\

or as a single character

    char = \\

It can be surrounded by single quotes **'**.

One single quote as value is interpreted as the single quote character itself:

    single-quote-char = '

The quotes must always be used if the value contains only one or two of the other operators from K-V:

    comment = ;
    character = ';'

    boolean-false = -
    character = '-'
    
    boolean-true = --
    string = '--'
    
    null-operator = []
    string = '[]'

otherwise they will not be interpreted as strings but processed in other ways as seen above.

A single equals sign has no special meaning, and is therefore interpreted as a character:
    
    character = =
    character = '='

In the case that quoted string is not empty several other rules apply -->

The whitespace at the start and end of the string that is within the quotes is included.

    str = ' two extra spaces '

Several characters escaped with the backslash character have a special interpretation:

    single-quote = '\''
    backslash = '\\'
    newline = '\n'
    tab = '\t'
    carriage-return = '\r'
    vertical-tab = '\v'
    formfeed = '\f'
    ascii-control-character = '\x7f'
    non-ascii-unicode-character = '\uffff'
    emoji-character = '\j10ffff'

These escapes only exist in quoted strings. Example:

    quoted-escaped-string = 'This is a quoted string and contains two escaped characters: single quote (\') and backslash (\\).'

A single newline escape cannot appear in a quoted string, unless it is positioned immediately after the opening quote and followed by a newline, in that case the first newline is removed from the string:

    str = '\
     the backslash and newline from the previous line are not part of this string'

and character escapes are interpreted literally in such strings (raw strings). To be able to include single quotes in the text, use at least three consecutive single quotes _'''_; the number of surrounding single quotes should be at least one higher than the number of enclosed consecutive single quotes:

    str = '''\
     This single quote ' and other characters like this backslash \ need no escape.'''

### Blobs

Bytes can be encoded textually using a special string notation, consisting of two single quotes _''_ enclosing pairs of whitespaces-separated hexidecimally-encoded bytes. Hexidecimal is a numerical system for base-16 (_16_ different numeric values: _0_ thru _15_), where the numbers 10 thru 15 are encoded with letters _a_ thru _f_:

    a = 10
    b = 11
    c = 12
    d = 13
    e = 14
    f = 15
4.294.967.295
Each pair encodes one byte, which represents a value between _0_ and _256_ (16 x 16). The lowest byte-string value is _00_ (0), the highest _ff_ (255). Pairs are separated with whitespace. A 32-bit value would be encoded as:

    blob = ''0a 12 bc d3''

The previous example consists of four bytes. Larger blobs can be separated per line using the backslash notation. An example of a 64-bit value:

    blob = ''\
     0a 12 bc d3
     4e 56 f7 89''
     
This contains eight bytes.

### Character range

A special string operation is the character range. For the Minicode characters which can be both grouped and ordered, namely digits, uppercase letters and lowercase letters, it is possible to define a value as an ordered subset of those characters by just listing the start and end character (in numerical or alphabetical ordering), without having to list all the characters separately. This is denoted with a square brackets operator block _[]_, where the character ranges are specified as start-char..end-char (two characters joined with double dots _.._), and multiple ranges can be combined (without spaces).

A character range that includes all digits 0 thru 9 is:

    uppercase = [0..9]

A range that includes all digits and lowercase letters (the range of Lazycode) is:

    uppercase = [0..9a..z]

The full range of all these characters (including uppercase letters in Minicode) is:

    character-range = [0..9A..Za..z]

## JSON types

Values of key in K-V are all string by default (including the character, blob and empty character/string/blob type). But when exported to JSON, they can be interpreted as several different types that are quite similar to JSON types.

For atomic values the supported types are:
- the **boolean** type
- the **number** type
- the **Minicode character** type.

and for each atomic type also 
- its **empty** type

Furthermore, K-V supports one compound type, consisting of one or more Minicode characters:
- the **string** type.

### Boolean

The boolean type contains two values:

    bool = -

for **false** values and

    bool = --

for **true** values.
The single hyphen **-**, which indicates false values, recalls the minus sign in front of numbers, which negates the value. The double hyphens **--**, which indicates true values, recalls the double minus sign in math, whcih equals the confirming plus sign _+_ in front of a value.

### Number

The number is very similar to the JSON number type. It can be any number from zero (_0_) upwards. A number can optionally have a negative sign **-**, a decimal separator **.** and an exponent (lowercase **e**) with an optional negative sign **-**:

    num = 1
    num = 19
    num = -2
    num = 2.7
    num = 3.77
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

The number type can be further subdivided as follows according the principles of a [Numerical tower](https://en.wikipedia.org/wiki/Numerical_tower):
- the **integer** type, for types which contain only integer numbers (whole numbers optionally preceded by a minus sign) - as in the first three examples above
- the **real** type, for types which can also contain decimal numbers (as indicated by an exponent and/or a decimal point) - as in the remaining examples above

A further special number type is the **fraction** type:

    num = 3//4
    num = -3//4

which consists of two integers joined together with two slashes, optionally preceded by the minus sign. A fraction can be converted to either an integer or a real.

### Empty

A term type names a certain value, and therefore whether it's an empty type depends on the type of its value. The special case of the anonymous term (_-_) qualifies as the empty metatype of the term type, since its name is empty.

The empty boolean type is the same as its **false** value:

    -

The empty number type for integers is the same as the digit zero:

    0

but can also be denoted with a minus sign:

    -0

For reals the empty number type can also be denoted as follows:

    0e0
    -0e0
    0e-0
    -0e-0
    0.0
    -0.0
    0.0e0
    -0.0e0
    0.0e-0
    -0.0e-0

The empty type for a Minicode character has no syntactic encoding, but is denoted by an empty value

    empty = 

The empty string value is denoted with two adjacent single quotes (no characters inside the quotes):

    ''

Since as explained a string is an ordered collection of characters, an empty string (no characters) is therefore the same as the empty character.

### Null

Unlike in JSON, there is no **null type** in K-V. However, it is still possible to emulate a **null value** with the **null operator**, a pair of square brackets: **[]**.
In K-V this indicates the pair will be skipped; if the K-V is converted to JSON however, the null operator might be reinterpreted as the _null_ value. I.e.:

    skipped-or-null = []
