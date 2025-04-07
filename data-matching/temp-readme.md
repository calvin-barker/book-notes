### 3.2 Issues with Names and Other Personal Information

*How extensively can we use thinkbabynames.com and similar databases for name variations in our identity graph?*

Alt: BehindTheName, NameAPI, census-based name tables

Things they can be used for:
1. Name normalization (might have some nicknames, diminutives, and variants that can map to a canonical form)
2. Cross-cultural name variants (Juan, Giovanni, Ivan -> John)
3. Gender normalization
4. Fuzzy matching supplemental support/source for phonetic match rules

*How often do people inaccurately write their address ("brooklyn" vs "new york city" vs "new york")*

The USPS mostly relies on ZIP to deal with it. It's complex, but using the correct borough or neighborhood name is generally recommended to ensure timely and accurate delivery.

Seems that ZIP is the ideal.

### 3.3 Types and Sources of Variations and Errors in Names

*If I could only match on hashed values of names, how can I deal with the typographical errors that don't affect phonetical structure of a name? (meyer vs meier; christina vs kristina)*

- Changing the pre-hashing strategy (like Soundex)
- pre-hashing canonicalization
- approximate hashing (SimHash, MinHash, Bloom Filters)
- Rainbow tables (mapping variant to Soundex)

*Is there a computationally feasible way to match on edit distance=1 errors with the hashed values?*

ChatGPT says not with SHA-256 or MD5.

