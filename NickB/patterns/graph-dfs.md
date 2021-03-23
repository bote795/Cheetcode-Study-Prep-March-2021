# Graph dfs

```javascript
// time O(V+E) and space O(V)
class Graph {
  constructor(noOfVertices) {
    this.V = noOfVertices;
    this.edges = new Array(this.V).fill(null).map(() => []);
    this.visited = new Array(this.V).fill(false);
    this.result = [];
  }
  addEdge(src, dest) {
    this.edges[src].push(dest);
  }

  dfsTraversal(v) {
    this.visited[v] = true;
    this.result.push(v);
    for (let i = 0; i < this.edges[v].length; i++) {
      if (!this.visited[this.edges[v][i]]) {
        this.dfsTraversal(this.edges[v][i]);
      }
    }
  }
  dfsIterative(s) {
    this.visited.fill(false);
    this.result = [];
    let stack = [s];

    while (stack.length != 0) {
      s = stack.pop();

      if (!this.visited[s]) {
        this.result.push(s);
        this.visited[s] = true;
      }

      for (let i = 0; i < this.edges[s].length; i++) {
        let adj = this.edges[s][i];
        if (!this.visited[adj]) stack.push(adj);
      }
    }
    console.log(this.result);
  }
}
var graph = new Graph(12);

graph.addEdge(0, 1);
graph.addEdge(0, 2);
graph.addEdge(0, 4);
graph.addEdge(1, 3);
graph.addEdge(1, 9);
graph.addEdge(1, 4);
graph.addEdge(1, 6);
graph.addEdge(2, 5);
graph.addEdge(3, 6);
graph.addEdge(4, 1);
graph.addEdge(4, 7);
graph.addEdge(5, 8);
graph.addEdge(7, 10);
graph.addEdge(10, 11);

graph.dfsTraversal(0);
console.log(graph.result);

graph.dfsIterative(0);
```
