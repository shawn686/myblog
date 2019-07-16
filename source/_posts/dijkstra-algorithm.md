---
title: Dijkstra's Algorithm
date: 2019-07-15 10:51:58
tags:
- java
- datastructure
- fundamental
- graph
categories:
- [datastructure, graph]
---
## Queue and PriorityQueue Implementation of Dijkstra's Algorithm
Given a weighted graph, starting from a source node, return the shortest path to other nodes.

![A Directed Graph](https://user-images.githubusercontent.com/24547983/61260010-a047f680-a74a-11e9-8895-9d8809534e57.png)


``` java
import java.util.*;

class DijkstraAlgorithm {
    static int[] shortestPath(Map<Integer, List<int[]>> graph, int srcNode) {
        int[] dist = new int[graph.size()];
        Arrays.fill(dist, Integer.MAX_VALUE);
        Deque<int[]> dq = new ArrayDeque<>();
        dq.addLast(new int[]{srcNode, 0});
        while (!dq.isEmpty()) {
            int currNode = dq.peekFirst()[0];
            int currDist = dq.peekFirst()[1];
            dq.removeFirst();
            if (currDist < dist[currNode]) {
                dist[currNode] = currDist;
                for (int[] adj : graph.get(currNode)) {
                    dq.addLast(new int[]{adj[0], currDist + adj[1]});
                }
            }
        }
        return dist;
    }

    static int[] shortestPathPQ(Map<Integer, List<int[]>> graph, int srcNode) {
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[1] - b[1]);
        int[] dist = new int[graph.size()];
        Arrays.fill(dist, Integer.MAX_VALUE);
        pq.add(new int[]{srcNode, 0});
        while (!pq.isEmpty()) {
            int currNode = pq.peek()[0];
            int currDist = pq.peek()[1];
            pq.remove();
            if (dist[currNode] == Integer.MAX_VALUE) {
                dist[currNode] = currDist;
                for (int[] adj : graph.get(currNode)) {
                    pq.add(new int[]{adj[0], currDist + adj[1]});
                }
            }
        }
        return dist;
    }

    public static void main(String[] args) {
        Map<Integer, List<int[]>> graph = new HashMap<>();
        List<int[]> tmp = new ArrayList<>();
        tmp.add(new int[]{1, 4}); // B, 4
        tmp.add(new int[]{2, 2}); // C, 2
        graph.put(0, tmp); // add A's adj
        tmp = new ArrayList<>();
        tmp.add(new int[]{2, 3}); // C, 3
        tmp.add(new int[]{3, 2}); // D, 2
        tmp.add(new int[]{4, 3}); // E, 3
        graph.put(1, tmp); // add B's adj
        tmp = new ArrayList<>();
        tmp.add(new int[]{1, 1}); // B, 1
        tmp.add(new int[]{3, 4}); // D, 4
        tmp.add(new int[]{4, 5}); // E, 5
        graph.put(2, tmp); // add C's adj
        tmp = new ArrayList<>();
        graph.put(3, tmp); // add D's adj
        tmp = new ArrayList<>();
        tmp.add(new int[]{3, 1}); // D, 1
        graph.put(4, tmp); // add E's adj
        System.out.println(Arrays.toString(shortestPath(graph, 0)));
        System.out.println(Arrays.toString(shortestPathPQ(graph, 0)));
    }
}
```
