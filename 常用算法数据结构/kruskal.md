图的最小生成树问题

基于并查集实现

参考代码（待验证）

```java
class Kruakal {
    static class Edge {
        int from;
        int to;
        int weidget;

        Edge(int from, int to, int weidget) {
            this.from = from;
            this.to = to;
            this.weidget = weidget;
        }

    }

    static class DSU {
        int[] parent;

        DSU(int n) {
            this.parent = new int[n];
            for (int i = 0; i < n; i++)
                parent[i] = i;
        }

        int find(int x) {
            if (parent[x] != x)
                parent[x] = find(parent[x]);
            return parent[x];
        }

        void union(int x, int y) {
            int rootX = find(x);
            int rootY = find(y);
            if (rootX != rootY) {
                parent[rootY] = rootX;
            }
        }
    }

    public static List<Edge> kruskal(int n, Edge[] edges) {
        List<Edge> list = new ArrayList<>();
        DSU dsu = new DSU(n);
        Arrays.sort(edges, (edge1, edge2) -> edge1.weidget - edge2.weidget);
        for (Edge edge : edges) {
            if (dsu.find(edge.from) != dsu.find(edge.to)) {
                dsu.union(edge.from, edge.to);
                list.add(edge);
                if (list.size() >= n - 1)
                    break;
            }
        }
        return list;
    }
}
```

