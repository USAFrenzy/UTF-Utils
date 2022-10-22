<div align="center">
<h1> What Is This? </h1>
</div>
<p align="center">
A simple and small header-only helper for transcoding between UTF-16LE, UTF-32LE, and UTF-8.<br>
This small library was born from the want of adding Unicode support to one of my other projects, Serenity Logger.<br>
</p>

-------------------------------------------------------------------------------------------------------------------------------------------------------------------
<div align="center">
<h1> What Does This Include? </h1>
</div>

-------------------------------------------------------------------------------------------------------------------------------------------------------------------

- The ability to convert encodings to/from
  * UTF-16LE
  * UTF-32LE
  * UTF-8
- Automatically Validate And Replace Invalid Codepoints In Each Of The Above Encodings Using An Appropriate Replacement Character 
- Ability to explicity validate UTF-8 Sequences
  - All of the conversion functions automatically self-validate, however, the explicit validation function is currently only implemented for UTF-8.
- Errors Reported In Debug Builds And Errors Replaced In Release Builds 
  * The whole idea of non-throwing on errors, but handle errors anyways<br>
    via a replacement character codepoint appropriate for the encoding.
- The ability to interchange between strings, string views, or vectors for the source and destination.
- The ability to detect any BOMs that may or may not be present
- All of the above in a rather fast manner; this may not be *THE* quickest library, but this library does do its job very well
  - In tests using SSO inputs, the timings were ~20ns-30ns over 1,000,000 iterations for round trip conversions of each encoding.
  - In tests avoiding SSO inputs, the timings were ~40ns-80ns over 1,000,000 iterations for round trip conversions of each encoding.

-------------------------------------------------------------------------------------------------------------------------------------------------------------------
<div align="center">
<h1> How To Use </h1>
</div>

-------------------------------------------------------------------------------------------------------------------------------------------------------------------
### For each of the below conversion functions, they can take either strings, string views, or vectors of the appropriate type for the source and for the destination paramaters.
-------------------------------------------------------------------------------------------------------------------------------------------------------------------
- ```constexpr void U32ToU16(Source&& sv, Buffer& buff, Pos&& startingPos = 0)```
- ```constexpr void U16ToU32(Source&& sv, Buffer& buff, Pos&& startingPos = 0)```
- ```constexpr void U16ToU8(Source&& sv, Buffer& buff, Pos&& startingPos = 0)```
- ```constexpr void U32ToU8(Source&& sv, Buffer& buff, Pos&& startingPos = 0)```
- ```constexpr void U8ToU32(Source&& sv, Buffer& buff, Pos&& startingPos = 0)```
- ```constexpr void U8ToU16(Source&& sv, Buffer& buff, Pos&& startingPos = 0)```
-------------------------------------------------------------------------------------------------------------------------------------------------------------------
<br>

#### If using U32ToU16(): 
- The available types to use for the source are:
  - ```std::basic_string<char32_t>```/```std::basic_string<wchar_t>```
  - ```std::basic_stringview<char32_t>```/```std::basic_stringview<wchar_t>```
  - ```std::vector<char32_t>```/```std::vector<wchar_t>```
- The available types to use for the destination are:
  - ```std::basic_string<char16_t>```/```std::basic_string<wchar_t>```
  - ```std::vector<char16_t>```/```std::vector<wchar_t>```
#### If using U32ToU8(): 
- The available types to use for the source are:
  - ```std::basic_string<char32_t>```/```std::basic_string<wchar_t>```
  - ```std::basic_stringview<char32_t>```/```std::basic_stringview<wchar_t>```
  - ```std::vector<char32_t>```/```std::vector<wchar_t>```
- The available types to use for the destination are:
  - ```std::basic_string<char>```/```std::basic_string<unsigned char>```
  - ```std::vector<char>```/```std::vector<unsigned char>```
#### If using U16ToU32(): 
- The available types to use for the source are:
  - ```std::basic_string<char16_t>```/```std::basic_string<wchar_t>```
  - ```std::basic_stringview<char16_t>```/```std::basic_stringview<wchar_t>```
  - ```std::vector<char16_t>```/```std::vector<wchar_t>```
- The available types to use for the destination are:
  - ```std::basic_string<char32_t>```/```std::basic_string<wchar_t>```
  - ```std::vector<char32_t>```/```std::vector<wchar_t>```
#### If using U16ToU8(): 
- The available types to use for the source are:
  - ```std::basic_string<char16_t>```/```std::basic_string<wchar_t>```
  - ```std::basic_stringview<char16_t>```/```std::basic_stringview<wchar_t>```
  - ```std::vector<char16_t>```/```std::vector<wchar_t>```
