## 1 Two Sum

```javascript
/*
Template: 
  // Running Time: O(n^2), Space O(1) - Brute force
  // Running Time: O(n), Space O(n) - Hash Table
*/
// Running Time: O(n^2), Space O(1) - Brute force
// Running Time: O(n), Space O(n) - Hash Table
var twoSum = function (nums, target) {
  let dict = new Map();
  if (nums.length <= 1) return [];
  for (let i = 0; i < nums.length; i++) {
    let difference = target - nums[i];
    if (dict.has(nums[i])) {
      return [dict.get(nums[i]), i];
    }
    dict.set(difference, i);
  }
  return [];
};
```

## 2 Two sum II – INPUT ARRAY IS SORTED

```javascript
// Running Time: O(nlogn), Space O(1) - Binary Search
// Running Time: O(n), Space O(1) - Two Pointers
var twoSum = function (numbers, target) {
  let left = 0,
    right = numbers.length - 1;
  while (left < right) {
    if (numbers[left] + numbers[right] < target) {
      left++;
    } else if (numbers[left] + numbers[right] > target) {
      right--;
    } else {
      return [left + 1, right + 1];
    }
  }
  return [];
};
```

## 3 Two sum III – DATA STRUCTURE DESIGN

```javascript
// Running Time: add:O(n) - Find- O(1), Space O(n^2) - Store pair sums in hash table
// Running Time: add: O(nlogn), Find O(n), Space O(n) - Stored Array - Binary Search + Two Pointers
// Running Time: add: O(n), Find O(n), Space O(n) - Hash table
class TwoSum {
  constructor() {
    this.dict = new map();
  }
  add(val) {
    this.dict[val] = this.dict[val] ? this.dict[val] + 1 : 1;
  }
  find(target) {
    for (let [key, value] of map) {
      let difference = target - key;
      if (difference === key && value >= 2) {
        return true;
      } else if (dict.has(difference)) {
        return true;
      }
    }
    return false;
  }
}
```

## 4 VALID PALINDROME

```javascript
// Running Time: O(n), Space O(n) - Reverse
// Running Time: O(n), Space O(1) - Two Pointers
var isPalindrome = function (s) {
  let clean = s.replace(/[^a-z0-9]/gi, "").toLowerCase();
  let left = 0;
  let right = clean.length - 1;
  while (left < right) {
    if (clean[left] !== clean[right]) {
      return false;
    }
    left++;
    right--;
  }

  return true;
  //OR return clean === clean.reverse();
};
```

## 5 IMPLEMENT STRSTR()

```javascript
// Running Time: O(nm), Space O(1) - Brute Force
var strStr = function (haystack, needle) {
  if (needle == "") return 0;
  if (needle.length > haystack.length) return -1;
  if (haystack.length < 1) return -1;
  let needleI = 0;
  for (let i = 0; i < haystack.length; i++) {
    if (haystack[i] != needle[needleI]) {
      i = i - needleI;
      needleI = 0;
    } else {
      if (needleX === needle.length - 1) return i - needleI;
      needleI++;
    }
  }
  return -1;
};
```

## 6 REVERSE WORDS IN A STRING

```javascript
// Running Time: O(n), Space O(n) - lots of cuntions
// Running Time: O(n), Space O(n) - split and push, join
var reverseWords = function (s) {
  let words = s.split(" ");
  let answer = [];
  for (let i = words.length - 1; i >= 0; i--) {
    if (words[i] != "") {
      answer.push(words[i]);
    }
  }
  return answer.join(" ");
};

var reverseWords = function (s) {
  return str.split(" ").filter(Boolean).reverse().join(" ");
};

var reverseWords = function (s) {
  //TBD can you does this in O(1) in js?
};
```

## 7 REVERSE WORDS IN A STRING II

```javascript
// Running Time: O(n), Space O(1) - In place assuming you have an array of chars
function reverse(s, begin, end) {
  for (let i = 0; i < (end - begin) / 2; i++) {
    let temp = s[begin + i];
    s[begin + i] = s[end - i - 1];
    s[end - i - 1] = temp;
  }
}
/*
@param [char] s
*/
var reverseWords = function (s) {
  //TBD can you does this in O(1) in js? with a string?
  reverse(s, 0, s.length);
  for (let left = 0, end = 0; end <= s.length; end++) {
    if (end === s.length || s[end] === "") {
      reverse(s, left, end);
      left = end + 1;
    }
  }
};
```

## 8 STRING TO INTEGER (ATOI)

