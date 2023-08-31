# RegEx Pattern Explantions

This was a fun project to work on. It really forced me to take a deep dive into regex. Below are the explanations for the schema, node and relationshp patterns I used. I did use GPT to help learn this, but note that GPT doens't always get regex correct! You'll need to dig into the expressions and understand them if you want to truly appreciate regex.

## Schema Pattern

```r'\((\w*),\s*(\w*),\s*(\w*)\)'```

The regex matches a string that starts with an opening parenthesis, followed by the source node label, a comma, optional whitespace, the relationship type label, another comma, optional whitespace, the target node variable, and ends with a closing parenthesis.

* ```\(``` : Matches an opening parenthesis character.
* ```([^,]+)``` : Matches any character except for a comma one or more times and captures it in a group. This is used to capture the source node label.
* ```,``` : Matches a comma character.
* ```\s*``` : Matches any whitespace character zero or more times. This is used to allow for optional whitespace between the source node and relationship type labels.
* ```([^,]+)``` : Matches any character except for a comma one or more times and captures it in a group. This is used to capture the relationship type label.
* ```,``` : Matches a comma character.
* ```\s*``` : Matches any whitespace character zero or more times. This is used to allow for optional whitespace between the relationship type and the target node labels.
* ```([^,]+)``` : Matches any character except for a comma one or more times and captures it in a group. This is used to capture the target node label.
* ```\)``` : Matches a closing parenthesis character.

## Node Pattern

```r'\(([\w]+)?:?([\w`:]+)?\s?(\{.*?\})?\)'```

The regex matches a string that starts with an opening parenthesis, followed by an optional node variable, an optional node label, optional whitespace, optional node properties, and ends with a closing parenthesis.

* ```\(``` : Matches an opening parenthesis character.
* ```([\w]+)?``` : Matches any word character (letters, digits, or underscores) one or more times and captures it in a group. This is used to capture the node variable, which is optional.
* ```:?``` : Matches a colon character zero or one time. This is used to allow for an optional label after the node variable.
* ```([\w:]+)?` ``` : Matches any word character, colon character, or backtick character one or more times and captures it in a group. This is used to capture the node label, which is optional.
* ```\s?``` : Matches any whitespace character zero or one time. This is used to allow for optional whitespace between the node label and the node properties.
* ```(\{.*?\})?``` : Matches a string that starts with an opening brace character, followed by any characters (except for a newline character) zero or more times, and ends with a closing brace character. This is used to capture the node properties, which are optional.
* ```\)``` : Matches a closing parenthesis character.

## Relationship Pattern

```r'\[?([\w]+)?:?([\w`*|!.]+)?\s*?(\{.*?\})?\]?'```

The regex matches a string that starts with an opening square bracket (which is optional), followed by an optional relationship variable, an optional relationship type, optional whitespace, optional relationship properties, and ends with a closing square bracket (which is optional).

* ```\]?``` : Matches a closing square bracket character zero or one time. This is used to allow for an optional closing square bracket at the end of the relationship pattern.
* ```([\w]+)?``` : Matches any word character (letters, digits, or underscores) one or more times and captures it in a group. This is used to capture the relationship variable, which is optional.
* ```:?``` : Matches a colon character zero or one time. This is used to allow for an optional type after the relationship variable.
* ```([\w*|!.]+)?` ``` : Matches any word character, backtick character, asterisk character, vertical bar character, exclamation mark character, or dot character one or more times and captures it in a group. This is used to capture the relationship type, which is optional.
* ```\s*?``` : Matches any whitespace character zero or more times, but as few times as possible. This is used to allow for optional whitespace between the relationship type and the relationship properties.
* ```(\{.*?\})?``` : Matches a string that starts with an opening brace character, followed by any characters (except for a newline character) zero or more times, and ends with a closing brace character. This is used to capture the relationship properties, which are optional.
* ```\]?``` : Matches a closing square bracket character zero or one time. This is used to allow for an optional closing square bracket at the end of the relationship pattern.

## Additional Info

You'll notice the use of optional groups above. It allows me to assume that 9 elements will be captaured, and if they don't exist they will have a value of None. This allows for easy unpacking into the data class I used to capture the information.

``` 
# Unpack the matched groups and create a new ParsedQuery instance 
parsed_pattern = ParsedPattern(*match.groups(), direction, match.group(), query, PatternStatus.UNVALIDATED)
```

