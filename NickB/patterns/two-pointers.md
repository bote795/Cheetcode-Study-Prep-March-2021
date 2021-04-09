# Two pointers

# Basic problem:

## Pair with Target Sum (Two sum)

```javascript
/*
usually have two pointers 
check if we have met requirement
depending on constrains one will contract else other well shrink
helpful for finding a pair, triplet or subarray 
  can be sorted or unosrted
  array or linkedlist 
*/
// running time: O(N) and O(N) with a hash table
// running time: O(N) and O(1)
const pair_with_targetsum = function (arr, target_sum) {
  let left = 0;
  let right = arr.length - 1;
  while (left < right) {
    if (arr[left] + arr[right] === target_sum) {
      return [left, right];
    } else if (arr[left] + arr[right] > target_sum) {
      right--;
    } else {
      left++;
    }
  }
  return [];
};
```

---

## Remove Duplicates

```
Given an array of sorted numbers, remove all duplicates from it. You should not use any extra space; after removing the duplicates in-place return the length of the subarray that has no duplicate in it.
Example 1:

Input: [2, 3, 3, 3, 6, 9, 9]
Output: 4
Explanation: The first four elements after removing the duplicates will be [2, 3, 6, 9].
Example 2:

Input: [2, 2, 2, 11]
Output: 2
Explanation: The first two elements after removing the duplicates will be [2, 11].
```

```javascript
// running time: O(N) and O(1)
function remove_duplicates(arr) {
  // index of the next non-duplicate element
  let nextNonDuplicate = 1;

  let i = 1;
  while (i < arr.length) {
    if (arr[nextNonDuplicate - 1] !== arr[i]) {
      arr[nextNonDuplicate] = arr[i];
      nextNonDuplicate += 1;
    }
    i += 1;
  }

  return nextNonDuplicate;
}
```

```
Problem 1: Given an unsorted array of numbers and a target ‘key’, remove all instances of ‘key’ in-place and return the new length of the array.

Example 1:

Input: [3, 2, 3, 6, 3, 10, 9, 3], Key=3
Output: 4
Explanation: The first four elements after removing every 'Key' will be [2, 6, 10, 9].
Example 2:

Input: [2, 11, 2, 2, 1], Key=2
Output: 2
Explanation: The first two elements after removing every 'Key' will be [11, 1].
Solution: This problem is quite similar to our parent problem. We can follow a two-pointer approach and shift numbers left upon encountering the ‘key’. Here is what the code will look
```

```javascript
// running time O(N) space O(1)
function remove_element(arr, key) {
  let nextElement = 0; // index of the next element which is not 'key'
  for (i = 0; i < arr.length; i++) {
    if (arr[i] !== key) {
      arr[nextElement] = arr[i];
      nextElement += 1;
    }
  }
  return nextElement;
}
```

# Squaring a Sorted Array

```
Given a sorted array, create a new array containing squares of all the numbers of the input array in the sorted order.

Example 1:

Input: [-2, -1, 0, 2, 3]
Output: [0, 1, 4, 4, 9]
Example 2:

Input: [-3, -1, 0, 1, 2]
Output: [0, 1, 1, 4, 9]
```

```javascript
//running time O(n) and space O(N)
const make_squares = function (arr) {
  let squares = Array(arr.length).fill(0);
  let left = 0;
  let right = arr.length - 1;
  let hightestSquareIndex = arr.length - 1;
  while (left <= right) {
    let leftSquare = arr[left] * arr[left];
    let rightSquare = arr[right] * arr[right];
    if (rightSquare > leftSquare) {
      squares[hightestSquareIndex] = rightSquare;
      right--;
    } else {
      squares[hightestSquareIndex] = leftSquare;
      left++;
    }
    hightestSquareIndex--;
  }

  return squares;
};
```

# Triplet Sum to Zero (medium)

```
Problem Statement #
Given an array of unsorted numbers, find all unique triplets in it that add up to zero.

Example 1:

Input: [-3, 0, 1, 2, -1, 1, -2]
Output: [-3, 1, 2], [-2, 0, 2], [-2, 1, 1], [-1, 0, 1]
Explanation: There are four unique triplets whose sum is equal to zero.
Example 2:

Input: [-5, 2, -1, -2, 3]
Output: [[-5, 2, 3], [-2, -1, 3]]
Explanation: There are two unique triplets whose sum is equal to zero.
```

