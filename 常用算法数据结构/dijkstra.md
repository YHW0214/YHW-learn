单源最短路径问题

参考代码（待验证）

```java
class Dijkstra {
    private static final int INF = Integer.MAX_VALUE;

    private int[] dijkstra(int n, int start, int end, List<int[]>[] edges) {
        int[] dis = new int[n];
        Arrays.fill(dis, INF);
        boolean[] visited = new boolean[n];
        PriorityQueue<int[]> queue = new PriorityQueue<>((a, b) -> a[1] - b[1]);
        queue.add(new int[] { start, 0 });
        dis[start] = 0;
        while (!queue.isEmpty()) {
            int[] a = queue.poll();
            if (visited[a[0]])
                continue;
            if (dis[a[0]] < a[1])
                continue;
            visited[a[0]] = true;
            for (int[] tmp : edges[a[0]]) {
                if (visited[tmp[0]])
                    continue;
                if (dis[tmp[0]] > a[1] + tmp[1]) {
                    dis[tmp[0]] = a[1] + tmp[1];
                    queue.add(new int[] { tmp[0], dis[tmp[0]] });
                }
            }
        }
        return dis;
    }
}
```

