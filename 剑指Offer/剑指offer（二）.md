###剑指Offer（二）
#####7 [斐波那契数列](https://www.nowcoder.com/practice/c6c7742f5ba7442aada113136ddea0c3?spm=a2c4e.10696291.0.0.11de19a4zp3w3m&tpId=13&tqId=11160&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

**题目描述**

	  大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0）。
	  
	  斐波那契数列（Fibonacci sequence），又称黄金分割数列、因数学家列昂纳多·斐波那契（Leonardoda Fibonacci）以兔子繁殖为例子而引入，故又称为“兔子数列”，指的是这样一个数列：1、1、2、3、5、8、13、21、34、……在数学上，斐波那契数列以如下被以递推的方法定义：F(1)=1，F(2)=1, F(n)=F(n-1)+F(n-2)（n>=3，n∈N*）在现代物理、准晶体结构、化学等领域，斐波纳契数列都有直接的应用，为此，美国数学会从1963年起出版了以《斐波纳契数列季刊》为名的一份数学杂志，用于专门刊载这方面的研究成果
	  
**解题思路**

	/**
     * 方法一  递归法
     * <p>
     * 斐波那契数列的标准公式为：F(1)=1，F(2)=1, F(n)=F(n-1)+F(n-2)（n>=3，n∈N*）
     * 根据公式可以直接写出
     *
     * @param n
     * @return
     */
    public static int Fibonacci01(int n) {
        if (n <= 1) {
            return n;
        }
        return Fibonacci01(n - 1) + Fibonacci01(n - 2);
    }


    /**
     * 方法二  优化递归
     * <p>
     * 递归会重复计算大量相同数据，我们用个数组把结果存起来8！
     *
     * @param n
     * @return
     */
    public static int Fibonacci02(int n) {
        int res[] = new int[40];
        res[0] = 0;
        res[1] = 1;
        for (int i = 2; i <= n; i++) {
            res[i] = res[i - 1] + res[i - 2];
        }
        return res[n];
    }


    /**
     * 方法三  优化存储
     * <p>
     * 其实我们可以发现每次就用到了最近的两个数，所以我们可以只存储最近的两个数
     * sum 存储第 n 项的值
     * one 存储第 n-1 项的值
     * two 存储第 n-2 项的值
     *
     * @param n
     * @return
     */
    public static int Fibonacci03(int n) {
        if (n == 0) {
            return 0;
        } else if (n == 1) {
            return 1;
        }
        int sum = 0;
        int two = 0;
        int one = 1;
        for (int i = 2; i <= n; i++) {
            sum = two + one;
            two = one;
            one = sum;
        }
        return sum;

    }


    /**
     * 方法四  对方法三持续优化
     *
     * @param n
     * @return
     */
    public static int Fibonacci04(int n) {
        if (n == 0) {
            return 0;
        } else if (n == 1) {
            return 1;
        }
        int sum = 1;
        int temp = 0;
        for (int i = 2; i <= n; i++) {
            sum = sum + temp;
            temp = sum - temp;
        }
        return sum;

    }
    
#####8. [二进制中1的个数](https://www.nowcoder.com/practice/8ee967e43c2c4ec193b040ea7fbb10b8?spm=a2c4e.10696291.0.0.3c0619a4kn2dMY&tpId=13&tqId=11164&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

**题目描述**

	输入一个整数，输出该数二进制表示中1的个数。其中负数用补码表示。
	
	如果一个整数不为0，那么这个整数至少有一位是1。如果我们把这个整数减1，那么原来处在整数最右边的1就会变为0，原来在1后面的所有的0都会变成1(如果最右边的1后面还有0的话)。其余所有位将不会受到影响。
	
    举个例子：一个二进制数1100，从右边数起第三位是处于最右边的一个1。减去1后，第三位变成0，它后面的两位0变成了1，而前面的1保持不变，因此得到的结果是1011.我们发现减1的结果是把最右边的一个1开始的所有位都取反了。这个时候如果我们再把原来的整数和减去1之后的结果做与运算，从原来整数最右边一个1那一位开始所有位都会变成0。如1100&1011=1000.也就是说，把一个整数减去1，再和原整数做与运算，会把该整数最右边一个1变成0.那么一个整数的二进制有多少个1，就可以进行多少次这样的操作。

