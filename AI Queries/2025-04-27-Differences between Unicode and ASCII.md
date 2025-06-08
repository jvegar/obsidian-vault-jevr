#unicode #ascii
# Differences Between Unicode and ASCII

## ASCII (American Standard Code for Information Interchange)
- **Character Set**: Contains only 128 characters (7-bit encoding)
- **Coverage**: Primarily English letters (uppercase and lowercase), digits, punctuation, and control characters
- **Limitations**: Cannot represent characters from other languages or special symbols
- **Usage**: Originally developed for teleprinters, still used in simple systems

## Unicode
- **Character Set**: Currently defines over 143,000 characters (as of Unicode 15.0)
- **Coverage**: Supports virtually all writing systems worldwide, plus symbols, emojis, and special characters
- **Encodings**: Multiple encoding forms (UTF-8, UTF-16, UTF-32) to represent characters
- **Compatibility**: Backward compatible with ASCII (UTF-8 matches ASCII for first 128 characters)
- **Usage**: Standard for modern computing and internet communication

## Key Differences
1. **Scope**: ASCII is English-only, Unicode is universal
2. **Size**: ASCII uses 7 bits per character, Unicode uses 8-32 bits depending on encoding
3. **Versatility**: Unicode can represent emojis, mathematical symbols, ancient scripts, etc.
4. **Modern Usage**: Unicode has largely replaced ASCII in most applications

Unicode was developed to solve ASCII's limitations and provide a single encoding standard for all languages and symbols.