```javascript
// Running Time: O(n), Space O(1)
var myAtoi = function (s) {
  const string = s.trim();
  let sign = "+";
  let start = 0;
  if (string[0] === "-" || string[0] === "+") {
    sign = string[0];
    start++;
  }

  if (string.length - start < 1) {
    return 0;
  }
  let zero = "0".charCodeAt(0);
  let nine = "9".charCodeAt(0);

  if (string.charCodeAt(start) < zero || string.charCodeAt(start) > nine)
    return 0;

  let result = 0;
  let curr = start;

  let currNumCharCode = string[curr].charCodeAt(0);
  const OVERFLOW_MAX = Math.pow(2, 31);

  while (
    curr < string.length &&
    currNumCharCode >= zero &&
    currNumCharCode <= nine
  ) {
    result = 10 * result + Number(string[curr++]);

    if (result >= OVERFLOW_MAX) {
      if (sign === "-") {
        return Number(-+OVERFLOW_MAX);
      }
      return OVERFLOW_MAX - 1;
    }

    if (curr < string.length) {
      currNumCharCode = string[curr].charCodeAt(0);
    }
  }

  return Number(sign + result);
};
```

## 9 VALID NUMBER

```javascript

```

## 10 LONGEST SUBSTRING WITHOUT REPEATING CHARACTERS

```javascript
// Running Time: O(n), Space O(1) - Two iterations
// Running Time: O(n), Space O(N) - Single iteration  (could use a static array and be O(1) )
var lengthOfLongestSubstring = function (s) {
  let max = 0;
  let index = {};
  let right = 0;
  for (let left = 0; right < s.length; right++) {
    left = Math.max(index[s[right]] || 0, left);
    max = Math.max(max, right - left + 1);
    index[s[right]] = right + 1;
  }
};
```

## 11 LONGEST SUBSTRING WITH AT MOST TWO DISTINCT CHARACTERS

```javascript
// Running Time: O(n^3), Space O(n) - brute force with set
// Running Time: O(n^2), Space O(n) - brute force with set reusing set when adding a char to form a new substring
// Running Time: O(n), Space O(n) -
// can switch k for another value for changing from 2 unique to k unique
var lengthOfLongestSubstring = function (s) {
  if (s === null || s.length === 0) return 0;
  let count = new Map();

  let max = 0;
  let left = 0;
  let k = 2;
  for (let right = 0; right < s.length; right++) {
    let value = count.get(s[right]);
    count.set(s[right], value ? value + 1 : 1);
    while (count.size > k) {
      count.set(s[left], count.get(s[left]) - 1);
      if (count.get(s[left]) === 0) {
        count.delete(s[left]);
      }
      left++;
    }
    max = Math.max(max, right - left + 1);
  }
  return max;
};
```

## 12 MISSING RANGES

```javascript
// Running Time: O(n), Space O(n)
function getRange(from, to) {
  return from === to ? `${from}` : `${from} -> ${to}`;
}
var findMissingRanges = function (vals, start, end) {
  let ranges = [];
  let prev = start - 1;
  for (let i = 0; i <= vals.length; i++) {
    let curr = i === vals.length ? end + 1 : vals[i];
    if (curr - prev >= 2) {
      ranges.push(getRange(prev + 1, curr - 1));
    }
    prev = curr;
  }

  return ranges;
};
```

## 13 LONGEST PALINDROMIC SUBSTRING

```javascript

```

## 14 ONE EDIT DISTANCE

```javascript
var isOneEditDistance = function(s, t) {
    if (s === t || Math.abs(s.length - t.length) > 1) return false;
    let i = 0;
    let j = 0;
    let isEdited = false;
    while (i < s.length && j < t.length) {
        if (s[i] === t[j]) {
            i++;
            j++;
        } else if (isEdited) {
            return false;
        } else if (s.length < t.length) {  // insert into s or delete from t
            j++;
            isEdited = true;
        } else if (s.length > t.length) {  // insert into t or delete form s
            i++;
            isEdited = true;
        } else {  // edit s or t
            i++;
            j++;
            isEdited = true;
        }
    }
    return true;

var isOneEditDistance = function(s, t) {
     if (s === t) return false;
       let sLength = s.length;
       let tLength = t.length;
       if( tLength > sLength){
           return this.isOneEditDistance(t,s);
       }
       if( sLength - tLength > 1){
           return false;
       }

        let tI = 0;
        let cost =0;
        let sI=0
        while(sI < sLength || tI < tLength){
            if(s[sI] !== t[tI]){
                if(sLength > tLength){ // simulate delete
                    cost++;
                    sI++;
                }
                else if( sLength < tLength){ // simulate insert
                    cost++;
                    sI++;
                }
                else {
                    sI++;
                    tI++;
                    cost++;
                }

            }
            else {
                sI++;
                tI++;
            }


            if(cost > 1){
                return false;
            }
        }
    if( cost === 0 ) return false;
    return true;
};
```