**解题思路**

	 public static int NumberOf1(int n) {
        int count = 0;
        while (n != 0) {
            count++;
            n = n & (n - 1);
        }
        return count;
    }
    
#####9. [数值的整数次方](https://www.nowcoder.com/practice/1a834e5e3e1a4b7ba251417554e07c00?spm=a2c4e.10696291.0.0.9a5119a4CO537e&tpId=13&tqId=11165&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

**题目描述**

	 给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。保证base和exponent不同时为0
	 
**解题思路**

	public static double Power(double base, int exponent) {
        if (base == 0.0) {
            return 0.0;
        }
        if (exponent == 0) {
            return 1.0;
        }
        double res = 1.0;
        for (int i = 0; i < Math.abs(exponent); i++) {
            res *= base;
        }
        return exponent > 0 ? res : 1 / res;
    }

#####10. [调整数组顺序使奇数位于偶数前面](https://www.nowcoder.com/practice/beb5aa231adc45b2a5dcc5b62c93f593?spm=a2c4e.10696291.0.0.7adf19a4qaxJ31&tpId=13&tqId=11166&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

**题目描述**

	输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于
	数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。

**解题思路**

	public static void reOrderArray(int[] array) {
        for (int i = 0; i < array.length; i++) {
            for (int j = 1; j < array.length; j++) {
                if (array[j] % 2 != 0 && array[j - 1] % 2 == 0) {
                    int temp = array[j - 1];
                    array[j - 1] = array[j];
                    array[j] = temp;
                }
            }
        }
    }

#####11. [链表中倒数第k个结点](https://www.nowcoder.com/practice/529d3ae5a407492994ad2a246518148a?spm=a2c4e.10696291.0.0.10b919a4Q6SU4L&tpId=13&tqId=11167&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

**题目描述**

	输入一个链表，输出该链表中倒数第k个结点。

**解题思路**

	/**
     * 方法1
     * 先获取链表的总长度count，然后倒数第k个节点就是正数第count-k个节点
     *
     * @param head
     * @param k
     * @return
     */

    public static ListNode FindKthToTail01(ListNode head, int k) {
        int count = 0;
        ListNode temp = head;
        while (temp != null) {
            count++;
            temp = temp.next;
        }
        if (count < k) {
            return null;
        }
        ListNode resultNode = head;
        for (int i = 0; i < count - k; i++) {
            resultNode = resultNode.next;
        }
        return resultNode;
    }


    /**
     * 方法二
     * 利用mArrayList.add(0, temp);将ListNode节点逆序存进ArrayList中，然后倒数第k个节点就是ArrayList的正数第k个节点
     *
     * @param head
     * @param k
     * @return
     */
    public static ListNode FindKthToTail02(ListNode head, int k) {
        ArrayList<ListNode> mArrayList = new ArrayList<>();
        ListNode temp = head;
        while (temp != null) {
            mArrayList.add(0, temp);
            temp = temp.next;
        }
        ListNode resultNode = null;
        if (mArrayList.size() >= k) {
            for (int i = 0; i < mArrayList.size(); i++) {
                //i与k相差1
                if (i == k - 1) {
                    resultNode = mArrayList.get(i);
                }
            }
        }

        return resultNode;
    }

    public static class ListNode {
        int val;
        ListNode next;

        public ListNode(int val) {
            this.val = val;
        }
    }

#####12. [反转链表](https://www.nowcoder.com/practice/75e878df47f24fdc9dc3e400ec6058ca?spm=a2c4e.10696291.0.0.583f19a4QpVaNM&tpId=13&tqId=11168&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

**题目描述**
	
	输入一个链表，反转链表后，输出新链表的表头。

**解题思路**

	public static ListNode ReverseList(ListNode head){
        ListNode newHead = new ListNode(0);
        ListNode cur = head;
        ListNode next = null;
        while (cur!=null){
            next = cur.next;
            cur.next = newHead.next;
            newHead.next = cur;
            cur = next;
        }
        return newHead.next;
    }



