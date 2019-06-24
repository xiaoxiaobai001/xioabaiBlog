# lintcode #
## 149. 买卖股票的最佳时机 ##

描述：
假设有一个数组，它的第i个元素是一支给定的股票在第i天的价格。如果你最多只允许完成一次交易(例如,一次买卖股票),设计一个算法来找出最大利润。

样例：给出一个数组样例 [3,2,3,1,2], 返回 1 

<pre>
public class Solution {
    /**
     * @param prices: Given an integer array
     * @return: Maximum profit
     */
    public int maxProfit(int[] prices) {
        // write your code here
        
        //思路1：循环遍历两次数组，将第一次循环遍历作为买入股票时机，第二次作为卖出时机，两者相减得出最大利润值。
       /*
       if(prices == null || prices.length ==0)
            return 0;
        int maxPro = 0;
        for(int i=0; i<prices.length;i++)
        {
            for(int j=i;j<prices.length;++j)
            {
                if(prices[j] - prices[i]>maxPro)
                {
                   maxPro = prices[j] - prices[i];
                }
                
            }
        }
        return maxPro;
    }*/
    
    //思路2：遍历一遍数组，记录买入时最小值，与之后的卖出值之差得到，两者相减比较得出最大利润值。
        if(prices == null || prices.length ==0)
                return 0;
        int min = Integer.MAX_VALUE;
        int profit = 0 ;
        for(int i:prices)
        {
            min = i < min ? i : min;
            profit = (i - min) > profit ? i - min : profit;
        }
        
        return profit;   
    }
}</pre>