## 15 READ N CHARACTERS GIVEN READ4

```javascript

```

## 16 READ N CHARACTERS GIVEN READ4 – CALL MULTIPLE TIMES

```javascript

```

## 20 Merge Two Sorted Lists

```javascript
// Running Time: O(n), Space O(n) - create new nodes
// Running Time: O(n), Space O(1) - in place
var mergeTwoLists = function (l1, l2) {
  let newList = new ListNode(0);
  let head = newList;
  while (l1 != null && l2 != null) {
    if (l1.val <= l2.val) {
      newList.next = l1;
      l1 = l1.next;
    } else {
      newList.next = l2;
      l2 = l2.next;
    }
    newList = newList.next;
  }

  if (l1 != null) {
    newList.next = l1;
  }
  if (l2 != null) {
    newList.next = l2;
  }
  return head.next;
};
```

## 21. ADD TWO NUMBERS

```javascript
// Running Time: O(n), Space O(n)
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var addTwoNumbers = function (l1, l2) {
  let head = new ListNode(0);
  let temp = head;
  let overflow = 0;
  //iterate until one of them is null
  while (l1 !== null || l2 !== null) {
    let x = l1 != null ? l1.val : 0;
    let y = l2 != null ? l2.val : 0;
    let total = x + y + overflow;
    // while iterating add them and keep an overflow

    // x%10
    let currentValue = total % 10;
    // x/10
    overflow = Math.floor(total / 10);
    temp.next = new ListNode(currentValue);

    temp = temp.next;
    if (l1 != null) l1 = l1.next;
    if (l2 != null) l2 = l2.next;
  }

  if (overflow > 0) {
    temp.next = new ListNode(overflow);
  }

  return head.next;
};
```

## 22. SWAP NODES IN PAIRS

```javascript
// Running Time: O(n), Space O(1) - in place
var swapPairs = function (head) {
  if (!head) return null;
  if (!head.next) return head;

  let prev = null;
  const start = head.next;

  while (head && head.next) {
    const next = head.next;
    const secondNext = head.next.next;

    head.next = secondNext;
    next.next = head;

    if (prev) prev.next = next;

    prev = head;
    head = secondNext;
  }

  return start;
};
```

## 23. MERGE K SORTED LINKED LISTS (HARD)

```javascript

```

## 24. COPY LIST WITH RANDOM POINTER

```javascript

```

# 25 Validate Binary Search Tree

```javascript
// O(N) and O(N)
var validBST = function (root) {
  const validateBSTRecursive = function (
    root,
    min = -Infinity,
    max = Infinity
  ) {
    if (root === null) return true;
    if (root.val > min && root.val < max) {
      return (
        validateBSTRecursive(root.left, min, root.val) &&
        validateBSTRecursive(root.right, root.val, max)
      );
    }

    return false;
  };
  return validateBSTRecursive(root);
};
```

# 26. Maximum Depth of Binary tree

```javascript
// O(N) and O(H) where h is size of level with bfs
// O(N) with space O(logn)DFS
var depthOfBinaryTree = function(root){


      if(root === null) return 0;
      return Math.max(calculateDepth(root.left), calculateDepth(root.right))+1;}
};

```

# 27. Min Depth of binary Tree

```javascript
//DFS run time: O(n), space: (logn)
var minDepthofBinaryTree = function (root) {
  if (root === null) return 0;
  if (root.left === null) return minDepthofBinaryTree(root.right) + 1;
  if (root.right === null) return minDepthofBinaryTree(root.left) + 1;
  return (
    Math.min(
      minDepthofBinaryTree(root.left),
      minDepthofBinaryTree(root.right)
    ) + 1
  );
};
```

# 28 Balanced Binary Tree

```javascript

```

# 39. Min Stack

# 40. Evaluate Reverse Polish Notation

```javascript

```

# 41. Valid Parentheses

```javascript
var isValid = function (s) {
  let stack = [];
  let map = {
    ")": "(",
    "]": "[",
    "}": "{",
  };
  for (let i = 0; i < s.length; i++) {
    if (s[i] in map) {
      let val = stack.pop();
      if (val !== map[s[i]]) {
        return false;
      }
    } else {
      stack.push(s[i]);
    }
  }
  return stack.length === 0;
};
```
