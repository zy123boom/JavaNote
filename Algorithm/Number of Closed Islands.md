### Leetcode.1254 Number of Closed Islands

#### 题目描述

有一个二维矩阵 grid ，每个位置要么是陆地（记号为 0 ）要么是水域（记号为 1 ）。
我们从一块陆地出发，每次可以往上下左右 4 个方向相邻区域走，能走到的所有陆地区域，我们将其称为一座「岛屿」。
如果一座岛屿 完全 由水域包围，即陆地边缘上下左右所有相邻区域都是水域，那么我们将其称为 「封闭岛屿」。
请返回封闭岛屿的数目。
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/number-of-closed-islands

考察点：DFS/BFS

```java

public int closedIsland(int[][] grid) {
    int res = 0;
    for (int i = 0; i < grid.length; i++) {
        for (int j = 0; j < grid[0].length; j++) {
            if (grid[i][j] == 0) {
                res += dfs(grid, i, j);
            }
        }
    }
    return res;
}

public int dfs(int[][] grid, int row, int column) {
    /*
       从当前陆地开始出发，如果能走出边界就说明该陆地所在岛屿不是封闭岛屿，
       返回封闭岛屿的个数0
     */

    if (row < 0 || row >= grid.length || column < 0 || column >= grid[0].length) {
        return 0;
    }

    /*
        如果碰到水域(值为1的点)就返回封闭岛屿个数1，表示该岛屿可能是一个封闭
        岛屿
     */
    if (grid[row][column] == 1) {
        return 1;
    }

    // 如果碰到陆地(0点)就继续向该陆地四个方向遍历，同时将该陆地标记为1
    grid[row][column] = 1;
    int[] vRow = {0, 1, 0, -1};
    int[] vColumn = {1, 0, -1, 0};
    int res = 1;
    for (int i = 0; i < 4; i++) {
        res = Math.min(res, dfs(grid, row + vRow[i], column + vColumn[i]));
    }
    return res;
}

```