```javascript
// running time O(n^2)  and space O(n) since sortings
function search_triplets(arr) {
  arr.sort((a, b) => a - b);
  let triplets = [];
  for (i = 0; i < arr.length; i++) {
    if (i > 0 && arr[i] === arr[i - 1]) {
      // skip same element to avoid duplicate triplets
      continue;
    }
    search_pair(arr, -arr[i], i + 1, triplets);
  }

  return triplets;
}

function search_pair(arr, target_sum, left, triplets) {
  let right = arr.length - 1;
  while (left < right) {
    const current_sum = arr[left] + arr[right];
    if (current_sum === target_sum) {
      // found the triplet
      triplets.push([-target_sum, arr[left], arr[right]]);
      left += 1;
      right -= 1;
      while (left < right && arr[left] === arr[left - 1]) {
        left += 1; // skip same element to avoid duplicate triplets
      }
      while (left < right && arr[right] === arr[right + 1]) {
        right -= 1; // skip same element to avoid duplicate triplets
      }
    } else if (target_sum > current_sum) {
      left += 1; // we need a pair with a bigger sum
    } else {
      right -= 1; // we need a pair with a smaller sum
    }
  }
}
```

# Triplet Sum Close to Target (medium)

```
Given an array of unsorted numbers and a target number, find a triplet in the array whose sum is as close to the target number as possible, return the sum of the triplet. If there are more than one such triplet, return the sum of the triplet with the smallest sum.

Example 1:

Input: [-2, 0, 1, 2], target=2
Output: 1
Explanation: The triplet [-2, 1, 2] has the closest sum to the target.
Example 2:

Input: [-3, -1, 1, 2], target=1
Output: 0
Explanation: The triplet [-3, 1, 2] has the closest sum to the target.
Example 3:

Input: [1, 0, 1, 1], target=100
Output: 3
Explanation: The triplet [1, 1, 1] has the closest sum to the target.
```

```javascript
// running time O(n^2) and O(n)
function triplet_sum_close_to_target(arr, targetSum) {
  arr.sort((a, b) => a - b);
  let smallest_difference = Infinity;
  for (let i = 0; i < arr.length - 2; i++) {
    let left = i + 1,
      right = arr.length - 1;
    while (left < right) {
      const target_diff = targetSum - arr[i] - arr[left] - arr[right];
      if (target_diff === 0) {
        // we've found a triplet with an exact sum
        return targetSum - target_diff; // return sum of all the numbers
      }

      if (Math.abs(target_diff) < Math.abs(smallest_difference)) {
        smallest_difference = target_diff; // save the closest difference
      }
      // the second part of the following 'if' is to handle the smallest sum when we have more than one solution
      if (
        Math.abs(target_diff) < Math.abs(smallest_difference) ||
        (Math.abs(target_diff) === Math.abs(smallest_difference) &&
          target_diff > smallest_difference)
      ) {
        smallest_difference = target_diff; // save the closest and the biggest difference
      }

      if (target_diff > 0) {
        left += 1; // we need a triplet with a bigger sum
      } else {
        right -= 1; // we need a triplet with a smaller sum
      }
    }
  }
  return targetSum - smallest_difference;
}
```

# Triplets with Smaller Sum

```
Given an array arr of unsorted numbers and a target sum, count all triplets in it such that arr[i] + arr[j] + arr[k] < target where i, j, and k are three different indices. Write a function to return the count of such triplets.

Example 1:

Input: [-1, 0, 2, 3], target=3
Output: 2
Explanation: There are two triplets whose sum is less than the target: [-1, 0, 3], [-1, 0, 2]
Example 2:

Input: [-1, 4, 2, 1, 3], target=5
Output: 4
Explanation: There are four triplets whose sum is less than the target:
[-1, 1, 4], [-1, 1, 3], [-1, 1, 2], [-1, 2, 3]
```

```javascript
// running time O(n^2) and space O(N)
function triplet_with_smaller_sum(arr, target) {
  arr.sort((a, b) => a - b);
  let count = 0;
  for (i = 0; i < arr.length - 2; i++) {
    count += search_pair(arr, target - arr[i], i);
  }
  return count;
}

function search_pair(arr, target_sum, first) {
  let count = 0;
  let left = first + 1,
    right = arr.length - 1;
  while (left < right) {
    if (arr[left] + arr[right] < target_sum) {
      // found the triplet
      // since arr[right] >= arr[left], therefore, we can replace arr[right] by any number between
      // left and right to get a sum less than the target sum
      count += right - left;
      left += 1;
    } else {
      right -= 1; // we need a pair with a smaller sum
    }
  }
  return count;
}
```

# Write a function to return the list of all such triplets instead of the count. How will the time complexity change in this case?

