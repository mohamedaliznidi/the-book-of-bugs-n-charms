# Strip whitespace from JavaScript strings

Here’s an example of `trim()` in action:

```
let str = "  Hello World!  ";
let trimStr = str.trim();

console.log(trimStr); // "Hello World!"
```

The `trim()` method doesn’t accept any parameter, and it only removes whitespace on either side of the string:

```
let str = "  Hello  Wo  rld!  ";
let trimStr = str.trim();

console.log(trimStr); // "Hello  Wo  rld!"
```

If you want to remove whitespaces from between the characters too, you need to the combination of the `split()` and `join()` methods to your string as follows:

```
let str = "  Hello  Wo  rld!  ";
let trimStr = str.split(' ').join('');

console.log(trimStr); // "HelloWorld!"
```

Or you can also use the `String.replace()` method that uses a regular expression to search whitespaces and replace them with empty string(`''`).

Here’s how you do it:

```
let str = "  Hello  Wo  rld!  ";
let trimStr = str.replace(/\s+/g, '');

console.log(trimStr); // "HelloWorld!"
```

The `/\s+/g` regex pattern will match all whitespace characters, and the `replace()` method will do the removal.
