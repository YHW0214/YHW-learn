多源最短路径问题

```java
class Floyd {
    private static final int INF = Integer.MAX_VALUE;

    /**
     * 
     * @param matrix 邻接矩阵
     * 
     *               example
     * 
     *               [[0,1,INF], [1,0,1], [INF,1,0]]
     */
    public static void floyd(int[][] matrix) {
        for (int k = 0; k < matrix.length; k++) {
            for (int i = 0; i < matrix.length; i++) {
                for (int j = 0; j < matrix.length; j++) {
                    if (matrix[i][k] == INF || matrix[k][j] == INF)
                        continue;
                    matrix[i][j] = Math.min(matrix[i][j], matrix[i][k] + matrix[k][j]);
                }
            }
        }
    }
}
```

