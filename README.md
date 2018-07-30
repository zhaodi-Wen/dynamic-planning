# 关于动态规划的讨论与分析
## 一．问题提出
- ①假设有一个数组，它的第i个元素是一支给定的股票在第i天的价格。如果你最多只允许完成一次交易(例如,一次买卖股票),设计一个算法来找出最大利润。
- ②假设你有一个数组，它的第i个元素是一支给定的股票在第i天的价格。设计一个算法来找到最大的利润。你最多可以完成两笔交易。
- ③假设你有一个数组，它的第i个元素是一支给定的股票在第i天的价格。设计一个算法来找到最大的利润。你最多可以完成k笔交易.
- ④假设有一个数组，它的第i个元素是一个给定的股票在第i天的价格。设计一个算法来找到最大的利润。你可以完成尽可能多的交易(多次买卖股票)。然而,你不能同时参与多个交易(你必须在再次购买前出售股票)

格式:输入一个一维数组prices，代表股票的价格。  
理解:这里对于交易次数可以理解为将股票卖出的次数  

# 下面是对以上问题的分析

## 二．问题①
### 1.问题分析

profit[i][j]表示第i个元素到第j个元素差值。
因为只能交易一次，所以这意味着只能选择数组中差值最大的两个元素prices[i],prices[j]。   
则profit[i][j] = prices[j]-prices[i],(i<j&&prices[i],prices[j]).
那么要怎么找到最小值和最大值呢  
下面是找到最大值和最小值的两个最优子结构：  
设minprices为当前遍历过程中的最小值每次,minprices的初值为prices[0]  
则minprices = min(prices[k],minprices)（k<n）  
对于这个最优子结构的理解是,在从0-k-1遍历中，minprices保留前k-1次便利的最小值，在第k次时比较minprices和prices[k]的大小，取较小的值，从而更新minprices，使之成为前k次遍历中的最小值。   
设maxprofit为当前遍历的最大利润，maxprofit的初值为0，  
假设maxprofit保存了前k-1次遍历的最大值，则在第k次遍历后，  
则maxprofit = max(maxprofit,prices[k]-minprices)  
由表达式可知若第k个元素和前面遍历的元素的最小值的差值比原来的maxprofit大，则更新maxprofit.  

也就是说这里涉及到两个最优子结构，一个是找到当前遍历过程中的最小元素，同时还要根据这个最小元素找到最大利润的最大值。  

### 2.算法思想:
    Input :prices[]
    Output :maxprofit
    int maxProfit(int *prices)
    {
    minpices = prices[0];
    maxprofit = 0;
	    for(i=1;i<n;i++)
      {
      minprices = min(minprices,prices[i]);
      maxprofit = max(maxprofit,prices[i]-minprices);
      }
    return maxprofit;
    }

### 3.算法实现
以下分别用python和c++对算法进行实现:  
#### Python代码：

        class Solution1:
            def __init__(self, prices):
                self.prices = prices
                self.profit = 0
                self.min_prices = prices[0]

            def max_profit(self):
                len1 = len(self.prices)

                for i in  range(1,len1):
                   self.min_prices = min(self.min_prices, self.prices[i])
                    self.profit = max(self.profit, self.prices[i]-self.min_prices)

                return self.profit
        if __name__ == '__main__':
             print('*'*10 +"交易一次" +'*'*10)
             a = Solution1([3, 2, 6, 5, 0, 3])
             print('最大利润为'+str(a.max_profit()))
             print('*'*10+"交易一次"+'*'*10)



