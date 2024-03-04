






昨天看见一本书叫《Python编程300例：快速构建可执行高质量代码》，今天来实现以下，顺便贴在博客里，接下来每10例发一条博客，以便后期查询相关案例，也供大家参考实现。




#### 文章目录


* + - [001反转一个三位整数](#001_3)
		- [002合并重排有序数组](#002_28)
		- [003旋转字符串](#003_72)
		- [004相对排名](#004_103)
		- [005二分查找](#005_139)
		- [006下一个更大的数](#006_177)
		- [007计算字符串中的单词数](#007_216)
		- [008勒索信](#008_242)
		- [009找出不重复的两个数](#009_279)
		- [010双胞胎字符串](#010_311)




#### 001反转一个三位整数


问题描述：将一个三位整数反转并输出。如输入123，输出321；输入900，输出9。


代码实现：



```
class Solution:
#参数number: 一个三位整数
    #返回值: 反转后的数字
    #记：/得到高位数，%得到低位数
    def reverseInteger(self,number):
        h = int(number/100)
        t = int(number%100/10)
        z = int(number%10)
        return (100\*z+10\*t+h)
#主函数
if __name__ == '\_\_main\_\_':
    solution = Solution()
    num = 857
    ans = solution.reverseInteger(num)
    print("输入：", num)
    print("输出：", ans)

```

运行结果：  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620094754313.png)


#### 002合并重排有序数组


问题描述：将两个升序的整数数组A和B合并重排。如输入A=[1，2，3，4]，B=[2，4，5，6]，输出[1，2，2，3，4，4，5，6]。


代码实现：



```
class Solution:
    #参数A: 有序整数数组A
    #参数B: 有序整数数组B
    #返回:一个新的有序整数数组
    def mergeSortedArray(self, A, B):
        i, j = 0, 0
        C = []
        while i < len(A) and j < len(B):
            if A[i] < B[j]:
                C.append(A[i])
                i += 1
            else:
                C.append(B[j])
                j += 1
        while i < len(A):
            C.append(A[i])
            i += 1
        while j < len(B):
            C.append(B[j])
            j += 1
        return C
#主函数
#主函数
if __name__ == '\_\_main\_\_':
    A = [1,4]
    B = [1,2,3]
    D = [1,2,3,4]
    E = [2,4,5,6]
    solution = Solution()
    print("输入：", A, " ", B)
    print("输出：", solution.mergeSortedArray(A,B))
    print("输入：", D, " ", E)
    print("输出：", solution.mergeSortedArray(D,E))


```

运行结果：  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620095625214.png)


#### 003旋转字符串


问题描述：给定一个字符串（以字符数组的形式）和一个偏移量，根据偏移量原地从左向右旋转字符串。如输入str=“abcdefg”，offset =2，输出"fgabcde"，返回旋转后的字符串。


代码实现：



```
class Solution:
    #参数s:字符列表
    #参数offset:整数
    #返回值:无
    def rotateString(self, s, offset):
        if len(s) > 0:
            offset = offset % len(s)
        temp = (s + s)[len(s) - offset : 2 \* len(s) - offset]
        for i in range(len(temp)):
            s[i] = temp[i]
    #[a,b]在python中是对列表的切片操作，表示将一个list从第a位截取到第b-1位。
    
#主函数
if __name__ == '\_\_main\_\_':
    s = ["a","b","c","d","e","f","g"]
    offset = 3
    solution = Solution()
    solution.rotateString(s, offset)
    print("输入：s =", ["a","b","c","d","e","f","g"], " ", "offset =",offset)
    print("输出：s =", s)


```

运行结果：  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620102053460.png)


#### 004相对排名


问题描述：根据N名运动员得分，找到相对等级和获得最高分前3名的人，分别获得金牌、银牌和铜牌。N是正整数，并且不超过10 000。所有运动员的成绩都保证是独一无二的（无并列）。如输入[5，4，3，2，1]，输出[“Gold Medal”，“Silver Medal”，“BronzeMedal”，“4”，“5”]，前3名运动员得分较高，根据得分依次获得金牌、银牌和铜牌。对于后两名运动员，根据分数输出相对等级。


代码实现：



```
class Solution:
    #参数nums为整数列表
    #返回列表
    def findRelativeRanks(self, nums):
        score = {}
        for i in range(len(nums)):
            score[nums[i]] = i
        sortedScore = sorted(nums, reverse=True)
        answer = [0] \* len(nums)
        for i in range(len(sortedScore)):
            res = str(i + 1)
            if i == 0:
                res = 'Gold Medal'
            if i == 1:
                res = 'Silver Medal'
            if i == 2:
                res = 'Bronze Medal'
            answer[score[sortedScore[i]]] = res
        return answer
#主函数
if __name__ == '\_\_main\_\_':
    num = [5,4,3,2,1]
    s = Solution()
    print("输入为：",num)
    print("输出为：",s.findRelativeRanks(num))


```

运行结果：  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620102656413.png)


#### 005二分查找


问题描述：给定一个排序的整数数组（升序）和一个要查找的目标整数target，查找到target第1次出现的下标（从0开始），如果target不存在于数组中，返回-1。


如输入数组[1，4，4，5，7，7，8，9，9，10]和目标整数1，输出其所在的位置为0，即第1次出现在第0个位置。输入数组[1，2，3，3，4，5，10]和目标整数3，输出2，即第1次出现在第2个位置。输入数组[1，2，3，3，4，5，10]和目标整数6，输出-1，即没有出现过6，返回-1。


代码实现：



```
class Solution:
    #参数nums: 整数数组
    #参数target: 要查找的目标数字
    #返回值：目标数字的第一个位置，从0开始 
	def binarySearch(self, nums, target):
		return self.search(nums, 0, len(nums) - 1, target)
	def search(self, nums, start, end, target):
		if start > end:
			return -1
		mid = (start + end)//2
		#'/'表示浮点数除法，返回浮点结果；'//'表示整数除法，返回不大于结果的最大整数
		if nums[mid] > target:
			return self.search(nums, start, mid, target)
		if nums[mid] == target:
			return mid
		if nums[mid] < target:
			return self.search(nums, mid, end, target)
#主函数
if __name__ == '\_\_main\_\_':
    my_solution = Solution()
    nums = [1,2,3,4,5,6]
    target = 3
    targetIndex = my_solution.binarySearch(nums, target)
    print("输入：nums =", nums, " ", "target =",target)
    print("输出：",targetIndex)


```

运行结果：  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620103408540.png)


#### 006下一个更大的数


问题描述：两个不重复的数组nums1和nums2，其中nums1是nums2的子集。在nums2的相应位置找到nums1所有元素的下一个更大数字。nums1中的数字x的下一个更大数字是nums2中x右边第1个更大的数字。如果它不存在，则为此数字输出-1。nums1和nums2中的所有数字都是唯一的，nums1和nums2的长度不超过1000。


示例：输入nums1=[4，1，2]，nums2=[1，3，4，2]，输出[-1，3，-1]。对于第1个数组中的数字4，在第2个数组中找不到下一个更大的数字，因此输出-1；对于第1个数组中的数字1，第2个数组中的下一个更大数字是3；对于第1个数组中的数字2，第2个数组中没有下一个更大的数字，因此输出-1。


代码实现：



```
class Solution:
    #参数nums1为整数数组
    #参数nums2为整数数组
    #返回整数数组
    def nextGreaterElement(self, nums1, nums2):
        answer = {}
        stack = []
        for x in nums2:
            while stack and stack[-1] < x:
                answer[stack[-1]] = x
                del stack[-1]
                #del的用法
            stack.append(x)
        for x in stack:
            answer[x] = -1
        return [answer[x] for x in nums1]
#主函数
if __name__ == '\_\_main\_\_':
    s = Solution()
    nums1 = [5,4,8]
    nums2 = [2,5,6,4,8]
    print("输入1为：",nums1)
    print("输入2为：",nums2)
    print("输出为 ：",s.nextGreaterElement(nums1,nums2))


```

注：[del的用法](https://blog.csdn.net/windscloud/article/details/79732014)


运行结果：  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620104340912.png)


#### 007计算字符串中的单词数


问题描述：计算字符串中的单词数，其中一个单词定义为不含空格的连续字符串。如：输入"Hello，my name is John"，输出5。


代码实现：



```
class Solution:
    #参数s为字符串
    #返回整数
    def countSegments(self, s):
        res = 0
        for i in range(len(s)):
            if s[i] != ' ' and (i == 0 or s[i - 1] == ' '):
                res += 1 
        return res
#主函数
if __name__ == '\_\_main\_\_':
    s = Solution()
    n =  "Hello, my name is Fank Wang"
    print("输入为：",n)
    print("输出为：",s.countSegments(n))


```

运行结果：  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620104944868.png)


#### 008勒索信


问题描述：给定一个表示勒索信内容的字符串和另一个表示杂志内容字符串，写一个方法判断能否通过剪下杂志中的内容构造出这封勒索信，若可以，返回T rue，否则返回False。注：杂志字符串中的每一个字符仅能在勒索信中使用一次。


示例：输入ransom Note=“aa”，m agazine=“aab”，输出T rue，勒索信的内容可以从杂志内容剪辑而来。


代码实现：



```
class Solution:
    """
 参数ransomNote为字符串
 参数magazine为字符串
 返回布尔类型
 """
    def canConstruct(self, ransomNote, magazine):
        arr = [0] \* 300
        #数组长度可修改
        for c in magazine:
            arr[ord(c) - ord('a')] += 1 
        for c in ransomNote:
            arr[ord(c) - ord('a')] -= 1
            if arr[ord(c) - ord('a')] < 0:
                return False
        return True
#主函数
if __name__ == '\_\_main\_\_':
    s = Solution()
    ransomNote = "Your son is here"
    magazine = "I am here"
    print("输入勒索信为：",ransomNote)
    print("输入杂志内容：",magazine)
    print("输出为：",s.canConstruct(ransomNote,magazine))


```

运行结果：  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620105910120.png)


#### 009找出不重复的两个数


问题描述：给定一个数组a[]，其中除了2个数，其他均出现2次，请找到不重复的2个数并返回。如：给出a=[1，2，5，5，6，6]，返回[1，2]，除1和2外其他数都出现了2次，因此返回[1，2]。


代码实现：



```
class Solution:
    #参数arr是输入的待查数组
    #返回值是两个值的列表，内容没有重复的
    def theTwoNumbers(self, a):
        ans = [0, 0]
        for i in a:
            ans[0] = ans[0] ^ i
        c = 1
        while c & ans[0] != c:
            c = c << 1
        for i in a:
            if i & c == c:
                ans[1] = ans[1] ^ i
        ans[0] = ans[0] ^ ans[1]
        #^表示异或XOR，若两个输入的电平相异，则输出为高电平（1）；若两个输入的电平相同，则输出为低电平（0）。
        return ans
if __name__ == '\_\_main\_\_':
    arr = [1, 2, 5, 1]
    solution = Solution()
    print(" 数组为：", arr)
    print(" 两个没有重复的数字是:", solution.theTwoNumbers(arr))


```

运行结果：  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2021062011041335.png)


#### 010双胞胎字符串


问题描述：给定两个字符串s和t，每次可以任意交换s的奇数位或偶数位上的字符，即奇数位上的字符能与其他奇数位的字符互换，偶数位上的字符也能与其他偶数位的字符互换，问能否经过若干次交换，使s变成t。


示例：输入为s=“abcd”，t=“cdab”，输出是"Yes"，第1次a与c交换，第2次b与d交换。输入s=“abcd”，t=“bcda”，输出是"No"，无论如何交换，都无法得到bcda。


代码实现：



```
class Solution:
    #参数s和t是一对字符串
    #返回值是个字符串，能否根据规则转换
    def isTwin(self, s, t):
        if len(s) != len(t):
            return "No"
        oddS = []
        evenS = []
        oddT = []
        evenT = []
        for i in range(len(s)):
            #当&作为位运算时，1&1=1 ，1&0=0，0&0=0；
            if i & 1:
                oddS.append(s[i])
                oddT.append(t[i])
            else :
                evenS.append(s[i])
                evenT.append(t[i])
            #append命令可以添加单个元素，也可以添加可迭代对象；而extend命令只能添加可迭代对象。
        oddS.sort()
        oddT.sort()
        evenS.sort()
        evenT.sort()
        for i in range (len(oddS)) :
            if oddS[i] != oddT[i]:
                return "No"
        for i in range (len(evenS)) :
            if evenS[i] != evenT[i]:
                return "No"
        return "Yes"
if __name__ == '\_\_main\_\_':
    s="abcd"
    t="cdab"
    solution = Solution()
    print(" s与t分别为：", s, t)
    print(" 是否:", solution.isTwin(s, t))


```

运行结果：  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620111355972.png)  
 以上，这是今天的10个例子，大家消化一下吧。





