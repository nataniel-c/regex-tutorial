# Regex Tutorial - Matching a URL

Regular expressions, or regex, are series of special characters used in coding that define a search pattern. Regex can be used to verify that a user input is valid based on a specific criteria. This gist tutorial explains the functionality of a regex expression used to match a URL

## Summary

Briefly summarize the regex you will be describing and what you will explain. Include a code snippet of the regex. Replace this text with your summary.

The following code is the "Matching a URL" regex:

```
/^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/
```
This regex can be used to verify whether a set of characters, or string, matches a URL format by evaluating each character in the string one by one. It matches both http and https protocols, domain names, top-level domains, and optional paths or additional segments in the URL. This gist will break down the different characters of this regex and explain how they are used to create a URL matching regex. 

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [OR Operator](#or-operator)
- [Character Classes](#character-classes)
- [Flags](#flags)
- [Grouping and Capturing](#grouping-and-capturing)
- [Bracket Expressions](#bracket-expressions)
- [Greedy and Lazy Match](#greedy-and-lazy-match)
- [Boundaries](#boundaries)
- [Back-references](#back-references)
- [Look-ahead and Look-behind](#look-ahead-and-look-behind)

## Regex Components

The following section will discuss each component of the regex and explain its functionality. The most effective way to explain the regex statement is to break it down into the different sections which are all wrapped in parenthesis.

Starting with the beginning of the regex: the "Matching a URL" regex, as with every other regex, must be wrapped in slash characters (**/**) because it is considered a literal:

``/^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/``

    Note: JavaScript provides two ways to create a regex object. The first, shown in our example, uses literal notation. The second is to use a RegExp constructor. The constructor function's parameters are not enclosed within slashes; instead, they use quotation marks. To learn more, review the MDN Web Docs on the RegExp object.

After the **/** wrappers, the regex begins and ends with anchors, which will be discussed in the following section.


### Anchors
The characters ^ and $ are both considered to be anchors.

The ^ anchor signifies a string that begins with the characters that follow it. This could be in one of two formats:
- An exact string match, such as ^The, where the strings "The" or "The person" match, but "the" and "the person" do not. This is because a regex is case-sensitive.

- A range of possible matches, displayed using bracket expressions. We'll discuss this in the next section.

The $ anchor signifies a string that ends with the characters that precede it. Just as with the ^ character, it can be preceded by an exact string or a range of possible matches.

In the “Matching a URL" regex, the ^ and $ anchors are each used once: at the beginning and end of the expression respectively, to define the beginning and end of the regex. Quantifiers, which will be discussed in the next section, allow the regex to evaluate the characters defined within these anchors.

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

The "Matching a URL" regex, shown again below, uses multiple quantifiers, which will be explained as they appear left to right in the regex.

    /^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/

The ? quantifier is used twice in the ``(https?:\/\/)?`` component. This section matches an optional "http://" or "https://" at the beginning of the URL string. A ? quantifier is attached to the ``s`` in ``https``, which means that it will match both ``http`` and ``https`` . Furthermore, another ? quantifier is attached at the end of the section, which is entirely wrapped in parenthesis. This means that whole matching criteria of ``https?:\/\/`` is optional and the regex can match urls that do not contain ``https://`` at the beginning. 

In the ``([\da-z\.-]+)`` component, the + quantifier is used at the end of the [bracket expression](#bracket-expressions) defining the possible characters that could make up the beginning of the url domain. This + quantifier will give a match for every url with at least one character as its domain string. 

The ``([a-z\.]{2,6})`` component has the quantifier "{2,6}". This means that we want to find the preceding string pattern a minimum of 2 times and a maximum of 6 times. Because our bracket expression ([a-z0-9_-]) will match any string that includes any combination of lowercase letters between a and z, any number between 0 and 9, and the special characters of an underscore or a hyphen, this quantifier means that this string has to be between 3 and 16 characters.

Let's return to our entire regex:



This regex is looking for any string between 3 and 16 characters that starts and ends with a combination of lowercase characters, the numbers 0–9, and the special characters of an underscore and a hyphen.

The following examples match:

    123

    l3rn-antin0_1

The 123 string is a match because it meets the minimum requirement of 3 characters, and the characters are all numbers. l3rn-antin0_1 is a match because it is 13 characters long (within the minimum and maximum character limits of 3 and 16) and includes a combination of lowercase characters, numbers, and the special characters of an underscore and a hyphen. Remember that because our regex pattern is entirely inside a bracket expression, the string doesn’t need to meet all of the requirements—just any of them.

The following examples do not match:

    25

    L3RN-antino0_1

    l3rn-antin0_am1k0

    l3rn*antin0?1

The 25 string is not a match because it only contains 2 characters and the minimum is 3. L3RN-antino0_1 includes uppercase characters. l3rn-antin0_am1k0 has 17 characters, and the maximum limit for this regex is 16. l3rn*antin0?1 includes the special characters * and ?, which aren't allowed in this regex.

Great! We've successfully explained the “Matching a Username” regex. But is that all there is to learn about regular expressions? This actually only scratches the surface of what we can accomplish by using a regex. Although it's beyond the scope of this tutorial to learn everything, let’s touch on a few other regex components that you might encounter.

### OR Operator
Remember that a bracket expression does not require the string to meet all of the requirements in the pattern. This means that [a-z0-9_-] searches for alphanumeric characters or the two special characters included in the pattern. Often, you'll want to add this same logic outside of a bracket expression, especially within a grouping construct or between two different grouping constructs.

Using the OR operator (|), the expression [abc] could be written as (a|b|c). Using our example in the grouping constructs section, we can take the original expression:

(abc):(xyz)

And then use the OR operator to convert it to the following:

(a|b|c):(x|y|z)

Now, both of the strings "abc:xyz" and "acb:xyz" would match, as well as "a:z", but "xyz:abc" would not.

### Character Classes
A character class in a regex defines a set of characters, any one of which can occur in an input string to fulfill a match. We've actually already discussed some character classes. The bracket expressions outlined previously, including positive and negative character groups, are considered character classes.

Here are some of the other common character classes:

    .—Matches any character except the newline character (\n)

    \d—Matches any Arabic numeral digit. This class is equivalent to the bracket expression [0-9].

    \w—Matches any alphanumeric character from the basic Latin alphabet, including the underscore (_). This class is equivalent to the bracket expression [A-Za-z0-9_].

    \s—Matches a single whitespace character, including tabs and line breaks

Each of the last three character classes can be changed to perform an inverse match by capitalizing the letter character. For example, \D matches a non-digit character.

### Flags
We started this tutorial by explaining that as a literal, a regex must be wrapped in slash characters. The one exception to this rule is with the component known as flags. Flags are placed at the end of a regex, after the second slash, and they define additional functionality or limits for the regex. There are six optional flags that can be used, either separately or together and in any order, but these are the three you're most likely to encounter:

    g—Global search: the regex should be tested against all possible matches in a string.

    i—Case-insensitive search: case should be ignored while attempting a match in a string

    m—Multi-line search: a multi-line input string should be treated as multiple lines

### Grouping and Capturing

### Bracket Expressions
Anything inside a set of square brackets ([]) represents a range of characters that we want to match. These patterns are known as bracket expressions, but they are also known as a positive character group, because they outline the characters we want to include. We can write these expressions to include all of the characters we want to match. For example, [abc] will look for a string that includes a or b or c, regardless of the length of the string. So all of the following examples would match: "aaa", "bin" "court", "abracadabra", and "bca".

You'll more commonly see a hyphen (-) used between alphanumeric characters (letters and numbers only) to represent a range of those possible characters. This means that [a-c] and [abc] will look for the exact same thing.

In our “Matching a Username” regex example, we can break down the bracket expressions as follows:

    [a-z]—The string can contain any lowercase letter between a–z. Keep in mind that this looks for lowercase characters only. If we wanted to include uppercase characters, we would need to change the expression to [a-zA-Z].

    [0-9]—The string can contain any number between 0–9

    [_-]—The string can contain an underscore or hyphen. Both the underscore and the hyphen are called special characters. Special characters include any non-alphanumeric characters, such as punctuation or symbols. In this case, we only want a string that includes _ or -. It's important to note that the hyphen here is not the same hyphen that we used in our alphanumeric ranges. In bracket expressions, special characters that we want to include follow alphanumeric character ranges within the brackets.

If we put all of these expressions together so that our pattern is [a-z0-9_-], this will match any string that includes any combination of lowercase letters between a and z, any number between 0 and 9, and the special characters of an underscore or a hyphen. Keep in mind that these characters can be in any order. It's also important to note that this pattern does not require the string to meet all of these requirements; it can meet any of them.

The following examples fulfill the requirements of this regex:

    "lernantino"

    "lernantino1"

    "l3rnantino_1"

    "lern-antino"

    "21452"

    "_-_-"

What about the string "Lernantino"? This would not match our pattern because it includes an uppercase character, L.

It's important to note that a bracket expression can be turned into a negative character group by adding the ^ symbol to the beginning of the expression inside the brackets. A common example is matching a string that doesn't include any vowels. The pattern [^aeiouAEIOU] would find any strings that don't include lowercase or uppercase vowels.

### Greedy and Lazy Match

"Greedy" matching criteria are criteria that they match as many occurrences of particular patterns as possible. "Lazy" matching criteria will match as few occurrences as possible. Quantifiers are inherently greedy but can be made lazy by adding the ? symbol at the end of the quantifier.

### Boundaries

### Back-references

### Look-ahead and Look-behind

## Author

A short section about the author with a link to the author's GitHub profile (replace with your information and a link to your profile)

https://github.com/nataniel-c