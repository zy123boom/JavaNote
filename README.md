#Java实现的各种排序
###一、冒泡排序
```
public class Hello {
    public static void main(String[] args) {
        System.out.println("hello");
    }
}
```

```
public static void bubbleSort2(int[] arr){
    if(arr == null || arr.length < 2) return;
    for(int end = arr.length - 1; end > 0; end--){
        for(int i = 0; i < end; i++){
            if(arr[i] > arr[i + 1]){
                swap(arr, i, i + 1);
            }
        }
    }
}

public static void swap(int[] arr, int i, int j) {
    int tmp = arr[i];
    arr[i] = arr[j];
    arr[j] = tmp;
}
```

###二、选择排序
```
/*
 * 数组长为n
 * 排0~n-1，最小的放0
 * 排1~n-1,最小的放1
 * 排2~n-1,最小的放2
 * ....
 */

public static void selectSort(int[] arr){
    if(arr == null || arr.length < 2) return;
    //外层0~n-1
    for(int i = 0; i < arr.length - 1; i++){
       //最小的从0~n-1开始
        int minIndex = i;
        for(int j = i + 1; j < arr.length; j++){
            if(arr[j] < arr[minIndex]){
                minIndex = j;
            }
        }
        swap(arr, i, minIndex);
    }
}

public static void swap(int[] arr, int i, int j){
    int tmp = arr[i];
    arr[i] = arr[j];
    arr[j] = tmp;
}
```

###三、插入排序
```
public static void insertSort(int[] arr){
    if(arr == null || arr.length < 2) return ;
    //从下标为1的开始往后走
    for(int i = 1; i < arr.length; i++){
        //j每次比i小1,然后比较，前面的比后面的大，则不停的交换
        for(int j = i - 1; j >= 0 && arr[j] > arr[j + 1]; j--){
            swap(arr, j, j + 1);
        }
    }
}

public static void swap(int[] arr, int i, int j){
    int tmp = arr[i];
    arr[i] = arr[j];
    arr[j] = tmp;
}
```

###四、快速排序
```
public static void quickSort(int[] arr, int start, int end) {
    if (start < end) {
        // 找到标准数，一般为第0个
        int stard = arr[start];
        // 记录需要排序的下标
        int low = start;
        int high = end;
        // 循环找比标准数大的数和比标准数小的数
        while (low < high) {
            // 高标志的数字比标准数大
            while (low < high && stard <= arr[high]) {
                high--;
            }
            // 高标志的数字比标准数小，则用高标志的数字替换左边的数字，即低标志的数字
            arr[low] = arr[high];
            // 低标志数字比标准数小
            while (low < high && arr[low] <= stard) {
                low++;
            }
            // 低标志的数字大于标准数，将低标志数赋给高标志数
            arr[high] = arr[low];
        }
        // 循环后标准数所在位已经不是标准数，将标准数赋给低标志所在的元素
        arr[low] = stard;
        // 处理所有的小的数字，递归
        quickSort(arr, start, low);
        // 处理所有的大的数字，递归
        quickSort(arr, low + 1, end);
    }
}
```

###五、归并排序
```
/**
 * 1.先合并两个数组，处理数组使两个数组有序
 * @param arr 数组
 * @param low 从哪里开始
 * @param middle 从哪里分割，指向小数组的最后一个
 * @param high 从哪里结束
 */
public static void merge(int[] arr, int low, int middle, int high){
     //用于存储归并后的临时数组
     int[] temp = new int[high - low + 1];
     //用于记录第一个数组中需要遍历的下标，两段
     int i = low;
     int j = middle + 1;
     //记录在临时数组中的存放下标
     int index = 0;
     //遍历两段数组，取出小的数字放入临时数组中
     while(i <= middle && j <= high){
         if(arr[i] < arr[j]){
             temp[index++] = arr[i++];              
         } else {
             temp[index++] = arr[j++];
         }
     }
     //一段数组放完，处理另一段多余的数据
     while(j <= high){
         temp[index++] = arr[j++];
     }
     while(i <= middle){
         temp[index++] = arr[i++];
     }
     //把临时数组中数据重新存到原数组
     for (int k = 0; k < temp.length; k++) {
         arr[k + low] = temp[k];
     }
}

// 递归的排序两段
public static void mergeSort(int[] arr, int low, int high) {
    if (low == high) {
        return;
    }
    int middle = (low + high) / 2;
    // 处理左边
    mergeSort(arr, low, middle);
    // 处理右边
    mergeSort(arr, middle + 1, high);
    // 排序后归并
    merge(arr, low, middle, high);
}
```

###六、堆排序
```
//堆排序
public static void heapSort(int[] arr){
    if(arr == null || arr.length < 2) return;
    //1.调整为大顶堆
    for (int i = 0; i < arr.length; i++) {
        heapInsert(arr, i);
    }
    int heapSize = arr.length;
    //2.交换堆上0位置的数和堆上最后一个位置的数,并且使堆的大小减1
    swap(arr, 0, --heapSize);
    while(heapSize > 0){
        //3.
        heapify(arr, 0, heapSize);
        swap(arr, 0, --heapSize);
    }
}

/**
 * 调整为大顶堆
 * @param arr
 * @param index 调整过程中要交换的位置
 */
public static void heapInsert(int[] arr, int index) {
    //index为当前位置，父节点位置为(index - 1) / 2
    while(arr[index] > arr[(index - 1) / 2]){
        swap(arr, index, (index - 1) / 2); 
        index = (index - 1) / 2;
    }
}

/**
 * 
 * @param arr
 * @param index 从哪个位置开始heapify
 * @param heapSize 堆一共的大小（不是数组的大小）
 */
public static void heapify(int[] arr, int index, int heapSize) {
    int left = (index * 2) + 1;
    while(left < heapSize){
        //left+1是右孩子
        //给出孩子中较大的孩子的下标
        int largest = left + 1 < heapSize && arr[left + 1] > arr[left] ? left + 1 : left; 
        //左右两个孩子的较大的值如果大于当前的值，孩子的下标赋给largest，即 我和我的孩子，哪个大，哪个作为largest
        largest = arr[largest] > arr[index] ? largest : index;
        //如果父节点依旧是最大的，不用交换了，直接break
        if(largest == index){
            break;
        }
        //此时说明我孩子的某一个值比我的值要大,那个孩子的位置是largest
        swap(arr, largest, index);
        //原来的index位置被换到了largest位置，要从largest继续往下走，要换回来
        index = largest;
        //此时的左孩子的位置变了，要更新为下一次while使用
        left = index * 2 + 1;
    }
}

public static void swap(int[] arr, int i, int j){
    arr[i] = arr[i] ^ arr[j];
    arr[j] = arr[i] ^ arr[j];
    arr[i] = arr[i] ^ arr[j];
}
```

###桶排序（0~200 values）
```
public static void bucketSort(int[] arr){
    if(arr == null || arr.length < 2) return;
    int max = Integer.MIN_VALUE;
    for (int i = 0; i < arr.length; i++) {
        max = Math.max(max, arr[i]);
    }
    int[] bucket = new int[max + 1];
    for (int i = 0; i < arr.length; i++) {
        bucket[arr[i]]++;
    }
    int i = 0;
    for (int j = 0; j < bucket.length; j++) {
        while(bucket[j]-- > 0){
            arr[i++] = j;
        }
    }
}

```