### 4.结果分析:
对于输入的数组[3,2,6,5,0,3],程序的结果是4，下面进行分析    
因为只能交易一次，所以刚开始minprices = prices[0]=3,maxprofit=0;    
共进行5次遍历:  

    第1次 minprices =min(minprices,prices[1])=min(3,2) = 2, 
    maxprofit = max(maxprofit,prices[1]-minprices)=max(0,2-2)=0

    第2次 minprices=min(minprices,prices[2])=min(2,6)=2,
    maxprofit=max(maxprofit,prices[2]-minprices) =max(0, 6-2)=4

    第3次 minprices=min(minprices,prices[3])=min(2,5)=2,
    maxprofit=max(maxprofit,prices[3]-minpices)=max(4,5-2)=4

    第4次 minprices=min(minprices,prices[4])=min(2,0)=0,
    maxprofit =max(maxprofit,prices[4]-minprices)=max(4,0-0)=4

    第5次 minprices=min(minprices,prices[5])=min(0,3)=0.
    maxprofit = max(maxprofit,prices[5]-minprices)=max(4,3-0)=4

所以最后的结果就是4，即第2天买入，第三天卖出。  



#### C++代码：
    class Solution2
    {
    public:
        int maxProfit(vector<int>& prices)
        {

            int minprices = prices[0],maxprofit =0;
            int i;
            int len = prices.size();
            vector<int>::iterator max1 = max_element(prices.begin(),prices.end());
            int max_one = max(*max1,len)+1;

            vector<int> day(max_one,0);
            for(i=0;i<len;i++) {
                day[prices[i]] = i + 1;
            }
            int start=day[prices[0]],end=day[prices[0]];
            for(i=0;i<len;i++)
            {
                if(prices[i]-minprices>maxprofit)
                {

                    start = day[minprices];
                    end = day[prices[i]];
                }

                minprices = min(minprices,prices[i]);
                maxprofit=max(maxprofit,prices[i]-minprices);
            }
            cout<<"第"<<start<<"天"<<"购买"<<","<<"第"<<end<<"天"<<"出售"<<",";
            return  maxprofit;
        }
    };


运行结果：


### 5.复杂度分析:
由算法可知，该算法只有一层for循环，故时间复杂度为O(n).



## 三.问题②
这个问题是对问题①的改进，因为只能进行2次交易，所以我们可以看成是两次问题①的结合。

### 1.问题分析:
首先还是像①中一样算出每一次遍历过程中每一支股票交易时所获得的最大利润,
还是以①中的例子为例

    Prices： 		 3     2     6     5     0     3   
    Minprices                3     2     2     2     0     0
    Profit			 0     0     4     3     0     3

这里minprices和题目①一样，就是遍历k次，前k次元素的最小值。  
在求minprices时，其初值为minprices = prices[0]=3  
Minprices = min(minprices,prices[k])  

这里的profit是假设每支股票在当前进行交易可以获得的收益  
也就是说在第k天将股票卖出获得的利润。  
所以profit[k] = max(maxprfit,prices[k]-minprices),(其中maxprofit在循环开始前的初始化为0)  

如果是只交易一次，那么我们就可以像①中一样，求出profit的最大值，但是，这里要求2次交易，所以还需确定则两次交易的时间。  

确定每一笔交易的时间，这里要做的就是把两个阶段的最大利润求出来。  
可以选择从后往前遍历,具体如下,  
初始化maxprofit = 0，maxprices = prices[n-1],temp= profit[n-1]  
这里的maxprofit是整体的最大利润，temp是当前的这一阶段的最大利润.  
可以对比以下，从前往后遍历时，我们要找的时minprices,从后往前遍历时，要找的时maxprices,  
这里的maxprices 需要更新,maxprices = max(prices[k],maxprices)  
这里同样有两个最优子结构，分别是temp和maxprofit  
循环从n-2开始,对于i=k;  
temp =max(temp,maxprices-prices[k])，  


在获得后一阶段的局部最优后，需要考虑整体最优  
Maxprofit = maxprofit(maxprofit,temp+profit[k])  
这里的profit[k]是每个元素的最大利润，也就是第一笔操作的局部最优  

所以综合这两部分，我们可以对上表进行进一步的处理 

    Prices:       3     2     6     5     0     3
    Minprices     3     2     2     2     0     0
    Profit        0     0     4     3     0	    3
    (从后往前)

    maxprices	6     6     6     5     3     3
    temp		3     4     0     0     3     0
    maxprofit	7     4     4     3     3     3



