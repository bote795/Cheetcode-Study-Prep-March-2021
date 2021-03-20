## Question #1 Google Interview Question Two Sum (Easy)

```javascript
var twoSum = function (nums, target) {
  const map = new Map();
  for (let i = 0; i < nums.length; i++) {
    let remainder = target - nums[i];
    if (map.has(nums[i])) {
      return [map.get(nums[i]), i];
    } else {
      map.set(remainder, i);
    }
  }
  return [];
};
```

## Question #2 Container With Most Water (Medium)

```javascript
var maxArea = function(height) {
let left =0;
let right =height.length-1;
let areaMax =0;

while(left < right){
  let width = right-left;
  let length =  Math.min(height[left], height[right]);
  let area =  length* width;
  areaMax = Math.max(areaMax, area);
  if(height[left]<height[right]){
    left++;
  }
  else {
    right--;
  }
}
```

## Question #3 Trapping Rainwater (Hard)

```javascript
/*
1. Identify the pointer with the lesser value
2. Is this pointer value greater than or equal to max on that side
  yes -> update max on that side
  no -> get water for pointer, add to total
3. move pointer inwards
4. repeat for other pointer
 */
/**
 * @param {number[]} height
 * @return {number}
 */
var trap = function (heights) {
  let left = 0,
    right = heights.length - 1,
    totalWater = 0,
    maxLeft = 0,
    maxRight = 0;

  while (left < right) {
    if (heights[left] <= heights[right]) {
      if (heights[left] >= maxLeft) {
        maxLeft = heights[left];
      } else {
        totalWater += maxLeft - heights[left];
      }
      left++;
    } else {
      if (heights[right] >= maxRight) {
        maxRight = heights[right];
      } else {
        totalWater += maxRight - heights[right];
      }

      right--;
    }
  }

  return totalWater;
};
```

## Question #4 Backspace String Compare (Easy)

```javascript
// runing time  O(n) and space O(n) with a  stack
//runing time O(n) and space O(1)
var backspaceCompare = function(S, T) {

    let i= S.length-1, j =T.length-1;
    let skipS =0, skipT=0;
    while(i >=0 || j >= 0){
        //find position for next possible char in S
        while(i >=0){
            if(S[i] ==='#') {
                skipS++;
            }
            else if (skipS >0){
                skipS--;
            }
            else break;
            i--;
        }
        //find position for next possible char in S
        while(j >=0){
            if(T[j] ==='#') {
                skipT++;
            }
            else if (skipT >0){
                skipT--;
            }
            else break;
            j--;
        }
        // If two actual characters are different
        if(i >=0 && j>=0 && S[i] != T[j]){
            return false;
        }
        if((i>=0) != (j>= 0)){
            return false;
        }
        i--;
        j--;
    }
    return true;

};

}
```

## Question #5 Longest Substring Without Repeating Characters (Medium)

```javascript
// running time O(n^2) and space O(1) brute force
// runing time O(n) and space O(1)
var lengthOfLongestSubstring = function (s) {
  let max = 0;
  let index = {};
  let right = 0;

  for (let left = 0; right < s.length; right++) {
    // use left or old value of right where we have already tested the ones b4
    left = Math.max(index[s[right]] || 0, left);
    max = Math.max(max, right - left + 1);
    //save right so left can move to it later
    index[s[right]] = right + 1;
  }
  return max;
};
```

## Question #6a Valid Palindrome(Easy)

```javascript
//runining time  O(n) and space O(1)
var isPalindrome = function (s) {
  let clean = s.toLowerCase().replace(/[^a-z0-9]/gi, "");
  return clean === clean.split("").reverse().join("");
};

var isPalindrome = function (s) {
  s = s.toLowerCase().replace(/[^a-z0-9]/gi, "");

  let left = 0;
  right = s.length - 1;

  while (left < right) {
    if (s[left] !== s[right]) {
      return false;
    }

    left++;
    right--;
  }

  return true;
};
```

## Question #6b Almost Palindrome (Easy)

```javascript
// running time O(n) and space O(1) - two pointer
var validSubPalindrome = function (s, left, right) {
  while (left < right) {
    if (s[left] !== s[right]) {
      return false;
    }

    left++;
    right--;
  }

  return true;
};

var validPalindrome = function (s) {
  let start = 0;
  let end = s.length - 1;
  while (start < end) {
    if (s[start] !== s[end]) {
      //when u find one that isn't the space lets try without it to the left and to the right
      return (
        validSubPalindrome(s, start + 1, end) ||
        validSubPalindrome(s, start, end - 1)
      );
    }
    start++;
    end--;
  }
  return true;
};
```

