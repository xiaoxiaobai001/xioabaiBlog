# lintcode #
## 420. 报数 ##

描述:
报数指的是，按照其中的整数的顺序进行报数，然后得到下一个数。如下所示：
	
	1, 11, 21, 1211, 111221, ...
	
	1 读作 "one 1" -> 11.
	
	11 读作 "two 1s" -> 21.
	
	21 读作 "one 2, then one 1" -> 1211.
	
	给定一个整数 n, 返回 第 n 个顺序。

样例: 给定 n = 5, 返回 "111221".

思路：主要是理解题意，后一个数字其实是统计前一个数字每个数字出现的次数及其数字

<pre>
public class Solution {
    /**
     * @param n: the nth
     * @return: the nth sequence
     */
    public String countAndSay(int n) {
        // write your code here
        String oldString = "1"; //最开始第一个字符串为“1”
        //for (int j = 1; j < n; ++j) {
        while(--n>0){  //循环n-1次
            StringBuilder sb = new StringBuilder();
            //之前的数转换为字符串数组
            char[] oldChars = oldString.toCharArray();
            for(int i=0;i< oldChars.length;i++){
                int count = 1;//把相邻的，而且相同的数计数
                while((i)< oldChars.length-1 && oldChars[i]==oldChars[i+1]){
                    count++;
                    i++;  //当前值和下一个值相同则直接遍历下一个
                }
                //将当前统计的加入StringBuilder中，当前后不相等就先继续遍历下一个
                sb.append(String.valueOf(count) + String.valueOf(oldChars[i]));
            }
			//将当前字符串作为下一个字符串统计的开始
            oldString = sb.toString();
        }
        return oldString;
    }
}</pre>