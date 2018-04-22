# 思路

```java
public int[] twoSum(int[] nums, int target) {
    int len=nums.length;
    int[] result=new int[2];
    for(int i=0;i<len;i++){
        for(int j=i+1;j<len;j++){
            if(nums[i]+nums[j]==target){
                result[0]=i;
                result[1]=j;
                break;
            }
        }
    }
    return result;
}
```
上面是我想出来的最蠢的办法...

这道题有点像找配对。找到 `a`，找到 `b`，让 `a + b = target`。那么肯定得遍历，遍历的过程中，记录什么，成了思考的关键。

可以使用一个Hash表，记录的是元素和其索引的kv,比较好的做法如下：

```java
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
```

只需进行一次遍历操作，每次进来一个新值，判断配对项是否在Hash表中，如果在直接返回，不在的话就把新值和对应的索引放入Hash表中。