```javascript
// running time: O(n^3) and space O(n)
function triplet_with_smaller_sum(arr, target) {
  arr.sort((a, b) => a - b);
  const triplets = [];
  for (i = 0; i < arr.length - 2; i++) {
    search_pair(arr, target - arr[i], i, triplets);
  }
  return triplets;
}

function search_pair(arr, target_sum, first, triplets) {
  let left = first + 1,
    right = arr.length - 1;
  while (left < right) {
    if (arr[left] + arr[right] < target_sum) {
      // found the triplet
      // since arr[right] >= arr[left], therefore, we can replace arr[right] by any number between
      // left and right to get a sum less than the target sum
      for (i = right; i > left; i--) {
        triplets.push([arr[first], arr[left], arr[i]]);
      }
      left += 1;
    } else {
      right -= 1; // we need a pair with a smaller sum
    }
  }
}
```

# Subarrays with Product Less than a Target

```
Example 1:

Input: [2, 5, 3, 10], target=30
Output: [2], [5], [2, 5], [3], [5, 3], [10]
Explanation: There are six contiguous subarrays whose product is less than the target.
Example 2:

Input: [8, 2, 6, 5], target=50
Output: [8], [2], [8, 2], [6], [2, 6], [5], [6, 5]
Explanation: There are seven contiguous subarrays whose product is less than the target.
```

```javascript
// running time O(n^3) and space O(n^3
const Deque = require("./collections/deque"); //http://www.collectionsjs.com

function find_subarrays(arr, target) {
  let result = [],
    product = 1,
    left = 0;
  for (right = 0; right < arr.length; right++) {
    product *= arr[right];
    while (product >= target && left < arr.length) {
      product /= arr[left];
      left += 1;
    }
    // since the product of all numbers from left to right is less than the target therefore,
    // all subarrays from left to right will have a product less than the target too; to avoid
    // duplicates, we will start with a subarray containing only arr[right] and then extend it
    const tempList = new Deque();
    for (let i = right; i > left - 1; i--) {
      tempList.unshift(arr[i]);
      result.push(tempList.toArray());
    }
  }
  return result;
}
```

# Dutch National Flag Problem

```
Given an array containing 0s, 1s and 2s, sort the array in-place. You should treat numbers of the array as objects, hence, we can’t count 0s, 1s, and 2s to recreate the array.

The flag of the Netherlands consists of three colors: red, white and blue; and since our input array also consists of three different numbers that is why it is called Dutch National Flag problem.

Example 1:

Input: [1, 0, 2, 1, 0]
Output: [0 0 1 1 2]
Example 2:

Input: [2, 2, 0, 1, 2, 0]
Output: [0 0 1 2 2 2 ]
```

```javascript
// running time O(n) and space O(1)
const dutch_flag_sort = function (arr) {
  let zeroIdx = 0;
  let twoIdx = arr.length - 1;
  while (arr[zeroIdx] === 0) {
    zeroIdx++;
  }
  while (arr[twoIdx] === 2) {
    twoIdx--;
  }
  const swap = function (arr, x, y) {
    let temp = arr[x];
    arr[x] = arr[y];
    arr[y] = temp;
  };
  let i = zeroIdx + 1;
  while (i <= twoIdx) {
    if (arr[i] === 0) {
      swap(arr, i, zeroIdx);
      zeroIdx++;
    } else if (arr[i] === 2) {
      swap(arr, i, twoIdx);
      twoIdx--;
    } else {
      i++;
    }
  }
  return arr;
};
```

# Quadruple Sum to Target (medium)

```
Given an array of unsorted numbers and a target number, find all unique quadruplets in it, whose sum is equal to the target number.

Example 1:

Input: [4, 1, 2, -1, 1, -3], target=1
Output: [-3, -1, 1, 4], [-3, 1, 1, 2]
Explanation: Both the quadruplets add up to the target.
Example 2:

Input: [2, 0, -1, 1, -2, 2], target=2
Output: [-2, 0, 2, 2], [-1, 0, 1, 2]
Explanation: Both the quadruplets add up to the target.
```

```javascript
// running time O(n^3) and space O(N)
function search_quadruplets(arr, target) {
  arr.sort((a, b) => a - b);
  const quadruplets = [];
  for (let i = 0; i < arr.length - 3; i++) {
    // skip same element to avoid duplicate quadruplets
    if (i > 0 && arr[i] === arr[i - 1]) {
      continue;
    }
    for (let j = i + 1; j < arr.length - 2; j++) {
      // skip same element to avoid duplicate quadruplets
      if (j > i + 1 && arr[j] === arr[j - 1]) {
        continue;
      }
      search_pairs(arr, target, i, j, quadruplets);
    }
  }
  return quadruplets;
}

function search_pairs(arr, targetSum, first, second, quadruplets) {
  let left = second + 1,
    right = arr.length - 1;
  while (left < right) {
    sum = arr[first] + arr[second] + arr[left] + arr[right];
    if (sum === targetSum) {
      // found the quadruplet
      quadruplets.push([arr[first], arr[second], arr[left], arr[right]]);
      left += 1;
      right -= 1;
      while (left < right && arr[left] === arr[left - 1]) {
        left += 1; // skip same element to avoid duplicate quadruplets
      }
      while (left < right && arr[right] === arr[right + 1]) {
        right -= 1; // skip same element to avoid duplicate quadruplets
      }
    } else if (sum < targetSum) {
      left += 1; // we need a pair with a bigger sum
    } else {
      right -= 1; // we need a pair with a smaller sum
    }
  }
}

console.log(search_quadruplets([4, 1, 2, -1, 1, -3], 1));
console.log(search_quadruplets([2, 0, -1, 1, -2, 2], 2));
```

