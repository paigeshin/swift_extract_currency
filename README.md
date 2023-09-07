# swift_extract_currency

```swift
import Foundation

extension String {
    
    func extractSpecialCharacters() -> String {
        let text = self
        let pattern = "[^0-9a-zA-Z]"
        if let regex = try? NSRegularExpression(pattern: pattern) {
            let matches = regex.matches(in: text, options: [], range: NSRange(location: 0, length: text.utf16.count))
            let matchedCharacters = matches.flatMap { match -> [String] in
                return (0..<match.numberOfRanges).map { rangeIndex in
                    let range = match.range(at: rangeIndex)
                    let startIndex = text.utf16.index(text.utf16.startIndex, offsetBy: range.location)
                    let endIndex = text.utf16.index(startIndex, offsetBy: range.length)
                    return String(text[startIndex..<endIndex])
                }
            }
            return matchedCharacters.joined()
        } else {
            return ""
        }
    }
    
    func containsCurrencySymbol(text: String) -> Bool {
        let currencySymbols: Set<Character> = ["$", "€", "£", "₹", "¥", "₩", "₪", "₱", "₦", "₲", "₴", "₵", "₸", "₺", "₽", "₿", "¢", "₡", "₣", "₤", "₥", "₧", "₨", "₩", "₪", "₫", "€", "₭", "₮", "₯", "₰", "₱", "₲", "₳", "₴", "₵", "₶", "₷", "₸", "₹", "₺", "₻", "₼", "₽", "₾", "₿"]
        
        for character in text {
            if currencySymbols.contains(character) {
                return true
            }
        }
        return false
    }


    func extractCurrency() -> String {
        let text = self
        let specialCharacters = text.extractSpecialCharacters()
        for character in specialCharacters {
            if String(character).containsCurrencySymbol(text: specialCharacters) {
                return String(character)
            }
        }
        return "" 
    }
    
}


```
