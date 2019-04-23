### Usage of arguments in Klasa
Now we understand how usage strings work, we can implement them in our code.
`[...params]` in our `async run` represents a variable number of parameters given when the command is run. The name of those parameters in the array and their count is determined by the usage property and it's arguments.

This is known as destructuring.

Let's look at an example usage string:
`<Message:msg> <delete|edit> [newContent:string]` with a usageDelim as `|`.
We can assume the code would look like the following:
```javascript {cmd="node"}
async run(msg, [message, action, newContent]) {
  // code
};
```

In which `message` is the argument assigned to the message object as provided in `<Message:msg>`. Same does `action` for `<delete|edit>` and respectively.
**Note:** if we didn't define a third parameter (`[]` means optional) then `newContent` would return undefined.

#### Using Regex
We can use even more flexibility to defined custom matching. Regex needs to be double escaped like the following:
`<hexColor:regex/#?([\\da-f]{6})/i>` That regex will resolve into `/#?([\da-f]{6})/i` which should match hex strings.

Then all we need to do is the following destructuring to get the first matching group of the hexColor arguments:

```javascript {cmd="node"}
async run(msg, [[, hexColor]]) {
  //code
}

```
So the value of `hexColor` becomes `ab24ff`.
