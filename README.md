# What Is This?
A simple and small header-only helper for transcoding between UTF-16LE, UTF-32LE, and UTF-8.<br>
This small library was born from the want of adding Unicode support to one of my other projects, Serenity Logger.<br>

# What Does This Include?
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

# How To Use

For each of the below conversion functions, they can take either strings, string views, or vectors of the appropriate type for the source and for the destination paramaters.
- ```U32ToU16(Source&& sv, Buffer& buff, Pos&& startingPos = 0)```
- ```U16ToU32(Source&& sv, Buffer& buff, Pos&& startingPos = 0)```
- ```U16ToU8(Source&& sv, Buffer& buff, Pos&& startingPos = 0)```
- ```U32ToU8(Source&& sv, Buffer& buff, Pos&& startingPos = 0)```
- ```U8ToU32(Source&& sv, Buffer& buff, Pos&& startingPos = 0)```
- ```U8ToU16(Source&& sv, Buffer& buff, Pos&& startingPos = 0)```
<br>

That is to say, if using U32ToU16() as an example: 
- The available types to use for the source are:
  - ```std::basic_string<char32_t>```/```std::basic_string<wchar_t>```
  - ```std::basic_stringview<char32_t>```/```std::basic_stringview<wchar_t>```
  - ```std::vector<char32_t>```/```std::vector<wchar_t>```
- The available types to use for the destination are:
  - ```std::basic_string<char16_t>```/```std::basic_string<wchar_t>```
  - ```std::vector<char16_t>```/```std::vector<wchar_t>```
- NOTE: The above lists the base valid type, meaning that ```std::u16string```, ```std::u16string_view```, ```std::u32string```, and ```std::u32string_view``` are supported from the get-go.
- NOTE: the usage of ```wchar_t``` is discouraged as it's easy to mix things that shouldn't be used. The usage here is to allow for compatability between systems that use and/or require it.
  - For example, on Windows, the usage of ```wchar_t``` would be a valid destination base char type since it's 2 bytes, but not a valid source char type due to UTF-32 requiring 32 bits.<br>However, on Linux, the opposite would be true since ```wchar_t``` is 4 bytes long, which is too large for UTF-16 codepoints but perfectly fine for UTF-32 codepoints.
- NOTE: The ```startingPos``` parameter can be omitted, but I have found some helpful use cases for a starting offset, so this paramater is present for those that may find it useful.
- All the above conversion functions follow this reasoning and will issue a compile-time error on invalid types through the usage of concepts and constraints.    
  