## Question 7 Reverse Linked List II, M, N Reversals

```javascript
// running O(n), space O(1)
//p,q are indexs to the position in the linkedlist not the values
const reverseSubList = function (head, p, q) {
  let current = head;
  let prev = null;
  let i = 0;
  while (current !== null && i < p - 1) {
    prev = current;
    current = current.next;
    i++;
  }

  let lastPartOfFirstLink = prev;

  let endOfLinkAfterReverse = current;

  current = endOfLinkAfterReverse;
  let next = null;
  prev = null;
  i = 0;
  while ((current != null) & (i < q - p + 1)) {
    next = current.next;
    current.next = prev;
    prev = current;
    current = next;
    i++;
  }
  if (lastPartOfFirstLink !== null) {
    // need to link first part to reversed
    lastPartOfFirstLink.next = prev;
  } else {
    head = prev;
  }
  //reversed to what is left of the linklist
  endOfLinkAfterReverse.next = current;
  return head;
};
```

## Question 8 Flatten a Multilevel Doubly Linked List

```javascript
// running time O(n), space O(1)
// merge up the child linkedlist and keep iterating next
var flatten = function (head) {
  if (!head) return head;

  let currentNode = head;
  while (currentNode !== null) {
    if (currentNode.child === null) {
      currentNode = currentNode.next;
    } else {
      let tail = currentNode.child;
      while (tail.next !== null) {
        tail = tail.next;
      }

      tail.next = currentNode.next;
      if (tail.next !== null) {
        tail.next.prev = tail;
      }

      currentNode.next = currentNode.child;
      currentNode.next.prev = currentNode;
      currentNode.child = null;
    }
  }

  return head;
};
```

## Question 9 Cycle Detection

```javascript
// running O(n), space O(1)
function find_start(head, cycle_length) {
  let pointer1 = head,
    pointer2 = head;
  // move pointer2 ahead 'cycle_length' nodes
  while (cycle_length > 0) {
    pointer2 = pointer2.next;
    cycle_length -= 1;
  }
  // increment both pointers until they meet at the start of the cycle
  while (pointer1 !== pointer2) {
    pointer1 = pointer1.next;
    pointer2 = pointer2.next;
  }

  return pointer1;
}
function calculate_cycle_length(slow) {
  let current = slow,
    cycle_length = 0;
  while (true) {
    current = current.next;
    cycle_length += 1;
    if (current === slow) {
      break;
    }
  }
  return cycle_length;
}

function find_cycle_start(head) {
  if (head == null || head.next == null) return null;
  cycle_length = 0;
  // find the LinkedList cycle
  let slow = head,
    fast = head;
  while (fast !== null && fast.next !== null) {
    fast = fast.next.next;
    slow = slow.next;
    if (slow === fast) {
      // found the cycle
      cycle_length = calculate_cycle_length(slow);
      let start = find_start(head, cycle_length);
      if (start != -1) return start;
      else return null;
      break;
    }
  }
  return null;
}
```

# Question #10 Valid Parentheses (Easy)

