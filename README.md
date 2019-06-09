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

## Working with strings in Swift

* First, there are methods for checking whether a string starts with or ends with a substring: ```hasPrefix()``` and ```hasSuffix()``` They look like this:
```
let password = "12345"
password.hasPrefix("123")
password.hasSuffix("345")
```

* can add extension methods to String to extend the way prefixing and suffixing works:
```
extension String {
    // remove a prefix if it exists
    func deletingPrefix(_ prefix: String) -> String {
        guard self.hasPrefix(prefix) else { return self }
        return String(self.dropFirst(prefix.count))
    }

    // remove a suffix if it exists
    func deletingSuffix(_ suffix: String) -> String {
        guard self.hasSuffix(suffix) else { return self }
        return String(self.dropLast(suffix.count))
    }
}
```
That uses the ```dropFirst()``` and ```dropLast()``` method of String, which removes a certain number of letters from either end of the string.

* The capitalized property that gives the first letter of each word a capital letter
```
let weather = "it's going to rain"
print(weather.capitalized)
```

could add our own specialized capitalization that uppercases only the first letter in our string:
```
extension String {
    var capitalizedFirst: String {
        guard let firstLetter = self.first else { return "" }
        return firstLetter.uppercased() + self.dropFirst()
    }
}
```

* Useful method of strings is ```contains()```, which returns true if it contains another string. So, this will return true:
```
let input = "Swift is like Objective-C without the C"
input.contains("Swift")
```
```
let languages = ["Python", "Ruby", "Swift"]
languages.contains("Swift")
```

* have an array of strings (["Python", "Ruby", "Swift"]) and we have an input string ("Swift is like Objective-C without the C"). How can we check whether any string in our array exists in our input string?

Well, we might start writing an extension on String like this:
```
extension String {
    func containsAny(of array: [String]) -> Bool {
        for item in array {
            if self.contains(item) {
                return true
            }
        }

        return false
    }
}
```
can now run our check like this:
```
input.containsAny(of: languages)
```

* arrays have a second ```contains()``` method called ```contains(where:)``` This lets us provide a closure that accepts an element from the array as its only parameter and returns true or false depending on whatever condition we decide we want. This closure gets run on all the items in the array until one returns true, at which point it stops.
```
languages.contains(where: input.contains)
```
- Now let’s put together the pieces:

1. When used with an array of strings, the ```contains(where:)``` method wants to call a closure that accepts a string and returns true or false.
2. The ```contains()``` method of String accepts a string as its parameter and returns true or false.
3. Swift massively blurs the lines between functions, methods, closures, and more.

## Formatting strings with NSAttributedString

* Attributed strings are made up of two parts:
a plain Swift string, plus a dictionary containing a series of attributes that describe how various segments of the string are formatted.
In its most basic form you might want to create one set of attributes that affect the whole string, like this:
```
let string = "This is a test string"
let attributes: [NSAttributedString.Key: Any] = [
    .foregroundColor: UIColor.white,
    .backgroundColor: UIColor.red,
    .font: UIFont.boldSystemFont(ofSize: 36)
]

let attributedString = NSAttributedString(string: string, attributes: attributes)
```

* It’s common to use an explicit type annotation when making attributed strings, because inside the dictionary we can just write things like ```.foregroundColor``` for the key rather than ```NSAttributedString.Key.foregroundColor```

* The values of the attributes dictionary are of type Any, because ```NSAttributedString``` attributes can be all sorts of things: numbers, colors, fonts, paragraph styles, and more.

* use ```NSMutableAttributedString```, which is an attributed string that you can modify:
```
let attributedString = NSMutableAttributedString(string: string)
attributedString.addAttribute(.font, value: UIFont.systemFont(ofSize: 8), range: NSRange(location: 0, length: 4))
attributedString.addAttribute(.font, value: UIFont.systemFont(ofSize: 16), range: NSRange(location: 5, length: 2))
attributedString.addAttribute(.font, value: UIFont.systemFont(ofSize: 24), range: NSRange(location: 8, length: 1))
attributedString.addAttribute(.font, value: UIFont.systemFont(ofSize: 32), range: NSRange(location: 10, length: 4))
attributedString.addAttribute(.font, value: UIFont.systemFont(ofSize: 40), range: NSRange(location: 15, length: 6))
```
When preview that you’ll see the font size get larger with each word.

* There are lots of formatting options for attributed strings, including:
1. Set ```.underlineStyle``` to a value from ```NSUnderlineStyle``` to strike out characters.
2. Set ```.strikethroughStyle``` to a value from ```NSUnderlineStyle``` (no, that’s not a typo) to strike out characters.
3. Set ```.paragraphStyle``` to an instance of ```NSMutableParagraphStyle``` to control text alignment and spacing.
4. Set ```.link ``` to be a URL to make clickable links in your strings.



