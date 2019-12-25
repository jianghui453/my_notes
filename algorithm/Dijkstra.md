# Dijkstra (迪杰斯特拉 最短路径树 算法)

Dijkstra's algorithm (or Dijkstra's Shortest Path First algorithm, SPF algorithm)[1] is an algorithm for finding the shortest paths between nodes in a graph, which may represent, for example, road networks. It was conceived by computer scientist Edsger W. Dijkstra in 1956 and published three years later.

## Applications

- 地图查找最短路径
- IP路由查找最短路径
- 电话线路查找最短路径

## Implementation

1. Create a set sptSet (shortest path tree set) that keeps track of vertices included in shortest path tree, i.e., whose minimum distance from source is calculated and finalized. Initially, this set is empty.
1. Assign a distance value to all vertices in the input graph. Initialize all distance values as INFINITE. Assign distance value as 0 for the source vertex so that it is picked first.
1. While sptSet doesn’t include all vertices
	1. Pick a vertex u which is not there in sptSet and has minimum distance value.
	1. Include u to sptSet.
	1. Update distance value of all adjacent vertices of u. To update the distance values, iterate through all adjacent vertices. For every adjacent vertex v, if sum of distance value of u (from source) and weight of edge u-v, is less than the distance value of v, then update the distance value of v.

### Example

Let us understand with the following example:

![img](../Images/Algorithm/Dijkstra/1.jpg)

The set sptSet is initially empty and distances assigned to vertices are {0, INF, INF, INF, INF, INF, INF, INF} where INF indicates infinite. Now pick the vertex with minimum distance value. The vertex 0 is picked, include it in sptSet. So sptSet becomes {0}. After including 0 to sptSet, update distance values of its adjacent vertices. Adjacent vertices of 0 are 1 and 7. The distance values of 1 and 7 are updated as 4 and 8. Following subgraph shows vertices and their distance values, only the vertices with finite distance values are shown. The vertices included in SPT are shown in green colour.

![img](../Images/Algorithm/Dijkstra/2.jpg)

Pick the vertex with minimum distance value and not already included in SPT (not in sptSET). The vertex 1 is picked and added to sptSet. So sptSet now becomes {0, 1}. Update the distance values of adjacent vertices of 1. The distance value of vertex 2 becomes 12.

![img](../Images/Algorithm/Dijkstra/3.jpg)

Pick the vertex with minimum distance value and not already included in SPT (not in sptSET). Vertex 7 is picked. So sptSet now becomes {0, 1, 7}. Update the distance values of adjacent vertices of 7. The distance value of vertex 6 and 8 becomes finite (15 and 9 respectively).

![img](../Images/Algorithm/Dijkstra/4.jpg)

Pick the vertex with minimum distance value and not already included in SPT (not in sptSET). Vertex 6 is picked. So sptSet now becomes {0, 1, 7, 6}. Update the distance values of adjacent vertices of 6. The distance value of vertex 5 and 8 are updated.

![img](../Images/Algorithm/Dijkstra/5.jpg)

We repeat the above steps until sptSet does include all vertices of given graph. Finally, we get the following Shortest Path Tree (SPT).

![img](../Images/Algorithm/Dijkstra/6.jpg)

### Code

```
func dijkstra(V int, graph [][]int) []int {
	spt := make([]int, V)

	for flag := true; flag; {
		var (
			idx int
			min int
		)

		flag = false
		for i := 0; i < V; i++ {
			if i != 0 && spt[i] == 0 {
				flag = true
				continue
			}

			for j := 1; j < V; j++ {
				if spt[j] == 0 && graph[i][j] > 0 && (min == 0 || spt[i]+graph[i][j] < min) {
					min = spt[i]+graph[i][j]
					idx = j
				}
			}
		}

		spt[idx] = min
	}

	return spt
}
```

## Reference

1. [GeeksforGeeks: Dijkstra’s shortest path algorithm | Greedy Algo-7](https://www.geeksforgeeks.org/dijkstras-shortest-path-algorithm-greedy-algo-7/)
1. [Wikipedia: Dijkstra's algorithm](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm)
