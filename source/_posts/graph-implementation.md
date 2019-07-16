---
title: Topological Ordering Iterative / DFS
date: 2019-06-03 09:05:06
tags:
- java
- datastructure
- fundamental
- graph
categories:
- [datastructure, graph]
---
## Topological Ordering Iterative / DFS

``` java

import java.util.*;

public class Graph {
    static class Node {
        String name;
        Node(String name) {
            this.name = name;
        }
    }
    private Map<String, Node> nodes;
    private Map<String, List<String>> edges;
    private Graph() {
        this.nodes = new HashMap<>();
        this.edges = new HashMap<>();
    }
    public void addNode(String name, Node node) {
        this.nodes.put(name, node);
    }

    public Map<String, Node> getNodes() {
        return this.nodes;
    }
    public void addEdge(String fromNode, String toNode) {
        List<String> toNodes = this.edges.getOrDefault(fromNode, new ArrayList<>());
        toNodes.add(toNode);
        this.edges.put(fromNode, toNodes);
    }
    public Map<String, List<String>> getEdges() {
        return this.edges;
    }
    private List<String> topologicalOrdering() {
        Map<String, Integer> inDegree = new HashMap<>();
        for (String fromNode : this.edges.keySet()) {
            for (String toNode : this.edges.get(fromNode)) {
                inDegree.put(toNode, inDegree.getOrDefault(toNode, 0) + 1);
            }
        }
        for (String node : this.nodes.keySet()) {
            if (!inDegree.containsKey(node)) {
                inDegree.put(node, 0);
            }
        }
        Queue<String> q = new LinkedList<>();
        Set<String> visited = new HashSet<>();
        for (String key : inDegree.keySet()) {
            if (inDegree.get(key) == 0) {
                visited.add(key);
                q.add(key);
            }
        }
        List<String> ans = new ArrayList<>();
        while (!q.isEmpty()) {
            String name = q.remove();
            ans.add(name);
            visited.add(name);
            for (String toNode : this.getEdges().getOrDefault(name, new ArrayList<>())) {
                if (visited.contains(toNode)) continue;
                inDegree.put(toNode, inDegree.get(toNode) - 1);
                if (inDegree.get(toNode) == 0) {
                    q.add(toNode);
                    visited.add(toNode);
                }
            }
        }
        if (ans.size() != this.nodes.keySet().size()) System.out.println("This is not a DAG.");
        return ans;
    }

    // FROM WIKI
    // L ← Empty list that will contain the sorted nodes
    // while exists nodes without a permanent mark do
    //     select an unmarked node n
    //     visit(n)

    // function visit(node n)
    //     if n has a permanent mark then return
    //     if n has a temporary mark then stop   (not a DAG)
    //     mark n with a temporary mark
    //     for each node m with an edge from n to m do
    //         visit(m)
    //     remove temporary mark from n
    //     mark n with a permanent mark
    //     add n to head of L

    public List<String> topologicalOrderingDFS() {
        Map<String, String> visited = new HashMap<>();
        Stack<String> stack = new Stack<>();
        for (String node : this.nodes.keySet()) {
            if (visited.containsKey(node)) continue;
            if (!dfs(node, visited, stack)) {
                System.out.println("This is not a DAG.");
                return null;
            }
        }
        List<String> ans = new ArrayList<>();
        while (!stack.isEmpty()) {
            ans.add(stack.pop());
        }
        return ans;
    }

    boolean dfs(String node, Map<String, String> visited, Stack<String> stack) {
        if (visited.getOrDefault(node, "").equals("perm")) return true;
        if (visited.getOrDefault(node, "").equals("temp")) return false;
        visited.put(node, "temp");
//        System.out.println(node.inDegree.keySet());
        for (String nextNode : this.edges.getOrDefault(node, new ArrayList<>())) {
            if (!dfs(nextNode, visited, stack)) return false;
        }
        visited.put(node, "perm");
        stack.push(node);
        return true;
    }
    // a -> b -> c -> d
    //      b -> e -> f -> h
    //           c -> f
    // a -----------> f
    //           e -> i
    // g
    public static void main(String[] args) {
        Graph g = new Graph();
        Node n_a = new Node("a");
        Node n_b = new Node("b");
        g.addNode(n_a.name, n_a);
        g.addNode(n_b.name, n_b);
        g.addEdge(n_a.name, n_b.name);
        Node n_c = new Node("c");
        g.addNode(n_c.name, n_c);
        g.addEdge(n_b.name, n_c.name);
        Node n_d = new Node("d");
        g.addNode(n_d.name, n_d);
        g.addEdge(n_c.name, n_d.name);
        Node n_e = new Node("e");
        g.addNode(n_e.name, n_e);
        g.addEdge(n_b.name, n_e.name);
        Node n_f = new Node("f");
        g.addNode(n_f.name, n_f);
        g.addEdge(n_e.name, n_f.name);
        g.addEdge(n_a.name, n_f.name);
        g.addEdge(n_c.name, n_f.name);
        Node n_g = new Node("g");
        g.addNode(n_g.name, n_g);
        Node n_h = new Node("h");
        g.addNode(n_h.name, n_h);
        g.addEdge(n_f.name, n_h.name);
        g.addEdge(n_f.name, n_d.name);
        Node n_i = new Node("i");
        g.addNode(n_i.name, n_i);
        g.addEdge(n_e.name, n_i.name);
//        g.addEdge(n_i.name, n_i.name);
//        g.addEdge(n_i.name, n_e.name);
        for (String fromNode : g.nodes.keySet()) {
            System.out.println(fromNode + ":");
            for (String toNode : g.getEdges().getOrDefault(fromNode, new ArrayList<>())) {
                System.out.println("  " + fromNode + " -> " + toNode);
            }
        }
        System.out.println(g.topologicalOrdering());
        System.out.println(g.topologicalOrderingDFS());
    }
}
```