# Comparing Strings containing Backspaces

```
Given two strings containing backspaces (identified by the character ‘#’), check if the two strings are equal.

Example 1:

Input: str1="xy#z", str2="xzz#"
Output: true
Explanation: After applying backspaces the strings become "xz" and "xz" respectively.
Example 2:

Input: str1="xy#z", str2="xyz#"
Output: false
Explanation: After applying backspaces the strings become "xz" and "xy" respectively.
Example 3:

Input: str1="xp#", str2="xyz##"
Output: true
Explanation: After applying backspaces the strings become "x" and "x" respectively.
In "xyz##", the first '#' removes the character 'z' and the second '#' removes the character 'y'.
Example 4:

Input: str1="xywrrmp", str2="xywrrmu#p"
Output: true
Explanation: After applying backspaces the strings become "xywrrmp" and "xywrrmp" respectively.
```

```javascript
//O(M + N) M and N are the lengths of the two input strings space O(1)
function backspace_compare(str1, str2) {
  // use two pointer approach to compare the strings
  let index1 = str1.length - 1,
    index2 = str2.length - 1;
  while (index1 >= 0 || index2 >= 0) {
    let i1 = get_next_valid_char_index(str1, index1),
      i2 = get_next_valid_char_index(str2, index2);
    if (i1 < 0 && i2 < 0) {
      // reached the end of both the strings
      return true;
    }
    if (i1 < 0 || i2 < 0) {
      // reached the end of one of the strings
      return false;
    }
    if (str1[i1] !== str2[i2]) {
      // check if the characters are equal
      return false;
    }

    index1 = i1 - 1;
    index2 = i2 - 1;
  }
  return true;
}

function get_next_valid_char_index(str, index) {
  let backspaceCount = 0;
  while (index >= 0) {
    if (str[index] === "#") {
      // found a backspace character
      backspaceCount += 1;
    } else if (backspaceCount > 0) {
      // a non-backspace character
      backspaceCount -= 1;
    } else {
      break;
    }

    index -= 1; // skip a backspace or a valid character
  }

  return index;
}
```

# Minimum Window Sort (medium)

```
Given an array, find the length of the smallest subarray in it which when sorted will sort the whole array.

Example 1:

Input: [1, 2, 5, 3, 7, 10, 9, 12]
Output: 5
Explanation: We need to sort only the subarray [5, 3, 7, 10, 9] to make the whole array sorted
Example 2:

Input: [1, 3, 2, 0, -1, 7, 10]
Output: 5
Explanation: We need to sort only the subarray [1, 3, 2, 0, -1] to make the whole array sorted
Example 3:

Input: [1, 2, 3]
Output: 0
Explanation: The array is already sorted
Example 4:

Input: [3, 2, 1]
Output: 3
Explanation: The whole array needs to be sorted.
```

```javascript
// space O(N) and time O(1)
function shortest_window_sort(arr) {
  let low = 0,
    high = arr.length - 1;
  // find the first number out of sorting order from the beginning
  while (low < arr.length - 1 && arr[low] <= arr[low + 1]) {
    low += 1;
  }

  if (low === arr.length - 1) {
    // if the array is sorted
    return 0;
  }

  // find the first number out of sorting order from the end
  while (high > 0 && arr[high] >= arr[high - 1]) {
    high -= 1;
  }

  // find the maximum and minimum of the subarray
  let subarrayMax = -Infinity,
    subarrayMin = Infinity;
  for (k = low; k < high + 1; k++) {
    subarrayMax = Math.max(subarrayMax, arr[k]);
    subarrayMin = Math.min(subarrayMin, arr[k]);
  }

  // extend the subarray to include any number which is bigger than the minimum of the subarray
  while (low > 0 && arr[low - 1] > subarrayMin) {
    low -= 1;
  }
  // extend the subarray to include any number which is smaller than the maximum of the subarray
  while (high < arr.length - 1 && arr[high + 1] < subarrayMax) {
    high += 1;
  }

  return high - low + 1;
}
```
