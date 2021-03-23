# Sliding Window

```javascript
// usually finding a max or min in a contiguous array or string
// running time O(N) | space O(1)
function basic_sliding_window(k, arr) {
  let maxSum = 0, // max or min usually
    windowSum = 0, // running window sum, min ,length, etc
    windowStart = 0;

  for (let window_end = 0; window_end < arr.length; window_end++) {
    windowSum += arr[window_end]; // add the next element
    // slide the window, we don't need to slide if we've not hit the required window size of 'k'
    if (window_end >= k - 1) {
      maxSum = Math.max(maxSum, windowSum);
      windowSum -= arr[windowStart]; // subtract the element going out
      windowStart += 1; // slide the window ahead
    }
  }
  return maxSum;
}
```

---

## Maximum Sum Subarray of Size K (easy)

```
Given an array of positive numbers and a positive number ‘k,’ find the maximum sum of any contiguous subarray of size ‘k’.

Example 1:

Input: [2, 1, 5, 1, 3, 2], k=3
Output: 9
Explanation: Subarray with maximum sum is [5, 1, 3].
Example 2:

Input: [2, 3, 4, 1, 5], k=2
Output: 7
Explanation: Subarray with maximum sum is [3, 4].
```

```javascript
// running time: O(N), space O(1)
function max_sub_array_of_size_k(k, arr) {
  let maxSum = 0,
    windowSum = 0,
    windowStart = 0;

  for (window_end = 0; window_end < arr.length; window_end++) {
    windowSum += arr[window_end]; // add the next element
    // slide the window, we don't need to slide if we've not hit the required window size of 'k'
    if (window_end >= k - 1) {
      maxSum = Math.max(maxSum, windowSum);
      windowSum -= arr[windowStart]; // subtract the element going out
      windowStart += 1; // slide the window ahead
    }
  }
  return maxSum;
}
```

## Smallest Subarray with a given sum (easy)

```
Given an array of positive numbers and a positive number ‘S,’ find the length of the smallest contiguous subarray whose sum is greater than or equal to ‘S’. Return 0 if no such subarray exists.

Example 1:

Input: [2, 1, 5, 2, 3, 2], S=7
Output: 2
Explanation: The smallest subarray with a sum greater than or equal to '7' is [5, 2].

Input: [2, 1, 5, 2, 8], S=7
Output: 1
Explanation: The smallest subarray with a sum greater than or equal to '7' is [8].
```

```javascript
// running time: O(N), space O(1)
function smallest_subarray_with_given_sum(s, arr) {
  let windowSum = 0,
    minLength = Infinity,
    windowStart = 0;

  for (windowEnd = 0; windowEnd < arr.length; windowEnd++) {
    windowSum += arr[windowEnd]; // add the next element
    // shrink the window as small as possible until the 'window_sum' is smaller than 's'
    while (windowSum >= s) {
      minLength = Math.min(minLength, windowEnd - windowStart + 1);
      windowSum -= arr[windowStart];
      windowStart += 1;
    }
  }

  if (minLength === Infinity) {
    return 0;
  }
  return minLength;
}
```

# Longest Substring with K Distinct Characters (medium)

```
Given a string, find the length of the longest substring in it with no more than K distinct characters.

Example 1:

Input: String="araaci", K=2
Output: 4
Explanation: The longest substring with no more than '2' distinct characters is "araa".
Example 2:

Input: String="araaci", K=1
Output: 2
Explanation: The longest substring with no more than '1' distinct characters is "aa".
```

