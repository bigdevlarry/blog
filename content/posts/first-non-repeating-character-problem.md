---
title: First Non-Repeating Character
date: 2024-10-17
description: "What is the most efficient way to find the first character in a string that appears only once in both JavaScript and PHP?"
tags:
   - algorithm
---



### Problem statement 🤔

##### Category - Easy 🟩

Given a string containing only English alphabet characters, the task is to find the first character in the string that does not repeat (i.e., it occurs exactly once). 
The function should return the index of this first non-repeating character. If no such character exists (i.e., if all characters repeat), the function should return `-1`.


##### Sample input 1
```
string = "aabccbcd"
```
##### Sample output
```
7 // The first non-repeating character is "d" and it is found at index 7
```

##### Sample input 2
```
string = "aabccbc"
```
##### Sample output
```
-1 // All characters repeat
```

### Illustration & Pseudo code
<img src="/blog/images/string/first-non-repeating-character.png">

- Approach the problem using 2 loops
- Create an empty hash table to store the character counts
- First loop: Loop through the input string and for each character in the string, increment its count and populate the hash table
- Second loop: Loop through the input string again. For each character, check its count in the hash table. If the count is equal to `1`, return the index of that character.



#### Solution in JS

```javascript
function firstNonRepeatingCharacter(string) {
    
  let charCount = {};

  // Loop through the string using for-of 
  for (const char of string) {
      // Increment the character count if the character already exists, otherwise initialize it to 1
      charCount[char] = charCount[char] ? charCount[char] + 1 : 1;
  }
  
  // Loop through the string to find the first non-repeating character
  for (i = 0; i < string.length; i++) {
      if (charCount[string[i]] === 1) return i; // Return the index of the first non-repeating character
  }
  
  return -1; // Return -1 if no non-repeating character is found
}

result = firstNonRepeatingCharacter('aabccbcd');

console.log(result) // 7 
```

#### Solution in PHP

```php
function firstNonRepeatingCharacter($string) {
    
  $charCount = [];
  
  // Count character occurrences using foreach
  for ($i = 0; $i < strlen($string); $i++) {
    $char = $string[$i];
    
    // Increment the character count if the character already exists, otherwise initialize it to 1
    $charCount[$char] = isset($charCount[$char]) ? $charCount[$char] + 1 : 1;
  }
    
  // Loop through the string to find the first non-repeating character
  for ($i = 0; i < strlen($string); $i++) {
    if ($charCount[$string[$i]] === 1) return $i; // Return the index of the first non-repeating character
  }
  
  return -1; // Return -1 if no non-repeating character is found
}

$result = firstNonRepeatingCharacter('aabccbcd');

echo $result; // 7 
```

#### Time and Space Complexity Analysis
Time - The algorithm will run in O(n) time, where n is the length of the input string.

Space - The constant space O(1) requirement arises from the fact that the input string consists solely of lowercase English alphabet letters (26), 
meaning our hash table will contain at most 26 character frequencies.