### 2.算法思想:
    Input :prices
    Output:maxprofit

    int maxprofit(int *prices)
    {
       第一步:求profit[i]
       Minprices = prices[0];
	    for(int i =0,i<n;i++)
      {
        minprices = min(minprices,prices[i])
        If(prices[i]-minprices>maxprofit){
           maxprofit = prices[i]-minprices;
          }
        Profit[i]=maxprofit;
      }
     }


求maxprofit，全局最优解

      maxprofit=profit[n-1],temp=0,maxprices=prices[n-1];
      for(i=n-2;i>0;i--)
      {
          Maxprices =max(maxprices,prices[i]);
          if(maxprices-prices[i]>temp)
            temp = maxprices-prices[i];
          if(temp+profit[i]>maxprofit)
              Maxprofit =temp+profit[i];
          //或者可以写成
            // temp = max(temp,maxprices-prices[i])
            // maxprofit = max(maxprofit,temp+profit[i])
          }
      return maxprofit;
      }


### 3.算法实现:
以下分别使用python和c++对算法进行实现:

#### Python代码：

    class Solution2:
        def __init__(self,prices):
            self.prices = prices
            self.max_profit = 0
            self.min_prices = prices[0]
        def max_Profit(self):
            len1 = len(self.prices)
            profit = [[] for i in range(len1)]
            for i in range(len1):
                self.min_prices = min(self.min_prices,self.prices[i])
                if(self.prices[i]-self.min_prices>self.max_profit):
                    self.max_profit = self.prices[i]-self.min_prices
                profit[i] =self.max_profit
            max_prices = self.prices[len1-1]
            temp = 0
            for i in reversed(range(len1-1)):
                max_prices =max(max_prices,self.prices[i])
                if(max_prices-self.prices[i]>temp):
                    temp = max_prices-self.prices[i]
                if(temp+profit[i]>self.max_profit):
                    self.max_profit=temp+profit[i]
            return self.max_profit
    if __name__ == '__main__':
      print('*'*10 +"交易两次" +'*'*10)
      a = Solution2([3, 2, 6, 5, 0, 3])
      print('最大利润为'+str(a.max_Profit()))
      print('*'*10+"交易两次"+'*'*10)




#### C++代码:
    class Solution1
    {
    public:
        int maxProfit(vector<int>& prices)
        {
                int n = prices.size();
                vector<int> profit(n,0);
                vector<int>::iterator max1= max_element(prices.begin(),prices.end());
                int max_one = max(*max1,n)+1;
                int i;
                int minprices = prices[0],maxprofit =0;
                for(i=0;i<n;i++)
                {
                    minprices = min(prices[i],minprices);
                    if(prices[i]-minprices>maxprofit)
                    {
                        maxprofit = prices[i]-minprices;
                    }
                    profit[i] = maxprofit;
                }
                int maxprices = prices[n-1],temp = profit[n-1];
                maxprofit=0;

                for(i=n-2;i>0;i--)
                {

                    maxprices = max(maxprices,prices[i]);
                    if(maxprices-prices[i]>maxprofit)
                    {
                        maxprofit = maxprices-prices[i];
                    }
                    if(maxprofit+profit[i]>temp)
                    {
                        temp = maxprofit+profit[i];
                    }

                }
                return temp;
        }
    };


运行结果:

### 4.复杂度分析:
由算法可知，每次for循环只有一层，故时间复杂度为O(n)


## 四．问题③
这里是对问题①和问题②的又一次升级，因为问题三加入了交易次数的限制，这里先分别举两个例子

- 1. 还是以prices[6] = {3,2,6,5,0,3}为例
由问题①和②可知，当交易次数分别为1次和2次时，最大利润为4和7
当交易次数k>2时，最大利润还是7

