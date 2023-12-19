# Regex Tutorial - Matching a URL

Regular expressions, or regex, are series of special characters used in coding that define a search pattern. Regex can be used to verify that a user input is valid based on a specific criteria. This gist tutorial explains the functionality of a regex expression used to match a URL.

## Summary

The following code is the "Matching a URL" regex:

```
/^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/
```
This regex can be used to verify whether a set of characters (i.e. a string) matches a URL format by evaluating each character in the string one by one. It matches both http and https protocols, domain names, top-level domains, and optional paths or additional segments in the URL. This gist will break down the different characters operator of this regex and explain how they are used to create a URL matching regex.

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [Character Classes](#character-classes)
- [Bracket Expressions](#bracket-expressions)
- [Greedy and Lazy Match](#greedy-and-lazy-match)
- [Author](#author)

## Regex Components

The following section will discuss each component of the regex and explain its functionality. The most effective way to explain the regex statement is to break it down into the sections which are wrapped in parenthesis. We can use an example URL and see how they match the sections of the regex:

Example URL: https://gist.github.com/nataniel-c

Regex: ``/^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/``

 * **https://** => ``/^(https?:\/\/)`` => URL protocol scheme

* **gist.github** => ``([\da-z\.-]+)`` => domain name

* **com** => ``([a-z\.]{2,6})`` => top level domain (separated by dot from domain name)

* **/nataniel-c** => ``([\/\w \.-]*)`` => URL path

The "Matching a URL" regex, as with every other regex, must be wrapped in slash characters (``/``) because it is considered a literal:

    Note: JavaScript provides two ways to create a regex object. The first, shown in our example, uses literal notation. The second is to use a RegExp constructor. The constructor function's parameters are not enclosed within slashes; instead, they use quotation marks. To learn more, review the MDN Web Docs on the RegExp object.

After the ``/`` wrappers, the regex begins and ends with anchors, which will be discussed in the following section.


### Anchors
The characters ``^`` and ``$`` are both considered to be anchors. The ``^`` anchor signifies a string that begins with the characters that follow it. This could be in one of two formats:
- An exact string match, such as ``^The``, where the strings "The" or "The person" match, but "the" and "the person" do not. This is because a regex is case-sensitive.

- A range of possible matches, displayed using [bracket expressions](#bracket-expressions).

The ``$`` anchor signifies a string that ends with the characters that precede it. Just as with the ``^`` character, it can be preceded by an exact string or a range of possible matches.

In the “Matching a URL" regex, the ``^`` and ``$`` anchors are each used once: at the beginning and end of the expression, respectively, to define the beginning and end of the regex. Quantifiers, which will be discussed in the next section, allow the regex to limit the types of characters defined within these anchors.

### Quantifiers
Quantifiers set the limits of the string that your regex matches (or an individual section of the string). They frequently include the minimum and maximum number of characters that your regex is looking for.

Quantifiers are inherently ["greedy"](#greedy-and-lazy-match). They include the following:

    * —Matches the pattern zero or more times

    + —Matches the pattern one or more times

    ? —Matches the pattern zero or one time

    {} —Curly brackets can provide three different ways to set limits for a match:

        { n } —Matches the pattern exactly n number of times

        { n, } —Matches the pattern at least n number of times

        { n, x } —Matches the pattern from a minimum of n number of times to a maximum of x number of times

Each of these quantifiers can be made ["lazy"](#greedy-and-lazy-match) by adding the ? symbol after it.

The "Matching a URL" regex (shown again below) uses multiple quantifiers, which will be explained as they appear left to right in the regex.

    /^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/

The ``?`` quantifier is used twice in the ``(https?:\/\/)?`` component. This section matches an optional protocol scheme at the beginning of the URL string. A ``?`` quantifier is attached to the ``s`` in ``https``, which means that it will match both ``http`` and ``https`` . Furthermore, another ``?`` quantifier is attached at the end of the section, which is entirely wrapped in parenthesis. This means that whole matching criteria of ``https?:\/\/`` is optional and the regex can match urls that do not contain a protocol scheme. 

Another ``?`` quantifier is used at the end of the whole regex after two opposite facing slashes: ``\/?``. This quantifier matches an optional trailing forward slash at the end of the string such as in the URL: ``https://gist.github.com/nataniel-c/``

In the ``([\da-z\.-]+)`` component of the regex, the ``+`` quantifier is used at the end of the [bracket expression](#bracket-expressions) defining the possible characters that could make up the beginning of the URL domain. This ``+`` quantifier will give a match for every url with at least one character as its domain string, given that it matches the criteria of the bracket expression. 

The ``([a-z\.]{2,6})`` component has the quantifier ``{2,6}``. This means it will match the preceding string pattern a minimum of 2 times and a maximum of 6 times. This limits this component of the URL to be between 2 and 6 characters long.

Finally, the ``([\/\w \.-]*)*`` component uses the ``*`` quantifier to match zero or more occurences of the characters defined in the bracket expression. This section matches any path or additional segments in the URL after the domain definition.

Though this section defined the quantifiers and how they are used, the character classes and [bracket expressions](#bracket-expressions) sections will discuss in depth how specific characters are matched within the limits of the quantifiers.

### Character Classes
A character class in a regex defines a set of characters, any one of which can occur in an input string to fulfill a match.

The following are common character classes:

    . —Matches any character except the newline character (\n)

    \d —Matches any Arabic numeral digit. This class is equivalent to the bracket expression [0-9].

    \w —Matches any alphanumeric character from the basic Latin alphabet, including the underscore (_). This class is equivalent to the bracket expression [A-Za-z0-9_].

    \s —Matches a single whitespace character, including tabs and line breaks

Each of the last three character classes can be changed to perform an inverse match by capitalizing the letter character. For example, ``\D`` matches a non-digit character.

The ``\d`` character class is used in the section of the "Matching a URL" regex which defines the match the domain name: ``([\da-z\.-]+)``. As such, it is used to match any numerical digit in the URL's domain name.

The ``\w`` character class is used in the section near the end of the "Matching a URL" regex which matches optional paths or additional segment of the url. Because URL paths can contain a wide-ranging variety of character types, the (almost) all-inclusive ``\w`` character class is used.

### Bracket Expressions
 Anything inside a set of square brackets (``[]``) represents a range of characters that we want to match. These patterns are known as bracket expressions and they are also considered character classes. They are also known as a positive character group, because they outline the characters we want to include. We can write these expressions to include all of the characters we want to match. For example, ``[abc]`` will look for a string that includes a or b or c, regardless of the length of the string. So all of the following examples would match: "aaa", "bin" "court", "abracadabra", and "bca".

Hyphens (``-``) are used between alphanumeric characters (letters and numbers only) to represent a range of those possible characters. This means that [a-c] and [abc] will look for the exact same thing.

In the “Matching a URL” regex (shown again below), bracket expressions are used in multiple places. 

    /^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/

We can break down the bracket expressions as follows:

``([\da-z\.-]+)`` - The bracket expression in this paranthesised section uses the character class ``\d`` for all numerical values, the a-z range to match any lowercase letter, and a literal ``\.-`` to also match hypens and dots. This section is used to match the domain name that is followed by the optional http/https protocol.

``([a-z\.]{2,6})`` - The bracket expression in this section looks similar to the last one except we do not see the ``\d`` character class. It represents the top-level domain, such as .com, .org, or .io. As discussed in the [quantifiers](#quantifiers) section, it is limited to between 2 and 6 characters.


``([\/\w \.-]*)*`` - This component matches zero or more occurrences of a forward slash, alphanumeric characters, spaces, dots, or hyphens. It represents the path or additional segments in the URL.

### Greedy and Lazy Match

**"Greedy"** matching criteria are criteria that they match as many occurrences of particular patterns as possible. **"Lazy"** matching criteria will match as few occurrences as possible. Quantifiers are inherently greedy but can be made lazy by adding the ``?`` symbol at the end of the quantifier.

### Examples

Using all the concepts as previously discussed, we can observe how the following examples match the regex (shown again below)

    /^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/

* ``https://github.com/nataniel-c``
    * **https://** => matches optional protocol scheme criteria
    * **github** => contains all lowercase letters in domain
    * **.com** => is between 2 and 6 characters long and preceded by a dot
    * **/nataniel-c** => contains alphanumeric characters preceded by a slash

* ``g2a.com/category/gaming-c1/``
    * **g2a** => contains all lowercase letters AND a valid number in domain name
    * **.com** => is between 2 and 6 characters long and preceded by a dot
    * **/category/gaming-c1/** => contains alphanumeric characters preceded by a slash and followed by a trailing slash

* ``itch.io``
    * **itch** => all lowercase letters in domain name
    * **.io** => is between 2 and 6 characters long and preceded by a dot
    * Note: this URL meets minimum matching standards.

* ``https://youtu.be/2yJgwwDcgV8?si=QjvXLUoh800JRwn9``
    * **https://** => matches optional protocol scheme criteria
    * **youtu** => all lowercase letters in domain name
    * **.be** => is between 2 and 6 characters long and preceded by a dot
    * **/2yJgwwDcgV8?si=QjvXLUoh800JRwn9** => contains alphanumeric characters preceded by a slash


The following examples do not match:
* ``25``
    * No domain name followed by a dot and a top-level domain name
* ``HelloWorld!.com``
    * Capital letters in domain name
    * "!" is not a valid character for domain name
* ``htttps://www.whitehouse.government``
    * Protocol scheme has an extra "t." Does not match criteria
    * Top-level domain name is more than 6 characters long

## Author

This gist was created by Nataniel Carrasquillo.

https://github.com/nataniel-c