```javascript
// running time: O(N), space O(1)  since 26 chars in alphabet  can also switch out to fixed array
function longest_substring_with_k_distinct(str, k) {
  let windowStart = 0,
    maxLength = 0,
    charFrequency = {};

  // in the following loop we'll try to extend the range [window_start, window_end]
  for (let windowEnd = 0; windowEnd < str.length; windowEnd++) {
    const rightChar = str[windowEnd];
    if (!(rightChar in charFrequency)) {
      charFrequency[rightChar] = 0;
    }
    charFrequency[rightChar] += 1;
    // shrink the sliding window, until we are left with 'k' distinct characters in the char_frequency
    while (Object.keys(charFrequency).length > k) {
      const leftChar = str[windowStart];
      charFrequency[leftChar] -= 1;
      if (charFrequency[leftChar] === 0) {
        delete charFrequency[leftChar];
      }
      windowStart += 1; // shrink the window
    }
    // remember the maximum length so far
    maxLength = Math.max(maxLength, windowEnd - windowStart + 1);
  }

  return maxLength;
}
```

# Fruits into Baskets (medium)

```
Given an array of characters where each character represents a fruit tree, you are given two baskets, and your goal is to put maximum number of fruits in each basket. The only restriction is that each basket can have only one type of fruit.

You can start with any tree, but you can’t skip a tree once you have started. You will pick one fruit from each tree until you cannot, i.e., you will stop when you have to pick from a third fruit type.

Write a function to return the maximum number of fruits in both baskets.

Example 1:

Input: Fruit=['A', 'B', 'C', 'A', 'C']
Output: 3
Explanation: We can put 2 'C' in one basket and one 'A' in the other from the subarray ['C', 'A', 'C']
Example 2:

Input: Fruit=['A', 'B', 'C', 'B', 'B', 'C']
Output: 5
Explanation: We can put 3 'B' in one basket and two 'C' in the other basket.
This can be done if we start with the second letter: ['B', 'C', 'B', 'B', 'C']
```

```javascript
// running time: O(N), space O(1)  since max 3 of fruits
function fruits_into_baskets(fruits) {
  let windowStart = 0,
    maxLength = 0,
    fruitFrequency = {};

  // try to extend the range [windowStart, windowEnd]
  for (let windowEnd = 0; windowEnd < fruits.length; windowEnd++) {
    const rightFruit = fruits[windowEnd];
    if (!(rightFruit in fruitFrequency)) {
      fruitFrequency[rightFruit] = 0;
    }
    fruitFrequency[rightFruit] += 1;

    // shrink the sliding window, until we are left with '2' fruits in the fruit frequency dictionary
    while (Object.keys(fruitFrequency).length > 2) {
      const leftFruit = fruits[windowStart];
      fruitFrequency[leftFruit] -= 1;
      if (fruitFrequency[leftFruit] === 0) {
        delete fruitFrequency[leftFruit];
      }
      windowStart += 1; // shrink the window
    }
    maxLength = Math.max(maxLength, windowEnd - windowStart + 1);
  }

  return maxLength;
}
```

# No-repeat Substring (hard)

```
Problem Statement #
Given a string, find the length of the longest substring, which has no repeating characters.

Example 1:

Input: String="aabccbb"
Output: 3
Explanation: The longest substring without any repeating characters is "abc".
Example 2:

Input: String="abbbb"
Output: 2
Explanation: The longest substring without any repeating characters is "ab".
```

```javascript
// running time: O(N), space O(1)  since 26 chars in alphabet  can also switch out to fixed array
function non_repeat_substring(str) {
  let windowStart = 0,
    maxLength = 0,
    charIndexMap = {};

  // try to extend the range [windowStart, windowEnd]
  for (let windowEnd = 0; windowEnd < str.length; windowEnd++) {
    const rightChar = str[windowEnd];
    // if the map already contains the 'rightChar', shrink the window from the beginning so that
    // we have only one occurrence of 'rightChar'
    if (rightChar in charIndexMap) {
      // this is tricky; in the current window, we will not have any 'rightChar' after its previous index
      // and if 'windowStart' is already ahead of the last index of 'rightChar', we'll keep 'windowStart'
      windowStart = Math.max(windowStart, charIndexMap[rightChar] + 1);
    }
    // insert the 'rightChar' into the map
    charIndexMap[rightChar] = windowEnd;
    // remember the maximum length so far
    maxLength = Math.max(maxLength, windowEnd - windowStart + 1);
  }
  return maxLength;
}
```