- 2. prices[6] ={1,2,1,2,1,2}
若从分别从问题①和问题②的角度考虑，最大利润分别为1和2
但是当k =3时，最大利润为3，当k>3时，最大利润了仍为3
也就是第1天购入，第2天售出，
      第3天购入，第4天售出
      第5天购入，第6天售出，总利润为3

### 1.问题分析：
问题③可以看成若干个问题①的结合，但还是需要采用问题②中的办法，只用局部最优和全局最优相结合的方法解决。  
定义profit[i][j]为前i天进行j次交易，获得的最大利润  
则若由一般动态规划问题可得profit[i][j]的表达式为  
Profit[i][j]= max(profit[i-1][j],profit[i-1][j-1]+prices[i]-prices[i-1])  

该表达式可以这么理解:  
profit[i-1][j]:为前i-1天进行的j笔交易获得的利润和  
profit[i-1][j-1]+prices[i]-prices[i-1]:前i-1天进行的j-1笔交易,然后在第i天进行的交易第j笔交易  
二者取最大值。  

但是有一个问题，就是若第i-1天也有交易，那么第i天和第i-1天进行的交易可以合为一次，也就是说总共进行了j-1次交易，没有j次，即是说profit[i-1][j-1]+prices[i]-prices[i-1]只进行了j-1次交易，这样最大利润就会减少。


为解决这个问题，需要引入两个数组global和local，类似于②中的maxprofit和temp, local为局部最优，global为全局最优  
对于第i次遍历  

其中local[i][j]=max(local[i-1][j]+prices[i]-prices[i-1],global[i-1][j-1]+max(0,prices[i]-prices[i-1]))  
这里local[i-1][j]+prices[i]-prices[i-1]就是为了避免第i天交易和第i-1天交易重合在一起。  
global[i-1][j-1]表示在前i-1天进行了j-1笔交易，若第i天交易由利润，则进行第i笔交易，否则不进行  


然后再来看一下全局最优，同样也是一个最优子结构问题，  
则global[i][j] = max(global[i-1][j],local[i][j])  

global[i-1][j]表示前i-1天进行了j笔交易的最大利润，  
将global[i-1][j]和local[i][j]进行比较，既是将全局最优和局部最优进行比较，从而得到新的全局最优。  


