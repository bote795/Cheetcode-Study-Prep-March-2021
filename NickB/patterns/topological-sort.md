# Topological Sort

```javascript
// time O(V+E) and space O(V+E)
function topological_sort(vertices, edges) {
  const sortedOrder = [];
  if (vertices <= 0) {
    return sortedOrder;
  }

  // a. Initialize the graph
  const inDegree = Array(vertices).fill(0); // count of incoming edges
  const graph = Array(vertices)
    .fill(0)
    .map(() => Array()); // adjacency list graph

  // b. Build the graph
  edges.forEach((edge) => {
    let parent = edge[0],
      child = edge[1];
    graph[parent].push(child); // put the child into it's parent's list
    inDegree[child]++; // increment child's inDegree
  });

  // c. Find all sources i.e., all vertices with 0 in-degrees
  const sources = [];
  for (i = 0; i < inDegree.length; i++) {
    if (inDegree[i] === 0) {
      sources.push(i);
    }
  }

  // d. For each source, add it to the sortedOrder and subtract one from all of its children's in-degrees
  // if a child's in-degree becomes zero, add it to the sources queue
  while (sources.length > 0) {
    const vertex = sources.shift();
    sortedOrder.push(vertex);
    graph[vertex].forEach((child) => {
      // get the node's children to decrement their in-degrees
      inDegree[child] -= 1;
      if (inDegree[child] === 0) {
        sources.push(child);
      }
    });
  }

  // topological sort is not possible as the graph has a cycle
  if (sortedOrder.length !== vertices) {
    return [];
  }

  return sortedOrder;
}
```

# Tasks Scheduling

```javascript
const is_scheduling_possible = function (tasks, prerequisites) {
  const sortedorder = [];
  let inDegress = new Array(tasks).fill(0);
  let adj = new Array(tasks).fill(0).map(() => []);

  for (let i = 0; i < prerequisites.length; i++) {
    let currTask = prerequisites[i][0];
    let needTask = prerequisites[i][1];
    adj[currTask].push(needTask);
    inDegress[needTask]++;
  }

  let queueSource = [];
  for (let i = 0; i < tasks; i++) {
    if (inDegress[i] === 0) {
      queueSource.push(i);
    }
  }

  while (queueSource.length > 0) {
    let levelLength = queueSource.length;
    for (let i = 0; i < levelLength; i++) {
      let v = queueSource.shift();
      sortedorder.push(v);
      let children = adj[v];
      children.forEach((child) => {
        inDegress[child]--;
        if (inDegress[child] === 0) {
          queueSource.push(child);
        }
      });
    }
  }
  return sortedorder.length === tasks;
};
```

# Tasks Scheduling Order

```javascript
// O (V+E)
// important child, parent matters!!!!
const find_order = function (tasks, prerequisites) {
  let sortedOrder = [];
  let inDegrees = new Array(tasks).fill(0);
  let graphAdjList = new Array(tasks).fill(0).map(() => []);

  //fill in adj list
  prerequisites.forEach((task) => {
    let child = task[1];
    let parent = task[0];
    graphAdjList[parent].push(child);
    inDegrees[child]++;
  });

  //setup sources
  let queueSource = [];
  for (let i = 0; i < inDegrees.length; i++) {
    if (inDegrees[i] === 0) {
      queueSource.push(i);
    }
  }

  while (queueSource.length > 0) {
    let levelLength = queueSource.length;
    for (let i = 0; i < levelLength; i++) {
      let v = queueSource.shift();
      sortedOrder.push(v);
      let children = graphAdjList[v];
      children.forEach((child) => {
        inDegrees[child]--;
        if (inDegrees[child] === 0) {
          queueSource.push(child);
        }
      });
    }
  }

  if (sortedOrder.length !== tasks) {
    return [];
  }
  return sortedOrder;
};
```

# All Tasks Scheduling Orders

