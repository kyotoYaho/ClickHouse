# Functions for working with strings

## empty {#string_functions-empty}

Returns 1 for an empty string or 0 for a non-empty string.
The result type is UInt8.
A string is considered non-empty if it contains at least one byte, even if this is a space or a null byte.
The function also works for arrays.

## notEmpty

Returns 0 for an empty string or 1 for a non-empty string.
The result type is UInt8.
The function also works for arrays.

## length

Returns the length of a string in bytes (not in characters, and not in code points).
The result type is UInt64.
The function also works for arrays.

## lengthUTF8

Returns the length of a string in Unicode code points (not in characters), assuming that the string contains a set of bytes that make up UTF-8 encoded text. If this assumption is not met, it returns some result (it doesn't throw an exception).
The result type is UInt64.

## char_length, CHAR_LENGTH

Returns the length of a string in Unicode code points (not in characters), assuming that the string contains a set of bytes that make up UTF-8 encoded text. If this assumption is not met, it returns some result (it doesn't throw an exception).
The result type is UInt64.

## character_length, CHARACTER_LENGTH

Returns the length of a string in Unicode code points (not in characters), assuming that the string contains a set of bytes that make up UTF-8 encoded text. If this assumption is not met, it returns some result (it doesn't throw an exception).
The result type is UInt64.

## lower, lcase

Converts ASCII Latin symbols in a string to lowercase.

## upper, ucase

Converts ASCII Latin symbols in a string to uppercase.

## lowerUTF8

Converts a string to lowercase, assuming the string contains a set of bytes that make up a UTF-8 encoded text.
It doesn't detect the language. So for Turkish the result might not be exactly correct.
If the length of the UTF-8 byte sequence is different for upper and lower case of a code point, the result may be incorrect for this code point.
If the string contains a set of bytes that is not UTF-8, then the behavior is undefined.

## upperUTF8

Converts a string to uppercase, assuming the string contains a set of bytes that make up a UTF-8 encoded text.
It doesn't detect the language. So for Turkish the result might not be exactly correct.
If the length of the UTF-8 byte sequence is different for upper and lower case of a code point, the result may be incorrect for this code point.
If the string contains a set of bytes that is not UTF-8, then the behavior is undefined.

## isValidUTF8

Returns 1, if the set of bytes is valid UTF-8 encoded, otherwise 0.

## toValidUTF8

Replaces invalid UTF-8 characters by the `�` (U+FFFD) character. All running in a row invalid characters are collapsed into the one replacement character.

```sql
toValidUTF8( input_string )
```

Parameters:

- input_string — Any set of bytes represented as the [String](../../data_types/string.md) data type object.

Returned value: Valid UTF-8 string.

### Example

```sql
SELECT toValidUTF8('\x61\xF0\x80\x80\x80b')
```
```text
┌─toValidUTF8('a����b')─┐
│ a�b                   │
└───────────────────────┘
```


## reverse

Reverses the string (as a sequence of bytes).

## reverseUTF8

Reverses a sequence of Unicode code points, assuming that the string contains a set of bytes representing a UTF-8 text. Otherwise, it does something else (it doesn't throw an exception).

## format(pattern, s0, s1, ...)

Formatting constant pattern with the string listed in the arguments. `pattern` is a simplified Python format pattern. Format string contains "replacement fields" surrounded by curly braces `{}`. Anything that is not contained in braces is considered literal text, which is copied unchanged to the output. If you need to include a brace character in the literal text, it can be escaped by doubling: `{{` and `}}`. Field names can be numbers (starting from zero) or empty (then they are treated as consequence numbers).

```sql
SELECT format('{1} {0} {1}', 'World', 'Hello')
```
```text
┌─format('{1} {0} {1}', 'World', 'Hello')─┐
│ Hello World Hello                       │
└─────────────────────────────────────────┘
```
```sql
SELECT format('{} {}', 'Hello', 'World')
```
```text
┌─format('{} {}', 'Hello', 'World')─┐
│ Hello World                       │
└───────────────────────────────────┘
```

## concat(s1, s2, ...)

Concatenates the strings listed in the arguments, without a separator.

## concatAssumeInjective(s1, s2, ...)

Same as [concat](./string_functions.md#concat-s1-s2), the difference is that you need to ensure that concat(s1, s2, s3) -> s4 is injective, it will be used for optimization of GROUP BY

## substring(s, offset, length), mid(s, offset, length), substr(s, offset, length)

Returns a substring starting with the byte from the 'offset' index that is 'length' bytes long. Character indexing starts from one (as in standard SQL). The 'offset' and 'length' arguments must be constants.

## substringUTF8(s, offset, length)

The same as 'substring', but for Unicode code points. Works under the assumption that the string contains a set of bytes representing a UTF-8 encoded text. If this assumption is not met, it returns some result (it doesn't throw an exception).

## appendTrailingCharIfAbsent(s, c)

If the 's' string is non-empty and does not contain the 'c' character at the end, it appends the 'c' character to the end.

## convertCharset(s, from, to)

Returns the string 's' that was converted from the encoding in 'from' to the encoding in 'to'.

## base64Encode(s)
Encodes 's' string into base64

## base64Decode(s)
Decode base64-encoded string 's' into original string. In case of failure raises an exception.

## tryBase64Decode(s)
Similar to base64Decode, but in case of error an empty string would be returned.

## endsWith(s, suffix) {#function-endswith}

Returns whether to end with the specified suffix. Returns 1 if the string ends with the specified suffix, otherwise it returns 0.

## startsWith(str, prefix) {#function-startswith}

Returns 1 whether string starts with the specified prefix, otherwise it returns 0.

```sql
SELECT startsWith('Spider-Man', 'Spi');
```

**Returned values**

- 1, if the string starts with the specified prefix.
- 0, if the string doesn't start with the specified prefix.

**Example**

Query:

```sql
SELECT startsWith('Hello, world!', 'He');
```

Result:

```text
┌─startsWith('Hello, world!', 'He')─┐
│                                 1 │
└───────────────────────────────────┘
```

## trimLeft(s)

Returns a string that removes the whitespace characters on left side.

## trimRight(s)

Returns a string that removes the whitespace characters on right side.

## trimBoth(s)

Returns a string that removes the whitespace characters on either side.

## CRC32(s)

Returns the CRC32 checksum of a string, using CRC-32-IEEE 802.3 polynomial and initial value `0xffffffff` (zlib implementation).

The result type is UInt32.

## CRC32IEEE(s)

Returns the CRC32 checksum of a string, using CRC-32-IEEE 802.3 polynomial.

The result type is UInt32.

## CRC64(s)

Returns the CRC64 checksum of a string, using CRC-64-ECMA polynomial.

The result type is UInt64.

[Original article](https://clickhouse.yandex/docs/en/query_language/functions/string_functions/) <!--hide-->
