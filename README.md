# Key-Value
Key-Value, or K-V for short, is a small data description language that standardizes the notation of key-value pairs. The file extension for files that contain K-V data is _kv_.


Confetti is a generic data description language with semantics based on _Data Algebra_ and a syntax derived from _configuration files_. Confetti is an extension of the [K-V](https://github.com/rolancon/key-value) data format. The file extension for Confetti files is _.cft_.

The data structures in Confetti mirror those found in [Data Algebra](https://algebraixlib.readthedocs.io/en/latest/intro.html), which is based entirely on [Zermelo-Fraenkel set theory with the axiom of choice (ZFC)](https://en.wikipedia.org/wiki/Zermelo-Fraenkel_set_theory).

The basic data structure in Data Algebra is a _Couplet_, called a _pair_ in Confetti. This is very similar to a key-value pair, where the key gives context for (describes) the value. See [K-V](https://github.com/rolancon/key-value) for more information.
Couplets can be grouped together in a _set_: these are called _Relations_ in Data Algebra, and _tuples_ in Confetti. Tuples are quite similar to records in a database. Tuples in turn are then grouped together in an enclosing set, know as a _Clan_, which mimics a table in a database schema. Finally, Clans could be grouped together in a set called a _Horde_, which is somewhat equivalent to a database.

These four data structures can generically model many different types of data stores and data formats, e.g.: key-value stores and [value types]() as in K-V, and configuration files, document stores, relational databases, RDF triple stores, graph databases, CSV files, spreadsheets, XML and JSON as in Confetti. The syntax to model these sets and pairs is derived from [configuration files](https://github.com/madmurphy/libconfini/blob/master/MANUAL.md), also known as [INI files](https://en.wikipedia.org/wiki/INI_file) on Windows.

Just as in K-V, the syntax of Confetti for data structures is limited to the character set and [whitespace rules](https://github.com/rolancon/lazycode-minicode/blob/main/README.md#whitespace) of [Lazycode](https://github.com/rolancon/lazycode-minicode/blob/main/README.md#lazycode), the syntax of values is extended to the character set of [Minicode](https://github.com/rolancon/lazycode-minicode/blob/main/README.md#minicode).

## Terms

A single _term_ is called an _atom_. 

    term

The naming convention for a term is as follows: a _base term_ should start with a lower-case letter [a-z], potentially followed by one ore more lower case letters or digits [0-9]. More than one base term can be combined into a _compound term_ with a hyphen **-**. Examples of terms:

    t
    
    term1
    
    term-a
    
    term-a1

A couplet contains of left-hand side term and a right-hand side term, connected with an equal sign **=**. E.g.:

    left-term = right-term

A set consists of two or more terms, separated with a comma **,**:

    term-a, term-b

The backslash **\\** newline escape can be used to break up one line over two lines:

    term-a, \
    term-b

## Comments

Single-line comments start with one semicolon **;** on a separate line:

    ; I am an atom
    term

Multi-line comments start end with two consecutive semicolons **;;** on separate lines:

    ;;
      This is a set containing two atoms:
      term-a
      term-b
    ;;
    term-a, term-b

## Key-value stores

Key-value stores are directly supported with a couplet as follows, which models a key-value _pair_:

    key = value

Multiple key-value pairs are then simply added one below the other:

    key1 = value-a
    key2 = value-b

Together these couplets form a set (a set containing couplets is called a relation).

