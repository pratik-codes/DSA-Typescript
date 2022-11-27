# Graph

Terminology of Graphs
This is not an exhaustive list of terms, but it is the terms that we may end up using today.

Graph Terms
cycle: When you start at Node(x), follow the links, and end back at Node(x)
acyclic: A graph that contains no cycles
connected: When every node has a path to another node
directed: When there is a direction to the connections. Think Twitter
undirected: !directed. Think Facebook (i haven't been on in 10 years, it may have changed)
weighted: The edges have a weight associated with them. Think Maps
dag: Directed, acyclic graph.

Implementation Terms
node: a point or vertex on the graph
edge: the connection betxit two nodes

Big O
BigO is commonly stated in terms of V and E where V stands for vertices and E stands for edges

So O(V \* E) means that we will check every vertex, and on every vertex we check every edge

<br />

### DFS IN GRAPH

```typescript
class DFSGraph {
  public adj: Array<number>[];
  public size: number;
}

class DFS {
  public dfs(G: DFSGraph, startVert: number) {
    let visited: boolean[] = Array<boolean>();
    // Pre-populate array:
    for (let i = 0; i < G.size; i++) {
      visited.push(false);
    }

    let s: number[] = new Array();

    visited[startVert] = true;

    s.push(startVert);

    while (s.length > 0) {
      const v = s.pop();
      for (let adjV of G.adj[v]) {
        if (!visited[adjV]) {
          visited[adjV] = true;
          s.push(adjV);
        }
      }
    }
  }

  public dfsRecursive(G: DFSGraph, startVert: number) {
    let visited: boolean[] = Array<boolean>();
    // Pre-populate array:
    for (let i = 0; i < G.size; i++) {
      visited.push(false);
    }
    this.dfsAux(G, startVert, visited);
  }

  private dfsAux(G: DFSGraph, v: number, visited: boolean[]) {
    visited[v] = true;
    for (let adjV of G.adj[v]) {
      if (!visited[adjV]) {
        // this.foo(); // Something can happen before the visit.
        this.dfsAux(G, adjV, visited);
        // this.bar(); // Something can happen after the visit.
      }
    }
  }
}
```

### BFS IN GRAPH

```typescript
// This is a basic representation of a graph
class BFSGraph {
  public adj: Array<number>[];
  public size: number;
}
```

```typescript
class BFS {
  public bfs(G: BFSGraph, startVert: number) {
    let visited: boolean[] = Array<boolean>();
    // Pre-populate array:
    for (let i = 0; i < G.size; i++) {
      visited.push(false);
    }

    // Use an array as our queue representation:
    let q: number[] = new Array<number>();

    visited[startVert] = true;

    q.push(startVert);

    while (q.length > 0) {
      const v = q.shift();
      for (let adjV of G.adj[v]) {
        if (!visited[adjV]) {
          visited[adjV] = true;
          q.push(adjV);
        }
      }
    }
  }
}
```

### Dijkstra's shortest path

Used to calculate the shortest path form one node to another.

```typescript
class NodeVertex {
  nameOfVertex: string;
  weight: number;
}

class Vertex {
  name: string;
  nodes: NodeVertex[];
  weight: number;

  constructor(theName: string, theNodes: NodeVertex[], theWeight: number) {
    this.name = theName;
    this.nodes = theNodes;
    this.weight = theWeight;
  }
}

class Dijkstra {
  vertices: any;
  constructor() {
    this.vertices = {};
  }

  addVertex(vertex: Vertex): void {
    this.vertices[vertex.name] = vertex;
  }

  findPointsOfShortestWay(
    start: string,
    finish: string,
    weight: number
  ): string[] {
    let nextVertex: string = finish;
    let arrayWithVertex: string[] = [];
    while (nextVertex !== start) {
      let minWeigth: number = Number.MAX_VALUE;
      let minVertex: string = "";
      for (let i of this.vertices[nextVertex].nodes) {
        if (i.weight + this.vertices[i.nameOfVertex].weight < minWeigth) {
          minWeigth = this.vertices[i.nameOfVertex].weight;
          minVertex = i.nameOfVertex;
        }
      }
      arrayWithVertex.push(minVertex);
      nextVertex = minVertex;
    }
    return arrayWithVertex;
  }

  findShortestWay(start: string, finish: string): string[] {
    let nodes: any = {};
    let visitedVertex: string[] = [];

    for (let i in this.vertices) {
      if (this.vertices[i].name === start) {
        this.vertices[i].weight = 0;
      } else {
        this.vertices[i].weight = Number.MAX_VALUE;
      }
      nodes[this.vertices[i].name] = this.vertices[i].weight;
    }

    while (Object.keys(nodes).length !== 0) {
      let sortedVisitedByWeight: string[] = Object.keys(nodes).sort(
        (a, b) => this.vertices[a].weight - this.vertices[b].weight
      );
      let currentVertex: Vertex = this.vertices[sortedVisitedByWeight[0]];
      for (let j of currentVertex.nodes) {
        const calculateWeight: number = currentVertex.weight + j.weight;
        if (calculateWeight < this.vertices[j.nameOfVertex].weight) {
          this.vertices[j.nameOfVertex].weight = calculateWeight;
        }
      }
      delete nodes[sortedVisitedByWeight[0]];
    }
    const finishWeight: number = this.vertices[finish].weight;
    let arrayWithVertex: string[] = this.findPointsOfShortestWay(
      start,
      finish,
      finishWeight
    ).reverse();
    arrayWithVertex.push(finish, finishWeight.toString());
    return arrayWithVertex;
  }
}

// Now lets test it

let dijkstra = new Dijkstra();
dijkstra.addVertex(
  new Vertex(
    "A",
    [
      { nameOfVertex: "C", weight: 3 },
      { nameOfVertex: "E", weight: 7 },
      { nameOfVertex: "B", weight: 4 },
    ],
    1
  )
);
dijkstra.addVertex(
  new Vertex(
    "B",
    [
      { nameOfVertex: "A", weight: 4 },
      { nameOfVertex: "C", weight: 6 },
      { nameOfVertex: "D", weight: 5 },
    ],
    1
  )
);
dijkstra.addVertex(
  new Vertex(
    "C",
    [
      { nameOfVertex: "A", weight: 3 },
      { nameOfVertex: "B", weight: 6 },
      { nameOfVertex: "E", weight: 8 },
      { nameOfVertex: "D", weight: 11 },
    ],
    1
  )
);
dijkstra.addVertex(
  new Vertex(
    "D",
    [
      { nameOfVertex: "B", weight: 5 },
      { nameOfVertex: "C", weight: 11 },
      { nameOfVertex: "E", weight: 2 },
      { nameOfVertex: "F", weight: 2 },
    ],
    1
  )
);
dijkstra.addVertex(
  new Vertex(
    "E",
    [
      { nameOfVertex: "A", weight: 7 },
      { nameOfVertex: "C", weight: 8 },
      { nameOfVertex: "D", weight: 2 },
      { nameOfVertex: "G", weight: 5 },
    ],
    1
  )
);
dijkstra.addVertex(
  new Vertex(
    "F",
    [
      { nameOfVertex: "D", weight: 2 },
      { nameOfVertex: "G", weight: 3 },
    ],
    1
  )
);
dijkstra.addVertex(
  new Vertex(
    "G",
    [
      { nameOfVertex: "D", weight: 10 },
      { nameOfVertex: "E", weight: 5 },
      { nameOfVertex: "F", weight: 3 },
    ],
    1
  )
);
console.log(dijkstra.findShortestWay("A", "F"));
```
