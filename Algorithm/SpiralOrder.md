## Leetcode54. 螺旋矩阵
#### 示例
    [                       
      [1, 2, 3],
      [4, 5, 6],       =>   [1,2,3,6,9,8,7,5,4]    
      [7, 8, 9]
    ]
#### 方法
    首先设置四个方位标志left, right, top, bottom,则可以知道以下四种信息：
    top行：[top][i](i是要遍历的）
    right列 [i][right]
    bottom行 [bottom][i]
    left列 [i][left]
    然后进行绕圈遍历，遍历的条件是：left<right && top<bottom
    最后会产生四种情况：
    第一种：全部遍历完
    第二种：中间剩了一个数字未遍历，加上即可
    第三种：中间剩了一行未遍历：top==bottom 遍历列[left,right]
    第四种：中间剩了一列未遍历：left==right 遍历列[top,bottom]
#### 代码
```java
public List<Integer> spiralOrder(int[][] matrix) {
    if (matrix == null || matrix.length == 0 || matrix[0].length == 0)
        return new LinkedList<Integer>();
    LinkedList<Integer> res = new LinkedList<>();
    int left = 0, right = matrix[0].length - 1;
    int top = 0, bottom = matrix.length - 1;
    while (left < right && top < bottom) {
        for (int i = left; i < right; i++)
            res.add(matrix[top][i]);
        for (int i = top; i < bottom; i++)
            res.add(matrix[i][right]);
        for (int i = right; i > left; i--)
            res.add(matrix[bottom][i]);
        for (int i = bottom; i > top; i--)
            res.add(matrix[i][left]);
        left++;
        right--;
        top++;
        bottom--;
    }

    if (left == right) {
        //剩了一列
        for (int i = top; i <= bottom; i++)
            res.add(matrix[i][left]);
    } else if (top == bottom) {
        //剩了一行，包括剩了一个的情况
        for (int i = left; i <= right; i++)
            res.add(matrix[top][i]);
    }
    return res;
}
```
