---
title: Union / Find
date: 2019-06-23 17:27:16
tags:
- java
- datastructure
- fundamental
- graph
categories:
- [datastructure, graph]
---
## An Union / Find Implementation and An Application Example
This article is partialy quoted from Huahua's [blog](https://zxi.mytechroad.com/blog/data-structure/sp1-union-find-set/).
Typical LeetCode problems: 547, 684, 737

![image](https://user-images.githubusercontent.com/24547983/59987008-3040c780-9607-11e9-8206-de674e8b1213.png)
![image](https://user-images.githubusercontent.com/24547983/59987022-3c2c8980-9607-11e9-853a-91381f1a1d3c.png)
![image](https://user-images.githubusercontent.com/24547983/59987032-43ec2e00-9607-11e9-91ad-6663fd03b8d5.png)
![Pseudo Code](https://user-images.githubusercontent.com/24547983/59987070-61b99300-9607-11e9-9c6a-5e649f04567d.png)

``` java
class UnionFindSet {
    private int[] parents_;
    private int[] ranks_;
    public UnionFindSet(int n) {
        parents_ = new int[n + 1];
        ranks_ = new int[n + 1];
        for (int i = 0; i < ranks_.length; i++) {
            parents_[i] = i;
            ranks_[i] = 1;
        }
    }

    public boolean union(int u, int v) {
        int pu = find(u);
        int pv = find(v);
        if (pv == pu) return false;
        if (ranks_[pu] > ranks_[pv]) {
            parents_[pv] = pu;
        } else if (ranks_[pu] < ranks_[pv]) {
            parents_[pu] = pv;
        } else {
            parents_[pu] = pv;
            ranks_[pv]++;
        }
        return true;
    }

    public int find(int u) {
        if (parents_[u] != u) {
            parents_[u] = find(parents_[u]);
        }
        return parents_[u];
    }

    public static void main(String[] args) {
        UnionFindSet uf = new UnionFindSet(8);
        uf.union(3, 1);
        uf.union(8, 5);
        uf.union(5, 3);
        uf.union(7, 5);
        uf.union(4, 2);
        uf.union(1, 2);
    }
}

// LeetCode 547. Friend Circles
class Solution {
    static class UnionFindSet {
        private int[] parents_;
        private int[] ranks_;
        public UnionFindSet(int n) {
            parents_ = new int[n + 1];
            ranks_ = new int[n + 1];
            for (int i = 0; i < ranks_.length; i++) {
                parents_[i] = i;
                ranks_[i] = 1;
            }
        }

        public boolean union(int u, int v) {
            int pu = find(u);
            int pv = find(v);
            if (pv == pu) return false;
            if (ranks_[pu] > ranks_[pv]) {
                parents_[pv] = pu;
            } else if (ranks_[pu] < ranks_[pv]) {
                parents_[pu] = pv;
            } else {
                parents_[pu] = pv;
                ranks_[pv]++;
            }
            return true;
        }

        public int find(int u) {
            if (parents_[u] != u) {
                parents_[u] = find(parents_[u]);
            }
            return parents_[u];
        }

        public static void main(String[] args) {
            UnionFindSet uf = new UnionFindSet(8);
            uf.union(3, 1);
            uf.union(8, 5);
            uf.union(5, 3);
            uf.union(7, 5);
            uf.union(4, 2);
            uf.union(1, 2);
        }
    }
    // What we do here is to find how many clusters do we have.
    public int findCircleNum(int[][] M) {
        Set<Integer> clusters = new HashSet<>();
        UnionFindSet uf = new UnionFindSet(M.length);
        for (int i = 0; i < M.length; i++) {
            for (int j = i + 1; j < M.length; j++) {
                if (M[i][j] == 1) {
                    uf.union(i, j);
                }
            }
        }
        for (int i = 0; i < M.length; i++) {
            clusters.add(uf.find(i));
        }
        return clusters.size();
    }
}
```
