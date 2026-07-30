## Introduction of BFS

The BFS is graph traversal technique where each node is traversed in level wise order. It uses queue data structure to stores the node.

## Steps to do BFS traversal :
1. BFS uses a queue to track which nodes to visit next and a visited set to avoid revisiting nodes.
2. Enqueue the starting node, mark it visited, then repeatedly dequeue a node, process it, and enqueue its unvisited neighbors until the queue is empty.
   
<img width="1001" height="299" alt="image" src="https://github.com/user-attachments/assets/f308a9c1-cc66-44fa-a649-0ffbb5532927" />

## Common Application of BFS

#### 1. Finding shortest path between nodes - 
    The first time a node is discovered, its distance from the root node will always be the shortest.

#### 2. Flood Fill
    BFS solves multi-source spread problems such as flood fill and computing the distance from each grid cell to its nearest source, since enqueuing all sources at once lets the frontier
    expand outward evenly and assign each cell the distance to the closest source.

#### 3. Web-Crawling
     BFS powers web crawling by treating pages as vertices and links as edges, so it can discover pages close to a seed URL before following links that lead further away.

#### 4. Social Network Analysis
      Finding friends within a given degree k (e.g., “friends of friends” up to 3 degrees).

#### 5. Cycle Detection
        Detecting cycles in undirected or directed graphs.

#### 6. Path Existence Checks
        Determining if a path exists between two vertices.


   

## Variation of BFS

### 1. Standard BFS - 
      This BFS uses a queue to explore nodes by starting with source node insert in queue and mark it visited then add its neighbouring nodes in queue and remove souce node. 
      Follow this process until all node get visited and queue is empty.
      
### 2. BFS from Borders
    Here identify all the grid boundaries , enqueue the all nodes of boundaries in queue 
    then mark them visited and run BFS for all the nodes.
    The direction will be from outward to inward in grid.
    Ex - Problems like Surrounded Regions or Pacific Atlantic Water Flow can be solved by marking all border-connected nodes first, then processing the interior.

### 3. MultiSource BFS
    In standard BFS, you start from a single source node, add it to a queue, and explore its neighbors level by level. 
    The FIFO queue guarantees that all nodes at distance d are processed before any node at distance d+1. This gives you shortest paths from that single source.

    Multi-source BFS is the same algorithm, but instead of putting one node into the queue at the start, you put all source nodes into the queue at level 0. From there, 
    BFS expands outward from all sources simultaneously, level by level. The result is the shortest distance from each cell to its nearest source, not to a specific source.
