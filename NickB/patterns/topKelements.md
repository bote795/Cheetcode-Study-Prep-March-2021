# Top 'K' Numbers

```javascript
const Heap = require("./collections/heap"); //http://www.collectionsjs.com

function find_k_largest_numbers(nums, k) {
  const minHeap = new Heap([], null, (a, b) => b - a);
  // put first 'K' numbers in the min heap
  for (i = 0; i < k; i++) {
    minHeap.push(nums[i]);
  }

  // go through the remaining numbers of the array, if the number from the array is bigger than the
  // top(i.e., smallest) number of the min-heap, remove the top number from heap and add the number from array
  for (i = k; i < nums.length; i++) {
    if (nums[i] > minHeap.peek()) {
      minHeap.pop();
      minHeap.push(nums[i]);
    }
  }

  // the heap has the top 'K' numbers, return them in a list
  return minHeap.toArray();
}
```

# Top 'K' Numbers

```javascript
const Heap = require("./collections/heap"); //http://www.collectionsjs.com

function find_Kth_smallest_number(nums, k) {
  maxHeap = new Heap();
  // put first k numbers in the max heap
  for (i = 0; i < k; i++) {
    maxHeap.push(nums[i]);
  }

  // go through the remaining numbers of the array, if the number from the array is smaller than the
  // top(biggest) number of the heap, remove the top number from heap and add the number from array
  for (i = k; i < nums.length; i++) {
    if (nums[i] < maxHeap.peek()) {
      maxHeap.pop();
      maxHeap.push(nums[i]);
    }
  }

  // the root of the heap has the Kth smallest number
  return maxHeap.peek();
}
```

# 'K' closest points to the origin

```javascript
const find_closest_points = function (points, k) {
  const _comparator = (a, b) => {
    return (
      Math.sqrt(Math.pow(a.x, 2) + Math.pow(a.y, 2)) -
      Math.sqrt(Math.pow(b.x, 2) + Math.pow(b.y, 2))
    );
  };
  const lessThanPoint = (a, b) => {
    return (
      Math.sqrt(Math.pow(a.x, 2) + Math.pow(a.y, 2)) <
      Math.sqrt(Math.pow(b.x, 2) + Math.pow(b.y, 2))
    );
  };
  let heap = new Heap([], null, _comparator);
  for (let i = 0; i < k; i++) {
    heap.push(points[i]);
  }
  for (let i = k; i < points.length; i++) {
    if (lessThanPoint(points[i], heap.peek())) {
      heap.pop();
      heap.push(points[i]);
    }
  }
  return heap.toArray();
};
```

# Connect Ropes

```javascript
// time: O(NlogN) and space O(N)
const Heap = require("./collections/heap"); //http://www.collectionsjs.com
const minimum_cost_to_connect_ropes = function (ropeLengths) {
  const minHeap = new Heap(ropeLengths, null, (a, b) => b - a);
  let cost = 0;
  while (minHeap.length > 1) {
    const newRope = minHeap.pop() + minHeap.pop();
    cost += newRope;
    minHeap.push(newRope);
  }
  return cost;
};
```

#