var isValid = function(s) {

```javascript
// running O(n), space O(n)
var isValid = function (s) {
  if (s.length < 2) return false;
  let stack = [];
  let matchingRightToLeft = {
    ")": "(",
    "}": "{",
    "]": "[",
  };

  for (let i = 0; i < s.length; i++) {
    if (s[i] in matchingRightToLeft) {
      let item = stack.pop();
      if (item !== matchingRightToLeft[s[i]]) {
        return false;
      }
    } else {
      stack.push(s[i]);
    }
  }
  return stack.length == 0;
};
```

# Question #11 Minimum Brackets To Remove To Make Valid (Medium)

```javascript
// running O(n), space O(n)
var minRemoveToMakeValid = function (s) {
  const answer = s.split("");
  const stack = [];

  for (let i = 0; i < answer.length; i++) {
    if (answer[i] === "(") {
      stack.push(i);
    } else if (answer[i] === ")" && stack.length) {
      stack.pop();
    } else if (answer[i] === ")") {
      answer[i] = "";
    }
  }

  while (stack.length) {
    const curIndex = stack.pop();
    answer[curIndex] = "";
  }

  return answer.join("");
};
```

# Question #12 Implement Queue With Stacks (Easy)

```javascript
class MyQueue {
  constructor() {
    this.data = [];
    this.stack2 = [];
  }

  /**
   * Push element x to the back of queue.
   * @param {number} x
   * @return {void}
   */
  push(x) {
    this.data.push(x);
  }

  /**
   * Removes the element from in front of queue and returns that element.
   * @return {number}
   */
  pop() {
    while (this.data.length) {
      this.stack2.push(this.data.pop());
    }
    let poped = this.stack2.pop();
    while (this.stack2.length) {
      this.data.push(this.stack2.pop());
    }
    return poped;
  }

  /**
   * Get the front element.
   * @return {number}
   */
  peek() {
    return this.data[0];
  }

  /**
   * Returns whether the queue is empty.
   * @return {boolean}
   */
  empty() {
    if (this.data.length || this.stack2.length) return false;
    else return true;
  }
}
```

# 13 Kth Largest Element (Medium)

```javascript
const swap = function (array, i, j) {
  const temp = array[i];
  array[i] = array[j];
  array[j] = temp;
};

const getPartition = function (nums, left, right) {
  let i = left;

  for (let j = left; j <= right; j++) {
    if (nums[j] <= nums[right]) {
      swap(nums, i, j);
      i++;
    }
  }
  return i - 1;
};

const quickSort = function (nums, left, right) {
  if (left < right) {
    const partitionIndex = getPartition(nums, left, right);

    quickSort(nums, left, partitionIndex - 1);
    quickSort(nums, partitionIndex + 1, right);
  }
};

var findKthLargest = function (nums, k) {
  const indexToFind = nums.length - k;
  quickSort(nums, 0, nums.length - 1);
  return nums[indexToFind];
};
```

# Start And End Of Target (Medium)

```javascript
const binarySearch = (nums, left, right, target) => {
  while (left <= right) {
    const mid = Math.floor((left + right) / 2);
    const foundVal = nums[mid];
    if (foundVal === target) {
      return mid;
    } else if (foundVal < target) {
      left = mid + 1;
    } else {
      right = mid - 1;
    }
  }

  return -1;
};

const searchRange = function (nums, target) {
  if (nums.length < 1) return [-1, -1];
  const firstPos = binarySearch(nums, 0, nums.length - 1, target);

  if (firstPos === -1) return [-1, -1];

  let endPos = firstPos,
    startPos = firstPos,
    temp1,
    temp2;

  while (startPos !== -1) {
    temp1 = startPos;
    startPos = binarySearch(nums, 0, startPos - 1, target);
  }
  startPos = temp1;

  while (endPos !== -1) {
    temp2 = endPos;
    endPos = binarySearch(nums, endPos + 1, nums.length - 1, target);
  }
  endPos = temp2;

  return [startPos, endPos];
};
```

# Question #15 Maximum Depth Of Binary Tree (Easy)

```javascript
var maxDepth = function (root, currentDepth) {
  if (!node) {
    return currentDepth;
  }
  currentDepth++;
  return Math.max(
    maxDepth(node.right, currentDepth),
    maxDepth(node.left, currentDepth)
  );
};
```

# Question #16 Level Order Of Binary Tree (Medium)

```javascript
//recursive
const getAllLevels = (root, level, data) => {
  if (!root) return;
  if (level >= data.length) data.push([]);
  data[level].push(root.val);
  getAllLevels(root.left, level + 1, data);
  getAllLevels(root.right, level + 1, data);
};
var levelOrder = function (root) {
  let data = [];
  getAllLevels(root, 0, data);
  return data;
};
```

```javascript
// iterative approach
var levelOrder = function (root) {
  if (root == null) return [];

  const res = [];
  const queue = [root];

  while (queue.length) {
    const level = [];
    let s = queue.length;
    for (let i = 0; i < s; i++) {
      const node = queue.shift();
      level.push(node.val);
      if (node.left) queue.push(node.left);
      if (node.right) queue.push(node.right);
    }
    res.push(level);
  }
  return res;
};
```

# Question #17 Right Side View of Tree (Medium)

```javascript
// BFS
var rightSideView = function (root) {
  if (root == null) return [];

  const result = [];
  let queue = [root];
  while (queue.length) {
    let levelLength = queue.length;
    let currentLevel = [];
    let current;
    for (let i = 0; i < levelLength; i++) {
      current = queue.shift();
      currentLevel.push(current.val);
      if (current.left) queue.push(current.left);
      if (current.right) queue.push(current.right);
    }
    result.push(current.val);
  }
  return result;
};
```

```javascript
//DFS
const dfs = (node, currentLevel, result) => {
  if (!node) return;
  if (currentLevel >= result.length) {
    result.push(node.val);
  }

  if (node.right) {
    dfs(node.right, currentLevel + 1, result);
  }

  if (node.left) {
    dfs(node.left, currentLevel + 1, result);
  }
};
var rightSideView = function (root) {
  const result = [];

  dfs(root, 0, result);
  return result;
};
```

# Question #18 Number Of Nodes In Complete Tree (Medium)

```javascript
const getTreeHeight = function (root) {
  let height = 0;
  while (root.left !== null) {
    height++;
    root = root.left;
  }

  return height;
};

const nodeExists = function (idxToFind, height, node) {
  let left = 0,
    right = Math.pow(2, height) - 1,
    count = 0;

  while (count < height) {
    const midOfNode = Math.ceil((left + right) / 2);

    if (idxToFind >= midOfNode) {
      node = node.right;
      left = midOfNode;
    } else {
      node = node.left;
      right = midOfNode - 1;
    }

    count++;
  }

  return node !== null;
};

const countNodes = function (root) {
  if (!root) return 0;

  const height = getTreeHeight(root);

  if (height === 0) return 1;

  const upperCount = Math.pow(2, height) - 1;

  let left = 0,
    right = upperCount;

  while (left < right) {
    const idxToFind = Math.ceil((left + right) / 2);

    if (nodeExists(idxToFind, height, root)) {
      left = idxToFind;
    } else {
      right = idxToFind - 1;
    }
  }

  return upperCount + left + 1;
};
```

# Question #19 Validate Binary Search Tree (Medium)

```javascript
const dfs = function (node, min, max) {
  if (node.val <= min || node.val >= max) {
    return false;
  }

  if (node.left) {
    if (!dfs(node.left, min, node.val)) {
      return false;
    }
  }

  if (node.right) {
    if (!dfs(node.right, node.val, max)) {
      return false;
    }
  }

  return true;
};

const isValidBST = function (root) {
  if (!root) return true;
  return dfs(root, -Infinity, Infinity);
};
```

# Validate Binary Search Tree

```javascript
var isValidBST = function (root) {
  const helper = (current, max = Infinity, min = -Infinity) => {
    if (current === null) return true;

    if (current.val < max && current.val > min) {
      return (
        helper(current.left, current.val, min) &&
        helper(current.right, max, current.val)
      );
    } else {
      return false;
    }
  };
  return helper(root);
};
```

# 2D-Arrays

## DFS in 2D-Arrays

```javascript
function dfs(matrix, row, column, seen, values) {
  if (
    row > matrix.length - 1 ||
    row < 0 ||
    column > matrix[0].length - 1 ||
    column < 0 ||
    seen[row][column]
  )
    return;
  values.push(matrix[row][column]);
  seen[row][column] = true;
  dfs(matrix, row + 1, column, seen, values);
  dfs(matrix, row - 1, column, seen, values);
  dfs(matrix, row, column - 1, seen, values);
  dfs(matrix, row, column + 1, seen, values);
}
var dfs2Darray = function (matrix) {
  const seen = new Array(matrix.length).fill(
    new Array(matrix[0].length).fill(false)
  );
  const values = [];
  dfs(matrix, 0, 0, seen, values);
  return values;
};
```

## BFS in 2D

```javascript
var bfs2Darray = function(matrix) {
  const seen = new Array(matrix.length).fill(
    new Array(matrix[0].length).fill(false)
  );
  const values = [];
  let queue=[[0, 0]];

  while(queue.length >0){
      let [row, col] = queue.shift();
      if(seen[row][col] ||
          row > matrix.length - 1 ||
          row < 0 ||
          column > matrix[0].length - 1 ||
          column < 0 ){
        continue;
      }
      values.push(matrx[row][col]);
      seen[row][col]=true;
      let directions = [
        [-1, 0],
        [1,0],
        [0,1],
        [0,-1]
      ]
      for(int i=0; i < directions.length; i++)
        queue.push([row + directions[i][0], col + directions[i][1]])

  }

  return values;
}
```

# Number Of Islands (Medium)

```javascript
//BFS
function pushIfValid(q, rows, cols, x, y) {
  if (x >= 0 && x < rows && y >= 0 && y < cols) {
    q.push([x, y]);
  }
}

function helperBfs(binaryMatrix, i, j, rows, cols) {
  let queue = [[i, j]];
  while (queue.length) {
    let item = queue.shift();
    const [x, y] = item;
    if (binaryMatrix[x][y] === 1) {
      binaryMatrix[x][y] = -1;
      pushIfValid(queue, rows, cols, x - 1, y);
      pushIfValid(queue, rows, cols, x, y - 1);
      pushIfValid(queue, rows, cols, x + 1, y);
      pushIfValid(queue, rows, cols, x, y + 1);
    }
  }
  return null;
}

function getNumberOfIslands(binaryMatrix) {
  let rows = binaryMatrix.length;
  let cols = binaryMatrix[0].length;
  let total = 0;
  for (let x = 0; x < binaryMatrix.length; x++) {
    for (let y = 0; y < binaryMatrix[x].length; y++) {
      // need to check if its been visited
      if (binaryMatrix[x][y] == 1) {
        helperBfs(binaryMatrix, x, y, rows, cols);
        total++;
      }
    }
  }
  return total;
}

//DFS
function sinkIsland(binaryMatrix, x, y, rows, cols){
  if(binaryMatrix[x][y] === 0 ||
      x >= rows ||
      x < 0 ||
      y > cols ||
      y < 0
    )
    return;
    binaryMatrix[x][y]=0;
    let directions = [
        [-1, 0],
        [1,0],
        [0,1],
        [0,-1]
    ];
    for(int i=0; i < directions.length; i++)
      sinkIsland([row + directions[i][0], col + directions[i][1]])

}


function getNumberOfIslands(binaryMatrix) {
  let rows = binaryMatrix.length;
  let cols = binaryMatrix[0].length;
  let total = 0;
  for (let x = 0; x < rows; x++) {
    for (let y = 0; y < cols; y++) {
      // need to check if its been visited
      if (binaryMatrix[x][y] == 1) {
        sinkIsland(binaryMatrix, x, y, rows, cols);
        total++;
      }
    }
  }
  return total;
}

```

# Rotting Oranges

```javascript
var orangesRotting = function (grid) {
  let row = grid.length;
  let col = grid[0].length;
  let queue = [];
  let freshOranges = 0;

  const ROTTEN = 2;
  const FRESH = 1;
  const EMPTY = 0;

  let maxMins = 0;
  for (let x = 0; x < row; x++) {
    for (let y = 0; y < col; y++) {
      if (grid[x][y] === ROTTEN) {
        queue.push([x, y]);
      } else if (grid[x][y] === FRESH) {
        freshOranges++;
      }
    }
  }
  let directions = [
    [-1, 0],
    [1, 0],
    [0, 1],
    [0, -1],
  ];
  let currentQueueSize = queue.length;
  while (queue.length > 0) {
    if (currentQueueSize === 0) {
      currentQueueSize = queue.length;
      maxMins++;
    }
    const [x, y] = queue.shift();
    currentQueueSize--;
    for (let i = 0; i < directions.length; i++) {
      const currentDir = directions[i];
      const nextX = x + directions[i][0];
      const nextY = y + directions[i][1];
      if (
        nextX < 0 ||
        nextX >= grid.length ||
        nextY < 0 ||
        nextY >= grid[0].length
      ) {
        continue;
      }
      if (grid[nextX][nextY] == FRESH) {
        grid[nextX][nextY] = ROTTEN;
        freshOranges--;
        queue.push([nextX, nextY]);
      }
    }
  }

  if (freshOranges !== 0) return -1;

  return maxMins;
};
```

# Walls and Gates

```javascript
// time O(M^2 N^2) and space O(MN) keep gonig through all empty rooms
// time O(MN) and O(MN)
var wallsAndGates = function (rooms) {
  const EMPTY = 2147483647;
  const GATE = 0;
  const DIRECTIONS = [
    [1, 0],
    [-1, 0],
    [0, 1],
    [0, -1],
  ];
  let rows = rooms.length;
  let cols = rooms[0].length;
  let queue = [];
  for (let x = 0; x < rows; x++) {
    for (let y = 0; y < cols; y++) {
      if (rooms[x][y] === GATE) {
        queue.push([x, y]);
      }
    }
  }
  while (queue.length > 0) {
    let [x, y] = queue.shift();
    for (let i = 0; i < DIRECTIONS.length; i++) {
      let newX = x + DIRECTIONS[i][0];
      let newY = y + DIRECTIONS[i][1];

      if (
        newX < 0 ||
        newY < 0 ||
        newX >= rooms.length ||
        newY >= rooms[0].length ||
        rooms[newX][newY] !== EMPTY
      ) {
        continue;
      }

      rooms[newX][newY] = rooms[x][y] + 1;
      queue.push([newX, newY]);
    }
  }
  return rooms;
};
```
