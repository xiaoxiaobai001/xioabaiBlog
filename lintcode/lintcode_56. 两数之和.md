# lintcode #
## 56. 两数之和 ##

描述：给一个整数数组，找到两个数使得他们的和等于一个给定的数 target。

你需要实现的函数twoSum需要返回这两个数的下标, 并且第一个下标小于第二个下标。注意这里下标的范围是 0 到 n-1。（tips：你可以假设只有一组答案。）

样例：给出 numbers = [2, 7, 11, 15], target = 9, 返回 [0, 1].

挑战：

Either of the following solutions are acceptable:

O(n) Space, O(nlogn) Time
O(n) Space, O(n) Time

思路1：遍历数组，将当前遍历到的作为第一数字，从当前的下一个开始遍历，找有没有数字等于target - numbers[i]的，有则返回。
时间复杂度为O(n^2),空间复杂度为O(1)
<pre>
public class Solution {
    /**
     * @param numbers: An array of Integer
     * @param target: target = numbers[index1] + numbers[index2]
     * @return: [index1 + 1, index2 + 1] (index1 < index2)
     */
    public int[] twoSum(int[] numbers, int target) {
        // write your code here
       int[] twoNum = new int[2]; //定义一个大小为2的一位数组，存放返回结果
        for(int i=0; i< numbers.length; ++i)
     {
            for(int j=i+1; j< numbers.length; ++j)
        {
            if(numbers[j] == target - numbers[i] )
            {
                twoNum[0] = i;
                twoNum[1] = j;
            }
        }
        
     }
     return twoNum;   
    }
}</pre>