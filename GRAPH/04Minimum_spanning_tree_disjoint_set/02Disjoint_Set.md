# disjoint Set implementation
````java
class DisjointSet {
    int[] parent;
    int[] size;
    int[] rank;

    // Constructor
    DisjointSet(int n) {
        parent = new int[n + 1]; // 1-based indexing
        size = new int[n + 1];
        rank = new int[n + 1];

        for (int i = 0; i <= n; i++) {
            parent[i] = i;
            size[i] = 1;
            rank[i] = 0;
        }
    }

    // Find Ultimate Parent (Path Compression)
    int ulPar(int node) {
        if (parent[node] == node)
            return node;
        return parent[node] = ulPar(parent[node]);
    }

    // Union by Size
    void unionBySize(int u, int v) {
        int pu = ulPar(u);
        int pv = ulPar(v);

        if (pu == pv) return;

        if (size[pu] < size[pv]) {
            parent[pu] = pv;
            size[pv] += size[pu];
        } else {
            parent[pv] = pu;
            size[pu] += size[pv];
        }
    }

    // Union by Rank
    void unionByRank(int u, int v) {
        int pu = ulPar(u);
        int pv = ulPar(v);

        if (pu == pv) return;

        if (rank[pu] < rank[pv]) {
            parent[pu] = pv;
        } else if (rank[pv] < rank[pu]) {
            parent[pv] = pu;
        } else {
            parent[pv] = pu;
            rank[pu]++;
        }
    }
}

````