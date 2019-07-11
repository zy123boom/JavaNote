## leetcode46.全排列
```java
public List<List<Integer>> permute(int[] nums){
    List<List<Intger>> res = new ArrayList<>();
    if(nums == null || nums.length == 0) return res;
    helper(res, nums, 0, nums.length - 1);
    return res;
}
public void helper(List<List<Integer>> res, int[] nums, int low, int high){
    if(low == high){
        List<Intger> list = new ArrayList<>();
        for(int j = 0; j < nums.length; j++){
            list.add(nums[j]);
        }
        res.add(list);
        return;
    }
    for(int i = low; i <= high; i++){
        swap(nums, low, i);
        helper(res, nums, low + 1, high);
        swap(nums, low, i);
    }
}
public void swap(int[] nums, int i, int j){
    int tmp = nums[i];
    nums[i] = nums[j];
    nums[j] = tmp;
}
```