# Longest Substring with Same Letters after Replacement (hard)

```
Given a string with lowercase letters only, if you are allowed to replace no more than ‘k’ letters with any letter, find the length of the longest substring having the same letters after replacement.

Example 1:

Input: String="aabccbb", k=2
Output: 5
Explanation: Replace the two 'c' with 'b' to have a longest repeating substring "bbbbb".
Example 2:

Input: String="abbcb", k=1
Output: 4
Explanation: Replace the 'c' with 'b' to have a longest repeating substring "bbbb".
```

```javascript
//running time O(n) and space O(1)
function length_of_longest_substring(str, k) {
  let windowStart = 0,
    maxLength = 0,
    maxRepeatLetterCount = 0,
    frequencyMap = {};

  // Try to extend the range [windowStart, windowEnd]
  for (let windowEnd = 0; windowEnd < str.length; windowEnd++) {
    const rightChar = str[windowEnd];
    if (!(rightChar in frequencyMap)) {
      frequencyMap[rightChar] = 0;
    }
    frequencyMap[rightChar] += 1;
    maxRepeatLetterCount = Math.max(
      maxRepeatLetterCount,
      frequencyMap[rightChar]
    );

    // Current window size is from windowStart to windowEnd, overall we have a letter which is
    // repeating 'maxRepeatLetterCount' times, this means we can have a window which has one letter
    // repeating 'maxRepeatLetterCount' times and the remaining letters we should replace.
    // if the remaining letters are more than 'k', it is the time to shrink the window as we
    // are not allowed to replace more than 'k' letters
    if (windowEnd - windowStart + 1 - maxRepeatLetterCount > k) {
      leftChar = str[windowStart];
      frequencyMap[leftChar] -= 1;
      windowStart += 1;
    }

    maxLength = Math.max(maxLength, windowEnd - windowStart + 1);
  }
  return maxLength;
}
```

# Longest Subarray with Ones after Replacement (hard)

```
Given an array containing 0s and 1s, if you are allowed to replace no more than ‘k’ 0s with 1s, find the length of the longest contiguous subarray having all 1s.

Example 1:

Input: Array=[0, 1, 1, 0, 0, 0, 1, 1, 0, 1, 1], k=2
Output: 6
Explanation: Replace the '0' at index 5 and 8 to have the longest contiguous subarray of 1s having length 6.
Example 2:

Input: Array=[0, 1, 0, 0, 1, 1, 0, 1, 1, 0, 0, 1, 1], k=3
Output: 9
Explanation: Replace the '0' at index 6, 9, and 10 to have the longest contiguous subarray of 1s having length 9.
```

```javascript
// O(N) and space O(1)
function length_of_longest_substring(arr, k) {
  let windowStart = 0,
    maxLength = 0,
    maxOnesCount = 0;

  // Try to extend the range [windowStart, windowEnd]
  for (windowEnd = 0; windowEnd < arr.length; windowEnd++) {
    if (arr[windowEnd] === 1) {
      maxOnesCount += 1;
    }

    // Current window size is from windowStart to windowEnd, overall we have a maximum of 1s
    // repeating 'maxOnesCount' times, this means we can have a window with 'maxOnesCount' 1s
    // and the remaining are 0s which should replace with 1s.
    // now, if the remaining 0s are more than 'k', it is the time to shrink the window as we
    // are not allowed to replace more than 'k' 0s
    if (windowEnd - windowStart + 1 - maxOnesCount > k) {
      if (arr[windowStart] === 1) {
        maxOnesCount -= 1;
      }
      windowStart += 1;
    }

    maxLength = Math.max(maxLength, windowEnd - windowStart + 1);
  }
  return maxLength;
}
```

# Permutation in a String (hard)

