图的最小生成树问题

基于贪心算法

```java
class Prim {
    private static final int INF = Integer.MAX_VALUE;

    public static int[] prim(List<int[]>[] edges) {
        int[] parent = new int[edges.length];
        parent[0] = -1;
        Set<Integer> visited = new HashSet<>();
        visited.add(0);
        while (visited.size() < edges.length) {
            int from = 0;
            int to = 0;
            int widget = INF;
            for (Integer index : visited) {
                for (int[] a : edges[index]) {
                    if (!visited.contains(a[0])) {
                        if (widget > a[1]) {
                            from = index;
                            to = a[0];
                            widget = a[1];
                        }
                    }
                }
            }
            parent[to] = from;
            visited.add(to);
        }
        return parent;
    }
}
```