- The available types to use for the destination are:
  - ```std::basic_string<char>```/```std::basic_string<unsigned char>```
  - ```std::vector<char>```/```std::vector<unsigned char>```

#### If using U8ToU16(): 
- The available types to use for the source are:
  - ```std::basic_string<char>```/```std::basic_string<unsigned char>```
  - ```std::basic_stringview<char>```/```std::basic_stringview<unsigned char>```
  - ```std::vector<char>```/```std::vector<unsigned char>```
- The available types to use for the destination are:
  - ```std::basic_string<char16_t>```/```std::basic_string<wchar_t>```
  - ```std::vector<char16_t>```/```std::vector<wchar_t>```

#### If using U8ToU32(): 
- The available types to use for the source are:
  - ```std::basic_string<char>```/```std::basic_string<unsigned char>```
  - ```std::basic_stringview<char>```/```std::basic_stringview<unsigned char>```
  - ```std::vector<char>```/```std::vector<unsigned char>```
- The available types to use for the destination are:
  - ```std::basic_string<char32_t>```/```std::basic_string<wchar_t>```
  - ```std::vector<char32_t>```/```std::vector<wchar_t>```

-------------------------------------------------------------------------------------------------------------------------------------------------------------------
<div align="center">
<h2> Important Notes About The Conversion Functions </h2>
</div>

- The above lists the base valid type, meaning that ```std::u16string```, ```std::u16string_view```, ```std::u32string```, and ```std::u32string_view``` are supported from the get-go.
- Please make note that ```char8_t``` is not supported in this library, however ```char``` and ```unsigned char``` are. This is intentional due to some of the known issues currently around ```char8_t```.
- The usage of ```wchar_t``` is discouraged as it's easy to mix things that shouldn't be used. The usage here is to allow for compatability between systems that use and/or require it.
  - For example, on Windows, the usage of ```wchar_t``` would be a valid destination base char type since it's 2 bytes, but not a valid source char type due to UTF-32 requiring 32 bits.
  - However, on Linux, the opposite would be true since ```wchar_t``` is 4 bytes long, which is too large for UTF-16 codepoints but perfectly fine for UTF-32 codepoints.
- The ```startingPos``` parameter can be omitted, but I have found some helpful use cases for a starting offset, so this paramater is present for those that may find it useful.
- All the above conversion functions follow this reasoning and will issue a compile-time error on invalid types through the usage of concepts and constraints. 

-------------------------------------------------------------------------------------------------------------------------------------------------------------------

<div align="center">
<h1> Other Miscellaneous Functions Available In This Library </h1> 
</div>

-------------------------------------------------------------------------------------------------------------------------------------------------------------------
1) ```bool IsValidU8(Buffer& buff)```
  - This function returns true if the sequence of bytes provided in ```buff``` are all valid, otherwise, this function returns false.
  - This function also checks for a BOM to bypass checking the entire sequence and return early if either UTF-16 or UTF-32 is detected.
    - This function also detects any UTF-8 BOM if present and correctly handles it, in this case, by ignoring the BOM and advancing the sequence position.
- NOTE: If an invalid UTF-8 sequence is detected, then the offending byte sequence is overwritten with a UTF-8 encoded replacement character (```?```).
  - In essence, this function will correct invalid UTF-8 sequences into valid UTF-8 sequences, however, it will 
  <br>NOT preserve the original data if invalid bytes are detected due to the fact that the offending bytes are
  <br>overwritten with a valid replacement character instead. This is intentional so as to avoid ignoring a 
  <br>failed check and using the invalid sequence elsewhere, possibly causing crashes on input.
  - If you need to simply check if a sequence is valid, please create a copy of the sequence and pass that copy into this function.

-------------------------------------------------------------------------------------------------------------------------------------------------------------------
2) ```constexpr bom_type DetectBom(Source&& src)```
  - This function returns one of the following enum values:
    * ```utf8_bom```
    * ```utf8_no_bom```
    * ```utf16LE_bom```
    * ```utf16BE_bom```
    * ```utf16_no_bom```
    * ```utf32LE_bom```
    * ```utf32BE_bom```
    * ```utf32_no_bom```
    
- NOTE: While ```utf32BE_bom``` and ```utf16BE_bom``` are both present as return types, they are currently not used as Big Endian support has yet to be implemented.
-------------------------------------------------------------------------------------------------------------------------------------------------------------------
3) ```constexpr size_t ReserveLengthForU8(StringishType&& s)```
  - This function takes in any supported source type from the lists found in the "How To Use" section.
  - This function simply returns the number of bytes that would be required for encoding a UTF-16 or UTF-32 Sequence into UTF-8.
-------------------------------------------------------------------------------------------------------------------------------------------------------------------