```
Given a string and a pattern, find out if the string contains any permutation of the pattern.

Permutation is defined as the re-arranging of the characters of the string. For example, “abc” has the following six permutations:

abc
acb
bac
bca
cab
cba
If a string has ‘n’ distinct characters, it will have n!n! permutations.

Example 1:

Input: String="oidbcaf", Pattern="abc"
Output: true
Explanation: The string contains "bca" which is a permutation of the given pattern.

Input: String="odicf", Pattern="dc"
Output: false
Explanation: No permutation of the pattern is present in the given string as a substring.
```

```javascript
//time O(N+M) and space O(M) m number of chars
function find_permutation(str, pattern) {
  let windowStart = 0,
    matched = 0,
    charFrequency = {};

  for (i = 0; i < pattern.length; i++) {
    const chr = pattern[i];
    if (!(chr in charFrequency)) {
      charFrequency[chr] = 0;
    }
    charFrequency[chr] += 1;
  }

  // Our goal is to match all the characters from the 'charFrequency' with the current window
  // try to extend the range [windowStart, windowEnd]
  for (windowEnd = 0; windowEnd < str.length; windowEnd++) {
    const rightChar = str[windowEnd];
    if (rightChar in charFrequency) {
      // Decrement the frequency of matched character
      charFrequency[rightChar] -= 1;
      if (charFrequency[rightChar] === 0) {
        matched += 1;
      }
    }

    if (matched === Object.keys(charFrequency).length) {
      return true;
    }

    // Shrink the sliding window
    if (windowEnd >= pattern.length - 1) {
      leftChar = str[windowStart];
      windowStart += 1;
      if (leftChar in charFrequency) {
        if (charFrequency[leftChar] === 0) {
          matched -= 1;
        }
        charFrequency[leftChar] += 1;
      }
    }
  }
  return false;
}
```

# String Anagrams (hard)

```
Given a string and a pattern, find all anagrams of the pattern in the given string.

Anagram is actually a Permutation of a string. For example, “abc” has the following six anagrams:

abc
acb
bac
bca
cab
cba
Write a function to return a list of starting indices of the anagrams of the pattern in the given string.

Example 1:

Input: String="ppqp", Pattern="pq"
Output: [1, 2]
Explanation: The two anagrams of the pattern in the given string are "pq" and "qp".
Example 2:

Input: String="abbcabc", Pattern="abc"
Output: [2, 3, 4]
Explanation: The three anagrams of the pattern in the given string are "bca", "cab", and "abc".
```

```javascript
// running time  O(N + M) and space O(N)
function find_string_anagrams(str, pattern) {
  let windowStart = 0,
    matched = 0,
    charFrequency = {};

  for (i = 0; i < pattern.length; i++) {
    const chr = pattern[i];
    if (!(chr in charFrequency)) {
      charFrequency[chr] = 0;
    }
    charFrequency[chr] += 1;
  }

  const resultIndices = [];
  // our goal is to match all the characters from the 'charFrequency' with the current window
  // try to extend the range [windowStart, windowEnd]
  for (windowEnd = 0; windowEnd < str.length; windowEnd++) {
    const rightChar = str[windowEnd];
    if (rightChar in charFrequency) {
      // decrement the frequency of matched character
      charFrequency[rightChar] -= 1;
      if (charFrequency[rightChar] === 0) {
        matched += 1;
      }
    }

    if (matched === Object.keys(charFrequency).length) {
      // have we found an anagram?
      resultIndices.push(windowStart);
    }

    // shrink the sliding window
    if (windowEnd >= pattern.length - 1) {
      leftChar = str[windowStart];
      windowStart += 1;
      if (leftChar in charFrequency) {
        if (charFrequency[leftChar] === 0) {
          matched -= 1; // before putting the character back, decrement the matched count
        }
        charFrequency[leftChar] += 1; // put the character back
      }
    }
  }

  return resultIndices;
}
```

# Smallest Window containing Substring (hard)

