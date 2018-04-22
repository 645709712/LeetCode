# 思路

'''

class Solution {

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
}

'''

上面是我想出来的最蠢的办法...

这道题有点像找配对。找到 `a`，找到 `b`，让 `a + b = target`。那么肯定得遍历，遍历的过程中，记录什么，成了思考的关键。

可以使用一个Hash表，记录的是元素和其索引的kv,比较好的做法如下：

![191524394493_.pic_hd](/Users/lutz/Library/Containers/com.tencent.xinWeChat/Data/Library/Application Support/com.tencent.xinWeChat/2.0b4.0.9/10719c9ed9d778d85bb20b3e888aaa23/Message/MessageTemp/10719c9ed9d778d85bb20b3e888aaa23/Image/191524394493_.pic_hd.jpg)

只需进行一次遍历操作，每次进来一个新值，判断配对项是否在Hash表中，如果在直接返回，不在的话就把新值放入Hash表中。

