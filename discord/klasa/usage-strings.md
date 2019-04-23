### Usage Strings
Usage strings are used to define the usage of your command and the data you expect to see as arugments. They are represented just as strings in your object under the `usage` property.

- `<>` Means the arugment is required
- `[]` Means the argument

An example: `<Name:Type{Min,Max}/Regex/Flags>`

- **Name** is mostly used for debugging message unless the type is Literal in which it compares the argument to the name.
- **Type** The type of variable you are accepting (See usage types in usage structure page).
- **Min, max** The minimum or maximum values. For strings this applies to length and for numbers it applies to value. You can give any combination of these, omit for none or set both min/max to the same integer then the provided string will need equal length.
- **Regex, flags** A regular expression that is doubled escaped with `\`. It is only valid for regex types of arguments but gives flexibility for custom arugment parsing. Flags are applied to the regex pattern.
- **Special repeat tag [\*\*\*]** will repeat the last usage optionally until you run out of arguments. Useful for doing something like `<SearchTerm:str> [...]` which will allow you to take as many search terms as you want per your Usage Delimeter.

**NOTE**
You can set multiple options in an argument by writing `|`. For example `<Message:msg|Content:string{4,16}>` will work when you provide a message ID or a string with a length between 4 and 16.
