# Graph BFS

```javascript
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

  bfsTraversal(source) {
    let queue = [source];
    while (queue.length > 0) {
      let levelSize = queue.length;
      for (let i = 0; i < levelSize; i++) {
        let curr = queue.shift();
        if (this.visited[curr]) {
          continue;
        }

        this.result.push(curr);
        this.visited[curr] = true;
        for (let j = 0; j < this.edges[curr].length; j++) {
          let adj = this.edges[curr][j];
          if (this.visited[adj]) {
            continue;
          }
          queue.push(adj);
        }
      }
    }
    console.log(this.result);
  }
}
var graph = new Graph(11);

graph.addEdge(0, 1);
graph.addEdge(1, 4);
graph.addEdge(0, 2);
graph.addEdge(0, 3);
graph.addEdge(1, 5);
graph.addEdge(5, 7);
graph.addEdge(7, 9);
graph.addEdge(2, 6);
graph.addEdge(5, 8);
graph.addEdge(2, 5);
graph.addEdge(7, 10);
graph.addEdge(4, 7);
graph.addEdge(6, 8);

graph.bfsTraversal(0);
```