```javascript
//O(V!∗E) where ‘V’ is the total number of tasks and ‘E’ is the total prerequisites
function print_orders(tasks, prerequisites) {
  const sortedOrder = [];
  if (tasks <= 0) {
    return sortedOrder;
  }

  // a. Initialize the graph
  const inDegree = Array(tasks).fill(0); // count of incoming edges
  const graph = Array(tasks)
    .fill(0)
    .map(() => Array()); // adjacency list graph

  // b. Build the graph
  prerequisites.forEach((prerequisite) => {
    const parent = prerequisite[0],
      child = prerequisite[1];
    graph[parent].push(child); // put the child into it's parent's list
    inDegree[child]++; // increment child's inDegree
  });

  // c. Find all sources i.e., all vertices with 0 in-degrees
  const sources = [];
  for (i = 0; i < inDegree.length; i++) {
    if (inDegree[i] === 0) {
      sources.push(i);
    }
  }

  print_all_topological_sorts(graph, inDegree, sources, sortedOrder);
}

function print_all_topological_sorts(graph, inDegree, sources, sortedOrder) {
  if (sources.length > 0) {
    for (let i = 0; i < sources.length; i++) {
      const vertex = sources[i];
      sortedOrder.push(vertex);
      const sourcesForNextCall = sources.slice(0); // clone current sources
      // only remove the current source, all other sources should remain in the queue for the next call
      sourcesForNextCall.splice(sourcesForNextCall.indexOf(vertex), 1);
      // get the node's children to decrement their in-degrees
      graph[vertex].forEach((child) => {
        // get the node's children to decrement their in-degrees
        inDegree[child]--;
        if (inDegree[child] === 0) {
          sourcesForNextCall.push(child);
        }
      });

      // recursive call to print other orderings from the remaining (and new) sources
      print_all_topological_sorts(
        graph,
        inDegree,
        sourcesForNextCall,
        sortedOrder
      );

      // backtrack, remove the vertex from the sorted order and put all of its children back to consider
      // the next source instead of the current vertex
      sortedOrder.splice(sortedOrder.indexOf(vertex), 1);
      for (p = 0; p < graph[vertex].length; p++) {
        inDegree[graph[vertex][p]] += 1;
      }
    }
  }

  // if sortedOrder doesn't contain all tasks, either we've a cyclic dependency between tasks, or
  // we have not processed all the tasks in this recursive call
  if (sortedOrder.length === inDegree.length) {
    console.log(sortedOrder);
  }
}
```

# alient dictionary

```javascript
// running time  O(V+E), where ‘V’ is the total number of different characters and ‘E and each pair give us one rule so O(N), so O(v+N) and space O(V+N)
function find_order(words) {
  if (words.length === 0) {
    return "";
  }

  // a. Initialize the graph
  const inDegree = {}; // count of incoming edges
  const graph = {}; // adjacency list graph

  // for each letter in each word create an entry in graph and inDegree
  words.forEach((word) => {
    for (let i = 0; i < word.length; i++) {
      inDegree[word[i]] = 0;
      graph[word[i]] = [];
    }
  });

  // build the graph
  for (let i = 0; i < words.length - 1; i++) {
    let w1 = words[i];
    let w2 = words[i + 1];
    for (let j = 0; j < Math.min(w1.length, w2.length); j++) {
      let parent = w1[j];
      let child = w2[j];
      if (parent != child) {
        graph[parent].push(child);
        inDegree[child]++;
        break; // only the first dif char between the two words help us finds the order
      }
    }
  }

  //find sources
  let queueSources = [];
  let chars = Object.keys(inDegree);
  for (let i = 0; i < inDegree.length; i++) {
    if (inDegree[i] === 0) {
      queueSources.push(i);
    }
  }
  let sortedOutput = [];
  // bfs source with leveling
  while (queueSources.length > 0) {
    let levellength = queueSources.length;
    for (let i = 0; i < queueSources.length; i++) {
      let v = queueSources.shift();
      sortedOutput.push(v);
      let children = graph[v];
      children.forEach((child) => {
        inDegree[child]--;
        if (inDegree[child] === 0) {
          queueSources.push(child);
        }
      });
    }
  }
  if (sortedOutput.length != chars.length) {
    return "";
  }
  return sortedOrder.join("");
}
```
