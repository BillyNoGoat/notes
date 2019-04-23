### terms
#### Events
Events are places in `./events`. These handle each discord.js event like `.ready` or `guildCreate`.
If a conflicting event is present in both the core and your client, only your client version is loaded and will run when that event is triggered.

#### Monitors
Monitors will always run on any message. This is useful for things like checking a message like a profanity filter etc.
They are almost completely identical to inhibitors, the only difference between one is ran on the message, and the other is ran on the command
Inhibitors and Monitors share the same structure but monitors don't want a command parameter passed to them.

#### Extendables
Extendables extend the current discord.js classes allowing you to set custom properties and methods.
The extend method can only be a setter, getter or method. You can't define multiple in one file.

An example of why we would use this is if we wanted to add a method of `message.prompt` to allow us to easily create a message promping a user "Are you sure"?

#### Providers
Data providers are designed to make life easier using a database. There's no rules for this, but by default Klasa uses JSON to store configuration.

We can call our providers using `client.providers.get(ProviderName)`.

#### Inhibitors
Inhibitors are only run on commands. They can check conditions before a command is ever ran such as checking if they have permissions.
The structure is specific so they must be defined exactly as the online example shows to work. They must also return a promise which can be done using `async` or returning a new promise. An inhibitor can prevent a command from running by rejecting the promise object or with an empty string or `undefined` for silent rejections.