```
Given a string and a pattern, find the smallest substring in the given string which has all the characters of the given pattern.

Example 1:

Input: String="aabdec", Pattern="abc"
Output: "abdec"
Explanation: The smallest substring having all characters of the pattern is "abdec"
Example 2:

Input: String="abdbca", Pattern="abc"
Output: "bca"
Explanation: The smallest substring having all characters of the pattern is "bca".
Example 3:

Input: String="adcad", Pattern="abc"
Output: ""
Explanation: No substring in the given string has all characters of the pattern.
```

```javascript
// running time O(N + M) and space O(N)
function find_substring(str, pattern) {
  let windowStart = 0,
    matched = 0,
    substrStart = 0,
    minLength = str.length + 1,
    charFrequency = {};

  for (i = 0; i < pattern.length; i++) {
    const chr = pattern[i];
    if (!(chr in charFrequency)) {
      charFrequency[chr] = 0;
    }
    charFrequency[chr] += 1;
  }

  // try to extend the range [windowStart, windowEnd]
  for (windowEnd = 0; windowEnd < str.length; windowEnd++) {
    const rightChar = str[windowEnd];
    if (rightChar in charFrequency) {
      charFrequency[rightChar] -= 1;
      if (charFrequency[rightChar] >= 0) {
        // Count every matching of a character
        matched += 1;
      }
    }

    // Shrink the window if we can, finish as soon as we remove a matched character
    while (matched === pattern.length) {
      if (minLength > windowEnd - windowStart + 1) {
        minLength = windowEnd - windowStart + 1;
        substrStart = windowStart;
      }

      const leftChar = str[windowStart];
      windowStart += 1;
      if (leftChar in charFrequency) {
        // Note that we could have redundant matching characters, therefore we'll decrement the
        // matched count only when a useful occurrence of a matched character is going out of the window
        if (charFrequency[leftChar] === 0) {
          matched -= 1;
        }
        charFrequency[leftChar] += 1;
      }
    }
  }

  if (minLength > str.length) {
    return "";
  }
  return str.substring(substrStart, substrStart + minLength);
}
```

# Words Concatenation (hard)

```
Given a string and a list of words, find all the starting indices of substrings in the given string that are a concatenation of all the given words exactly once without any overlapping of words. It is given that all words are of the same length.

Example 1:

Input: String="catfoxcat", Words=["cat", "fox"]
Output: [0, 3]
Explanation: The two substring containing both the words are "catfox" & "foxcat".
Example 2:

Input: String="catcatfoxfox", Words=["cat", "fox"]
Output: [3]
Explanation: The only substring containing both the words is "catfox".
```

```javascript
// O(N∗M∗Len) and space O(M+N)
function find_word_concatenation(str, words) {
  if (words.length === 0 || words[0].length === 0) {
    return [];
  }

  wordFrequency = {};

  words.forEach((word) => {
    if (!(word in wordFrequency)) {
      wordFrequency[word] = 0;
    }
    wordFrequency[word] += 1;
  });

  const resultIndices = [],
    wordsCount = words.length;
  wordLength = words[0].length;

  for (i = 0; i < str.length - wordsCount * wordLength + 1; i++) {
    const wordsSeen = {};
    for (j = 0; j < wordsCount; j++) {
      next_word_index = i + j * wordLength;
      // Get the next word from the string
      word = str.substring(next_word_index, next_word_index + wordLength);
      if (!(word in wordFrequency)) {
        // Break if we don't need this word
        break;
      }

      // Add the word to the 'wordsSeen' map
      if (!(word in wordsSeen)) {
        wordsSeen[word] = 0;
      }
      wordsSeen[word] += 1;

      // no need to process further if the word has higher frequency than required
      if (wordsSeen[word] > (wordFrequency[word] || 0)) {
        break;
      }

      if (j + 1 === wordsCount) {
        // Store index if we have found all the words
        resultIndices.push(i);
      }
    }
  }

  return resultIndices;
}
```
