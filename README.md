# Swift Strings

## Strings are not arrays
* Swift’s strings look like arrays of letters, but that’s not really true.
```
let name = "Taylor"

for letter in name {
    print("Give me a \(letter)!"
}
```
the reason for this is that letters in a string aren’t just a series of alphabetical characters – they can contain accents such as á, é, í, ó, or ú, they can contain combining marks that generate wholly new characters by building up symbols, or they can even be emoji.

* if want to read the fourth character of name you need to start at the beginning and count through each letter until you come to the one you’re looking for:
```
let letter = name[name.index(name.startIndex, offsetBy: 3)]
```

* Apple could change this easily enough by adding a rather complex extension like this:
```
extension String {
    subscript(i: Int) -> String {
        return String(self[index(startIndex, offsetBy: i)])
    }
}
```

* reading ```name.count``` isn’t a quick operation: Swift literally needs to go over every letter counting up however many there are, before returning that. As a result, it’s always better to use ```someString.isEmpty``` rather than ```someString.count == 0 ``` if you’re looking for an empty string.
