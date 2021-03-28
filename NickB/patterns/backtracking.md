# backtracking

```
Given a set with distinct elements, find all of its distinct subsets.

Example 1:

Input: [1, 3]
Output: [], [1], [3], [1,3]
Example 2:

Input: [1, 5, 3]
Output: [], [1], [5], [3], [1,5], [1,3], [5,3], [1,5,3]
```

```javascript
// time O(N*2^N) space  O(N*2^N)

let x = [];
let y = x;
y.push(4);

const find_subsets = function (nums) {
  let subsets = [];
  subsets.push([]);
  for (let i = 0; i < nums.length; i++) {
    let levelLength = subsets.length;
    for (let j = 0; j < levelLength; j++) {
      let curr = [...subsets[j]];
      curr.push(nums[i]);
      subsets.push(curr);
    }
  }
  return subsets;
};
```

# Subsets With Duplicates

```javascript
// time O(N*2^N) space  O(N*2^N)
function find_subsets(nums) {
  // sort the numbers to handle duplicates
  nums.sort((a, b) => a - b);
  const subsets = [];
  subsets.push([]);
  let startIndex = 0,
    endIndex = 0;
  for (i = 0; i < nums.length; i++) {
    startIndex = 0;
    // if current and the previous elements are same, create new subsets only from the subsets
    // added in the previous step
    if (i > 0 && nums[i] === nums[i - 1]) {
      startIndex = endIndex + 1;
    }
    endIndex = subsets.length - 1;
    for (j = startIndex; j < endIndex + 1; j++) {
      // create a new subset from the existing subset and add the current element to it
      const set1 = subsets[j].slice(0);
      set1.push(nums[i]);
      subsets.push(set1);
    }
  }
  return subsets;
}
```
