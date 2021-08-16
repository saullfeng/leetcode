### 题目描述

给定一个整数数组 `nums` 和一个目标值 `target`，请你在该数组中找出和为目标值的那 **两个** 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

**示例:**

```
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```

```
# 时间复杂度：O(n)
# 空间复杂度：O(n)
```

哈希表法

![image-20210816105115987](C:\Users\solfeng\AppData\Roaming\Typora\typora-user-images\image-20210816105115987.png)

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        //
        int n= nums.length;
        int[] res =new int[2];
        if(n==0)
        return res;
        

        HashMap<Integer,Integer> map = new HashMap<>();

        for(int i=0;i<n;i++){
            int value =nums[i];
            int r =target-value;
           //此时map的集合中的key是否有等于 r
           if(map.containsKey(r)){
               res[1]=i;
               res[0]=map.get(r);
               return res;
           }else{
               //找到合适值便会停下，不一定put所有值
               map.put(value,i);
           }
        }
      throw new IllegalArgumentException("No two sum solution");

    }
}
```

简化

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            if (map.containsKey(complement)) {
                return new int[] { map.get(complement), i };
            }
            map.put(nums[i], i);
        }
        throw new IllegalArgumentException("No two sum solution");
    }
}

```

![image-20210816105012237](C:\Users\solfeng\AppData\Roaming\Typora\typora-user-images\image-20210816105012237.png)