### 2算法思想:
    Input: prices,k
    Output :maxprofit

    int max_profit(int k,int *prices)
    {
		    global[n][k+1] =0
            local[n][k+1] =0
            for(i=1;i<n;i++)
                for(j=0;j<k+1;j++)
                    {
                          local[i][j] = max(local[i-1][j]+prices[i]-prices[i-1],global[i-1][j-1]+max(0,prices[i]-prices[i-1])
                          global[i][j] = max(global[i-1][j],local[i][j])
                    }
            return global[n-1][k];
    }



### 3.算法实现:
#### Python代码：
    class Solution3:
        def __init__(self,k,prices):
            self.prices = prices
            self.len = len(self.prices)
            self.k =k
            self.globals=[[0 for i in range(self.k+1)]for j in range(self.len)]
            self.local = [[0 for i in range(self.k+1)] for j in range(self.len)]

        def max_Profit(self):
            for i in range(1,self.len):
                for j in range(1,self.k+1):
                    self.local[i][j]=max(self.local[i-1][j]+self.prices[i]-self.prices[i-1],self.globals[i-1][j-1]+max(0,self.prices[i]-self.prices[i-1]))
                    self.globals[i][j]=max(self.globals[i1][j],self.local[i][j])

            return self.globals[self.len-1][self.k]

    if __name__ == '__main__':
        k=3
        print('\n'+'*'*10 +'交易{}次'.format(k) +'*'*10)
        a = Solution3(k, [1, 2, 1, 2, 1, 2])
        print('最大利润为'+str(a.max_Profit()))
        print('*'*10+'交易{}次'.format(k)+'*'*10)

当prices[6] = {3,2,6,5,0,3}时运行结果为


当prices[6] = {1,2,1,2,1,2}时
运行结果为



#### C++代码:
    #include <iostream>
    #include <vector>
    #include <algorithm>
    using namespace std;
    class Solution3{
    public:
        int maxProfit(int k,vector<int>& prices)
        {
            int n = prices.size();
            vector<vector<int>> local(n,vector<int>(k,0));
            vector<vector<int>> global(n,vector<int>(k,0));
            for(int i =1;i<n;i++)
            {
                for(int j =1;j<=k;j++)
                {
                    int diff = prices[i]-prices[i-1];
                    local[i][j] = max(global[i-1][j-1]+max(diff,0),local[i-1][j]+diff);
                    global[i][j] = max(global[i-1][j],local[i][j]);
                }
            }
            for(int i =0;i<n;i++)
            {
                for(int j=0;j<=k;j++)
                    cout<<local[i][j]<<" ";
                cout<<endl;
            }
            cout<<endl;
            for(int i =0;i<n;i++)
            {
                for(int j=0;j<=k;j++)
                    cout<<global[i][j]<<" ";
                cout<<endl;
            }
            return global[n-1][k];
       }
        ~Solution3() {}
    };


运行结果，这里prices[6]={3,2,6,5,0,3},其中打印出来的分别为local和global的值

### 4.时间复杂度分析:
for循环由两层，故时间复杂度为O(kn)

## 五问题④
### 1. 问题分析  
问题四不限交易次数，故可以选择不用动态规划法，而是使用贪心算法  
每次遍历过程中，若prices[i]>prices[i-1]，则进行交易  
   设maxprofit为最后结果
  则当(prices[i]>prices[i-1])时，maxprofit+=prices[i]-prices[i-1]
### 2. 算法思想：
    Input ：prices
    Output :maxprofit
    int max_profit(int *prices)
    {
        maxprofit=0;
		    if(prices==NULL)
            return 0
        for(i=1;i<n;i++)
            maxprofit +=max(0,prices[i]-prices[i-1])
        return maxprofit
    }
    

### 3. 算法实现：

#### Python代码：
    class Solution4:
        def __init__(self,prices):
            self.prices = prices
            self.len = len(self.prices)
            self.maxprofit = 0
        def max_Profit(self):
            for i in range(1,self.len):
                self.maxprofit += max(self.prices[i]-self.prices[i-1],0)

            return  self.maxprofit

    if __name__ == '__main__':
        a = Solution4([1,2,2,3,0,1])
        print('最大利润为'+str(a.max_Profit()))


这里的测试样例为prices[6]={1,2,2,3,0,1}
结果产生的过程是 
第一天购入，第二天售出   
第三天购入，第四天售出   
第五天购入，第六天售出   
总利润为3

#### C++代码
    class Solution5
    {
    public:
        int maxProfit(vector<int>& prices)
        {
            int len = prices.size();
            int maxprofit=0;
            for(int i =1;i<len;i++)
            {
                maxprofit+=max(prices[i]-prices[i-1],0);
            }
            return maxprofit;
        }
       ~Solution5(){}
    };
    int main()
    {
        int a[6]={1,2,2,3,0,1};
        vector<int> c(a,a+6);
        Solution5 g;
        cout<<"最大利润是"<<g.maxProfit(c)<<endl<<endl;
    }


运行结果是：



### 时间复杂度：
 因为for循环只有一次，故时间复杂度为O(n)



## 六．总结
这次主要从四个题目分析了动态规划法，加入了自己的思考，前三个问题的最优子结构不止只有一个,从前遍历，和从后往前遍历，以及求全局的最优解都有最优子结构，都可以推出自己的数学推导式。在推出表达式后，分别使用了python和c++进行实现，由于算法思想相同，故程序的结构差距不大，python明显比c++简洁。

存在的问题:
本来想除了求出最优解，还想求出每一次交易的购入点和出售点，但是除了求出交易一次的购入点和出售点外，当交易次数大于1次时，无法求出，分析的原因是对于每一次交易局部最优解的没有发现到下标的变化，初步猜想下标的变化也是一个动态规划问题，但是没有推出其表达式。
