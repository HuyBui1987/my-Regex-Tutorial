# Basic Regex Tutorial

This Regex Tutorial is explaining and providing you how the basic Regular Expression works. The Instruction will be breaking down as parts below.

## Summary

This tutorial introduces the concepts, objects and functions used to work with and perform Regular Expression in Javascripts.

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [Grouping Constructs](#grouping-constructs)
- [Bracket Expressions](#bracket-expressions)
- [Character Classes](#character-classes)
- [The OR Operator](#the-or-operator)
- [Flags](#flags)
- [Character Escapes](#character-escapes)

## Regex Components
- Creating a regular expression:
Using a regular expression literal, which consists of a pattern enclosed between slashes, as follows
```
const re = /ab+c/;
```
or calling the constructor function of the RegExp object, as follows 
```
const re = new RegExp('ab+c');
```
- Using simple patterns:
Simple patterns are constructed of characters for which you want to find a direct match. For example, the pattern /abc/ matches character combinations in strings only when the exact sequence "abc" occurs (all characters together and in that order). Such a match would succeed in the strings "Hi, do you know your abc's?" and "The latest airplane designs evolved from slabcraft.". In both cases the match is with the substring "abc". There is no match in the string "Grab crab" because while it contains the substring "ab c", it does not contain the exact substring "abc".

- Using special characters:
When the search for a match requires something more than a direct match, such as finding one or more b's, or finding white space, you can include special characters in the pattern. For example, to match a single "a" followed by zero or more "b"s followed by "c", you'd use the pattern /ab*c/: the * after "b" means "0 or more occurrences of the preceding item." In the string "cbbabbbbcdebc", this pattern will match the substring "abbbbc".

### Anchors

Anchors have special meaning in regular expressions. They do not match any character. Instead, they match a position before or after characters:
 ^ – The caret anchor matches the beginning of the text.
 $ – The dollar anchor matches the end of the text.
 m - flag to enable the multiline mode that instructs the ^ and $ anchors to match the beginning and end of the text as well as the beginning and end of the line.

 Example: 
```
let str = 'JavaScript';
console.log(/^J/.test(str));

output: True.
```
The /^J/ match any text that starts with the letter J. It returns true.

The following example returns false because the string JavaScript doesn’t start with the letter S:
```
let str = 'JavaScript';
console.log(/^S/.test(str));
```
The default of the anchor ^ or $ is the single-line mode. In the single-line mode, the anchor ^ and $ matches the beginning and the end of a string.
To enable the multiline mode, you use m flag. In the multiline mode, the ^ or $ anchor matches the beginning or end of the string as well as the beginning or end of lines.

Example: 
```
let str = `1st line
2nd line
3rd line`;

let re = /^\d/g;
let matches = str.match(re);

console.log(matches);
```
output: ["1"]
 

### Quantifiers

Quantifiers indicate numbers of characters or expressions to match.

x* - Matches the preceding item "x" 0 or more times. For example, /bo*/ matches "boooo" in "A ghost booooed" and "b" in "A bird warbled", but nothing in "A goat grunted".
x? - Matches the preceding item "x" 0 or 1 times. For example, /e?le?/ matches the "el" in "angel" and the "le" in "angle."(If used immediately after any of the quantifiers *, +, ?, or {}, makes the quantifier non-greedy (matching the minimum number of times), as opposed to the default, which is greedy (matching the maximum number of times)).
x{n} - Where "n" is a positive integer, matches exactly "n" occurrences of the preceding item "x". For example, /a{2}/ doesn't match the "a" in "candy", but it matches all of the "a"'s in "caandy", and the first two "a"'s in "caaandy".
x{n,} - Where "n" is a positive integer, matches at least "n" occurrences of the preceding item "x". For example, /a{2,}/ doesn't match the "a" in "candy", but matches all of the a's in "caandy" and in "caaaaaaandy".
x{n,m} - Where "n" is 0 or a positive integer, "m" is a positive integer, and m > n, matches at least "n" and at most "m" occurrences of the preceding item "x". For example, /a{1,3}/ matches nothing in "cndy", the "a" in "candy", the two "a"'s in "caandy", and the first three "a"'s in "caaaaaaandy". Notice that when matching "caaaaaaandy", the match is "aaa", even though the original string had more "a"s in it.
[x*?, x+?, x??, x{n}?, x{n,}?, x{n,m}?] - By default quantifiers like * and + are "greedy", meaning that they try to match as much of the string as possible. The ? character after the quantifier makes the quantifier "non-greedy": meaning that it will stop as soon as it finds a match. For example, given a string like "some <foo> <bar> new </bar> </foo> thing":
    - /<.*>/ will match "<foo> <bar> new </bar> </foo>"
    - /<.*?>/ will match "<foo>"
Example:
* Repeated pattern: 
```
const wordEndingWithAs = /\w+a+\b/;
const delicateMessage = "This is Spartaaaaaaa";

console.table(delicateMessage.match(wordEndingWithAs)); // [ "Spartaaaaaaa" ]
```
* Counting characters:
```
const singleLetterWord = /\b\w\b/g;
const notSoLongWord = /\b\w{2,6}\b/g;
const loooongWord = /\b\w{13,}\b/g;

const sentence = "Why do I have to learn multiplication table?";

console.table(sentence.match(singleLetterWord)); // ["I"]
console.table(sentence.match(notSoLongWord));    // [ "Why", "do", "have", "to", "learn", "table" ]
console.table(sentence.match(loooongWord));      // ["multiplication"]
```
* Optional character:
```
const britishText = "He asked his neighbour a favour.";
const americanText = "He asked his neighbor a favor.";

const regexpEnding = /\w+ou?r/g;
// \w+ One or several letters
// o   followed by an "o",
// u?  optionally followed by a "u"
// r   followed by an "r"

console.table(britishText.match(regexpEnding));
// ["neighbour", "favour"]

console.table(americanText.match(regexpEnding));
// ["neighbor", "favor"]
```
* Greedy versus non-greedy:
```
const text = "I must be getting somewhere near the center of the earth.";
const greedyRegexp = /[\w ]+/;
// [\w ]      a letter of the latin alphabet or a whitespace
//      +     one or several times

console.log(text.match(greedyRegexp)[0]);
// "I must be getting somewhere near the center of the earth"
// almost all of the text matches (leaves out the dot character)

const nonGreedyRegexp = /[\w ]+?/; // Notice the question mark
console.log(text.match(nonGreedyRegexp));
// "I"
// The match is the smallest one possible
```

### Grouping Constructs

Groups and ranges indicate groups and ranges of expression characters.
x|y - Matches either "x" or "y". For example, /green|red/ matches "green" in "green apple" and "red" in "red apple".
[xyz]/[a-c] - A character class. Matches any one of the enclosed characters. You can specify a range of characters by using a hyphen, but if the hyphen appears as the first or last character enclosed in the square brackets it is taken as a literal hyphen to be included in the character class as a normal character.
    For example, [abcd] is the same as [a-d]. They match the "b" in "brisket", and the "c" in "chop".
    For example, [abcd-] and [-abcd] match the "b" in "brisket", the "c" in "chop", and the "-" (hyphen) in "non-profit".
[^xyz]/[^a-c] - A negated or complemented character class. That is, it matches anything that is not enclosed in the brackets. You can specify a range of characters by using a hyphen, but if the hyphen appears as the first or last character enclosed in the square brackets it is taken as a literal hyphen to be included in the character class as a normal character. For example, [^abc] is the same as [^a-c]. They initially match "o" in "bacon" and "h" in "chop".
(?<Name>x) - Named capturing group: Matches "x" and stores it on the groups property of the returned matches under the name specified by <Name>. The angle brackets (< and >) are required for group name.
    For example, to extract the United States area code from a phone number, we could use /\((?<area>\d\d\d)\)/. The resulting number would appear under matches.groups.area
Example:
* Counting vowels:
```
var aliceExcerpt = "There was a long silence after this, and Alice could only hear whispers now and then.";
var regexpVowels = /[AEIOUYaeiouy]/g;

console.log("Number of vowels:", aliceExcerpt.match(regexpVowels).length);
// Number of vowels: 26
```
* Using name groups:
```
let personList = `First_Name: John, Last_Name: Doe
First_Name: Jane, Last_Name: Smith`;

let regexpNames =  /First_Name: (?<firstname>\w+), Last_Name: (?<lastname>\w+)/mg;
let match = regexpNames.exec(personList);
do {
  console.log(`Hello ${match.groups.firstname} ${match.groups.lastname}`);
} while((match = regexpNames.exec(personList)) !== null);
```
* Using groups:
```
let personList = `First_Name: John, Last_Name: Doe
First_Name: Jane, Last_Name: Smith`;

let regexpNames =  /First_Name: (\w+), Last_Name: (\w+)/mg;
let match = regexpNames.exec(personList);
do {
  console.log(`Hello ${match[1]} ${match[2]}`);
} while((match = regexpNames.exec(personList)) !== null);
```

### Bracket Expressions

[ ] - Brackets indicate a set of characters to match. Any individual character between the brackets will match, and you can also use a hyphen to define a set.
Example:
```
'elephant'.match(/[abcd]/) // -> matches 'a'
'donkey'.match(/[^abcd]/) // -> matches 'o'
'elephant'.match(/[a-d]/) // -> matches 'a'
'elephant'.match(/[A-D]/) // -> no match
'elephant'.match(/[A-D]/i) // -> matches 'a'
```
{ } - Curly braces are used to specify an exact amount of things to match. 
Example: They are used after an expression: \na{2}\ will only match ‘na’ exactly twice.
```
'panama'.match(/na{2}/) // -> no match
'banana'.match(/na{2}/) // -> matches 'nana'
```
* You can use these in conjunction with a comma to specify more than one amount. {2,} = two or more times, {2,4} = between two and four times.
```
'banana'.match(/a{2,4}/) // -> no match
'bananaa'.match(/a{2,4}/) // -> matches 'aa'
'bananaaa'.match(/a{2,4}/) // -> matches 'aaa'
'bananaaaa'.match(/a{2,4}/) // -> matches 'aaaa'
'bananaaaaaaaaaaa'.match(/a{2,4}/) // -> matches 'aaaa'
```
( ) - Parentheses represent remembered matches. This is especially useful for find-and-replace operations or any time you need to do something with part of the match.
Example: When a match is remembered you can use $n to refer to it, starting with $1 up to $9, or with $& to refer to the entire match.
```
'Firsty McLastname'.match(/([A-Za-z]+)\s([A-Za-z]+)/) // -> matches 'First McLastname' with 'Firsty' remembered as $1 and 'McLastname' as $2
'Firsty McLastname'.replace(/([A-Za-z]+)\s([A-Za-z]+)/, '$1') // -> returns 'Firsty'
'Firsty McLastname'.replace(/([A-Za-z]+)\s([A-Za-z]+)/, '$2, $1') // -> returns 'McLastname, Firsty'
'Firstipher Lasterman'.replace(/([A-Za-z]+)\s([A-Za-z]+)/, '$&') // -> returns 'Firstipher Lasterman'
```

### Character Classes

Character classes distinguish kinds of characters such as, for example, distinguishing between letters and digits.
. - Has one of the following meanings:
        - Matches any single character except line terminators: \n, \r, \u2028 or \u2029. For example, /.y/ matches "my" and "ay", but not "yes", in "yes make my day".
        - Inside a character class, the dot loses its special meaning and matches a literal dot.
        - Note that the m multiline flag doesn't change the dot behavior. So to match a pattern across multiple lines, the character class [^] can be used — it will match any character including newlines.
\d - Matches any digit (Arabic numeral). Equivalent to [0-9]. For example, /\d/ or /[0-9]/ matches "2" in "B2 is the suite number".
ES2018 added the s "dotAll" flag, which allows the dot to also match line terminators.
\D - Matches any character that is not a digit (Arabic numeral). Equivalent to [^0-9]. 
    For example, /\D/ or /[^0-9]/ matches "B" in "B2 is the suite number".
\w - Matches any alphanumeric character from the basic Latin alphabet, including the underscore. Equivalent to [A-Za-z0-9_]. 
    For example, /\w/ matches "a" in "apple", "5" in "$5.28", "3" in "3D" and "m" in "Émanuel".
\W - Matches any character that is not a word character from the basic Latin alphabet. Equivalent to [^A-Za-z0-9_].  
    For example, /\W/ or /[^A-Za-z0-9_]/ matches "%" in "50%" and "É" in "Émanuel".
\s - Matches a single white space character, including space, tab, form feed, line feed, and other Unicode spaces. Equivalent to [ \f\n\r\t\v\u00a0\u1680\u2000-\u200a\u2028\u2029\u202f\u205f\u3000\ufeff]. 
    For example, /\s\w*/ matches " bar" in "foo bar".
\S - Matches a single character other than white space. Equivalent to [^ \f\n\r\t\v\u00a0\u1680\u2000-\u200a\u2028\u2029\u202f\u205f\u3000\ufeff]. 
    For example, /\S\w*/ matches "foo" in "foo bar".
\t - Matches a horizontal tab.
\r - Matches a carriage return.
\n - Matches a linefeed.
\v - Matches a vertical tab.
\f - Matches a form-feed.
[\b] - Matches a backspace. If you're looking for the word-boundary character (\b).
Example:
* Looking for a series of digits:
```
var randomData = "015 354 8787 687351 3512 8735";
var regexpFourDigits = /\b\d{4}\b/g;
// \b indicates a boundary (i.e. do not start matching in the middle of a word)
// \d{4} indicates a digit, four times
// \b indicates another boundary (i.e. do not end matching in the middle of a word)

console.table(randomData.match(regexpFourDigits));
// ['8787', '3512', '8735']
```
* Looking for a word (from the latin alphabet) starting with A:
```
var aliceExcerpt = "I'm sure I'm not Ada,' she said, 'for her hair goes in such long ringlets, and mine doesn't go in ringlets at all.";
var regexpWordStartingWithA = /\b[aA]\w+/g;
// \b indicates a boundary (i.e. do not start matching in the middle of a word)
// [aA] indicates the letter a or A
// \w+ indicates any character *from the latin alphabet*, multiple times

console.table(aliceExcerpt.match(regexpWordStartingWithA));
// ['Ada', 'and', 'at', 'all']
```
* Looking for a word (from Unicode characters):
```
var nonEnglishText = "Приключения Алисы в Стране чудес";
var regexpBMPWord = /([\u0000-\u0019\u0021-\uFFFF])+/gu;
// BMP goes through U+0000 to U+FFFF but space is U+0020

console.table(nonEnglishText.match(regexpBMPWord));
[ 'Приключения', 'Алисы', 'в', 'Стране', 'чудес' ]
```

### The OR Operator

Alternation is the term in regular expression that is actually a simple “OR”. In a regular expression it is denoted with a vertical line character |.
    For instance, we need to find programming languages: HTML, PHP, Java or JavaScript.
Example:
Example: 
* regexp for time:
We can use more careful matching. First, the hours:
- If the first digit is 0 or 1, then the next digit can be any: [01]\d.
- Otherwise, if the first digit is 2, then the next must be [0-3].
- (no other first digit is allowed)
* minutes are added to the second alternation variant:
```
let regexp = /([01]\d|2[0-3]):[0-5]\d/g;
alert("00:00 10:10 23:59 25:99 1:2".match(regexp)); // 00:00,10:10,23:59
```
* Find programming languages: Create a regexp that finds them in the string Java JavaScript.
```
let regexp = /your regexp/g;

alert("Java JavaScript PHP C++ C".match(regexp)); // Java JavaScript PHP C++ C
```

### Flags

Regular expressions have optional flags that allow for functionality like global searching and case-insensitive searching. These flags can be used separately or together in any order, and are included as part of the regular expression.

d - Generate indices for substring matches. (RegExp.prototype.hasIndices)
g - Global search. (RegExp.prototype.global)
i - Case-insensitive search. (RegExp.prototype.ignoreCase)
m - Multi-line search. (RegExp.prototype.multiline)
s - Allows . to match newline characters. (RegExp.prototype.dotAll)
u - "unicode"; treat a pattern as a sequence of unicode code points. (RegExp.prototype.unicode)
y - Perform a "sticky" search that matches starting at the current position in the target string. See sticky.(RegExp.prototype.sticky)
To include a flag with the regular expression, use this syntax:
```
const re = /pattern/flags;
```
or:
```
const re = new RegExp('pattern', 'flags');
```
For example, re = /\w+\s/g creates a regular expression that looks for one or more characters followed by a space, and it looks for this combination throughout the string.
```
const re = /\w+\s/g;
const str = 'fee fi fo fum';
const myArray = str.match(re);
console.log(myArray);

// ["fee ", "fi ", "fo "]
```

### Character Escapes

- If you need to use any of the special characters literally (actually searching for a "*", for instance), you must escape it by putting a backslash in front of it. For instance, to search for "a" followed by "*" followed by "b", you'd use /a\*b/ — the backslash "escapes" the "*", making it literal instead of special.
- Similarly, if you're writing a regular expression literal and need to match a slash ("/"), you need to escape that (otherwise, it terminates the pattern). For instance, to search for the string "/example/" followed by one or more alphabetic characters, you'd use /\/example\/[a-z]+/i—the backslashes before each slash make them literal.
- To match a literal backslash, you need to escape the backslash. For instance, to match the string "C:\" where "C" can be any letter, you'd use /[A-Z]:\\/ — the first backslash escapes the one after it, so the expression searches for a single literal backslash.
- If using the RegExp constructor with a string literal, remember that the backslash is an escape in string literals, so to use it in the regular expression, you need to escape it at the string literal level. /a\*b/ and new RegExp("a\\*b") create the same expression, which searches for "a" followed by a literal "*" followed by "b".
- If escape strings are not already part of your pattern you can add them using String.replace:
```
function escapeRegExp(string) {
  return string.replace(/[.*+?^${}()|[\]\\]/g, '\\$&'); // $& means the whole matched string
}
```

## Author

Name: Xuan Huy Bui

[Conctact me via github](https://github.com/HuyBui1987) 

[my LinkedIn](https://www.linkedin.com/in/huy-bui-xuan-b0b079238/)
