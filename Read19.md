# Automation

## Python Regular Expression

Regular Expressions, often shortened as regex, are a sequence of characters used to check whether a pattern exists in a given text (string) or not. 

If you've ever used search engines, search and replace tools of word processors and text editors - you've already seen regular expressions in use.

They are used at the server side to validate the format of email addresses or passwords during registration, used for parsing text data files to find, replace, or delete certain string, etc.

You will start with importing `re` Python library that supports regular expressions
Then you will see how basic/ordinary characters are used for performing matches
Next, you'll learn about using repetitions in your regular expressions

you have to import this module with the help of import:

`import re`

### Basic Patterns: Ordinary Characters

You can easily tackle many basic patterns in Python using ordinary characters. Ordinary characters are the simplest regular expressions.

Ordinary characters can be used to perform simple exact matches:

```
pattern = r"Cookie"
sequence = "Cookie"
if re.match(pattern, sequence):
    print("Match!")
else: print("Not a match!")
```

`Match!`

Most alphabets and characters will match themselves, as you saw in the example.

The match() function returns a match object if the text matches the pattern. Otherwise, it returns None. The re module also contains several other functions, and you will learn some of them later on in the tutorial.

### Wild Card Characters: Special Characters
Special characters are characters that do not match themselves as seen but have a special meaning when used in a regular expression.

For simple understanding, they can be thought of as reserved metacharacters that denote something else and not what they look like.

But before you do, the examples below make use of two functions namely: search() and group().

**.** A period. Matches any single character except the newline character.

```
re.search(r'Co.k.e', 'Cookie').group()
```
`'Cookie'`

**^** A caret. Matches the start of the string.

`re.search(r'^Eat', "Eat cake!").group()`
`'Eat'`

**$** Matches the end of string.

```
re.search(r'cake$', "Cake! Let's eat cake").group()
```
`'cake'`

**\[abc\]** Matches a or b or c.
**\[a-zA-Z0-9\]** Matches any letter from (a to z) or (A to Z) or (0 to 9).

```
re.search(r'[0-6]', 'Number: 5').group()
```
`'5'`

**\\** Backslash.

- If the character following the backslash is a recognized escape character, then the special meaning of the term is taken (Scenario 1)
- Else if the character following the \ is not a recognized escape character, then the \ is treated like any other character and passed through (Scenario 2).
- \ can be used in front of all the metacharacters to remove their special meaning (Scenario 3).

```
re.search(r'Just a \regular character', 'Just a \regular character').group()
```
`'Just a \regular character'`

**\w** Lowercase 'w'. Matches any single letter, digit, or underscore.
**\W** Uppercase 'W'. Matches any character not part of \w (lowercase w).

```
print("Lowercase w:", re.search(r'Co\wk\we', 'Cookie').group())
```
`Lowercase w: Cookie`

**\s** Lowercase 's'. Matches a single whitespace character like: space, newline, tab, return.
**\S** Uppercase 'S'. Matches any character not part of \s (lowercase s).

```
print("Lowercase s:", re.search(r'Eat\scake', 'Eat cake').group())
```
`Lowercase s: Eat cake`

**\t** Lowercase t. Matches tab.
**\n** Lowercase n. Matches newline.
**\r** Lowercase r. Matches return.
**\A** Uppercase a. Matches only at the start of the string. Works across multiple lines as well.
**\Z** Uppercase z. Matches only at the end of the string.

TIP: ^ and \A are effectively the same, and so are $ and \Z. Except when dealing with MULTILINE mode. Learn more about it in the flags section.

**\b** Lowercase b. Matches only the beginning or end of the word.

```
print("\\t (TAB) example: ", re.search(r'Eat\tcake', 'Eat    cake').group())
```
`\t (TAB) example:  Eat    cake`

### Repetitions

**\+** Checks if the preceding character appears one or more times starting from that position.

```
re.search(r'Co+kie', 'Cooookie').group()
```
`'Cooookie'`

**\*** Checks if the preceding character appears zero or more times starting from that position.

```
re.search(r'Ca*o*kie', 'Cookie').group()
```
`'Cookie'`

**?** Checks if the preceding character appears exactly zero or one time starting from that position.

```
re.search(r'Colou?r', 'Color').group()
```
`'Color'`

`{x}` Repeat exactly x number of times.
`{x,}` Repeat at least x times or more.
`{x, y}` Repeat at least x times but no more than y times.

### Greedy vs. Non-Greedy Matching

When a special character matches as much of the search sequence (string) as possible, it is said to be a "Greedy Match". It is the normal behavior of a regular expression, but sometimes this behavior is not desired:

```
pattern = "cookie"
sequence = "Cake and cookie"

heading  = r'<h1>TITLE</h1>'
re.match(r'<.*>', heading).group()
```

`'<h1>TITLE</h1>'`

### Function provided by 're'

The re library in Python provides several functions to make your tasks easier. You have already seen some of them, such as the re.search(), re.match()

`compile(pattern, flags=0)`

Regular expressions are handled as strings by Python. However, with compile(), you can computer a regular expression pattern into a regular expression object

```
pattern = re.compile(r"cookie")
sequence = "Cake and cookie"
pattern.search(sequence).group()
```

`search(pattern, string, flags=0)`

With this function, you scan through the given string/sequence, looking for the first location where the regular expression produces a match. It returns a corresponding match object if found, else returns None if no position in the string matches the pattern.

```
pattern = "cookie"
sequence = "Cake and cookie"

re.search(pattern, sequence)
```
`<re.Match object; span=(9, 15), match='cookie'>`


`match(pattern, string, flags=0)`

Returns a corresponding match object if zero or more characters at the beginning of string match the pattern. Else it returns None, if the string does not match the given pattern.

```
pattern = "C"
sequence1 = "IceCream"
sequence2 = "Cake"

# No match since "C" is not at the start of "IceCream"
print("Sequence 1: ", re.match(pattern, sequence1))
print("Sequence 2: ", re.match(pattern,sequence2).group())
```

Sequence 1:  None
Sequence 2:  C

`search() versus match()`

- The match() function checks for a match only at the beginning of the string (by default), whereas the search() function checks for a match anywhere in the string.

`findall(pattern, string, flags=0)`
Finds all the possible matches in the entire sequence and returns them as a list of strings. Each returned string represents one match.

```
statement = "Please contact us at: support@datacamp.com, xyz@datacamp.com"

#'addresses' is a list that stores all the possible match
addresses = re.findall(r'[\w\.-]+@[\w\.-]+', statement)
for address in addresses:
    print(address)
```
support@datacamp.com
xyz@datacamp.com

### Compilation Flags

**IGNORECASE (I)** Allows case-insensitive matches.
**DOTALL (S)** Allows . to match any character, including newline.
**MULTILINE (M)** Allows start of string (^) and end of string ($) anchor to match newlines as well.
**VERBOSE (X)** Allows you to write whitespace and comments within a regular expression to make it more readable.
