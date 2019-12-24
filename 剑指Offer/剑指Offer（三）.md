
#####13. [合并两个排序的链表](https://www.nowcoder.com/practice/d8b6b4358f774294a89de2a6ac4d9337?spm=a2c4e.10696291.0.0.1cc019a4K8IEH2&tpId=13&tqId=11169&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

**题目描述**

	输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。

**解题思路**

	public static ListNode Merge(ListNode list1, ListNode list2) {
        ListNode head = new ListNode(0);
        ListNode cur = head;
        while (list1!=null && list2!=null){
            if(list1.val<list2.val){
                cur.next = list1;
                list1 = list1.next;
            }else {
                cur.next = list2;
                list2 = list2.next;
            }
            cur = cur.next;
        }
        if(list1!=null){
            cur.next = list1;
        }
        if(list2!=null){
            cur.next = list2;
        }
        return head.next;

    }



    public static class ListNode {
        int val;
        ListNode next;

        public ListNode(int val) {
            this.val = val;
        }
    }

#####14. [树的子结构](https://www.nowcoder.com/practice/6e196c44c7004d15b1610b9afca8bd88?spm=a2c4e.10696291.0.0.42d219a4uKU6t6&tpId=13&tqId=11170&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

**题目描述**

	输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）

**解题思路**

	
    
    /**
     * 
     * @param root1
     * @param root2
     * @return root2是不是root1的子树
     */
     
    public static boolean hasSubTree(TreeNode root1, TreeNode root2) {
        if (root1 == null || root2 == null) {
            return false;
        }
        return judgeSubTree(root1, root2) ||
                judgeSubTree(root1.left, root2) ||
                judgeSubTree(root1.right, root2);
    }

    private static boolean judgeSubTree(TreeNode root1, TreeNode root2) {
        if (root2 == null) {
            return true;
        }
        if (root1 == null) {
            return false;
        }
        if (root1.val != root2.val) {
            return judgeSubTree(root1.left, root2) ||
                    judgeSubTree(root1.right, root2);
        }
        return judgeSubTree(root1.left, root2.left) &&
                judgeSubTree(root1.right, root2.right);
    }
    
    public static class TreeNode {
        int val;
        TreeNode left;
        TreeNode right;

        public TreeNode(int val) {
            this.val = val;
        }
    }

#####15. [二叉树的镜像](https://www.nowcoder.com/practice/564f4c26aa584921bc75623e48ca3011?spm=a2c4e.10696291.0.0.69ba19a4RFAZZ0&tpId=13&tqId=11171&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

**题目描述**

	操作给定的二叉树，将其变换为源二叉树的镜像。
	输入描述:
	二叉树的镜像定义：源二叉树
	8
	/  \
	6   10
	/ \  / \
	5  7 9 11
	镜像二叉树
	8
	/  \
	10   6
	/ \  / \
	11 9 7  5

	
**解题思路**

	public static void Mirror(TreeNode root) {
        if (root == null) {
            return;
        }
        TreeNode temp = root.left;
        root.left = root.right;
        root.right = temp;
        Mirror(root.left);
        Mirror(root.right);

    }

    public static class TreeNode {
        int val;
        TreeNode left;
        TreeNode right;

        public TreeNode(int val) {
            this.val = val;
        }
    }


#####16. [顺时针打印矩阵](https://www.nowcoder.com/practice/9b4c81a02cd34f76be2659fa0d54342a?spm=a2c4e.10696291.0.0.6e4b19a4h3DKSK&tpId=13&tqId=11172&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

**题目描述**

	输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字，例如，如果输入如下4 X 4
	矩阵： 
	1  2  3  4 
	5  6  7  8 
	9  10 11 12 
	13 14 15 16
	
	则依次打印出数字
	1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10.
	
**解题思路**

	public class Client {
    	public static void main(String[] args) {
    	    int arr[][] = {
        	        {1, 2, 3, 4},
        	        {5, 6, 7, 8},
        	        {9, 10, 11, 12},
        	        {13, 14, 15, 16}};
     	   ArrayList<Integer> result = printMatrix(arr);
     	   StringBuilder stringBuilder = new StringBuilder();
     	   for (int i = 0; i < result.size(); i++) {
     	       stringBuilder.append(result.get(i)).append(",");
     	   }
     	   System.out.println(stringBuilder.toString());
    	}

    	public static ArrayList<Integer> printMatrix(int[][] matrix) {
    	    ArrayList<Integer> result = new ArrayList<>();
    	    if (matrix == null) {
    	        return result;
    	    }
    	    int left = 0;
    	    int top = 0;
    	    int right = matrix[0].length - 1;
    	    int bottom = matrix.length - 1;


    	    while (left <= right && top <= bottom) {
        	    //最上面一行，由左向右遍历
        	    for (int i = left; i <= right; i++) {
        	        result.add(matrix[top][i]);
        	    }

        	    //最右面一列，由上向下遍历
        	    for (int i = top + 1; i <= bottom; i++) {
        	        result.add(matrix[i][right]);
        	    }

        	    //最下面一行，由右向左遍历
        	    if (top < bottom) {
        	        for (int i = right - 1; i >= left; i--) {
        	            result.add(matrix[bottom][i]);
        	        }
        	    }

        	    //最左面一列，由下向上遍历
        	    if (left < right) {
        	        for (int i = bottom - 1; i >= top + 1; i--) {
        	            result.add(matrix[i][left]);
        	        }
        	    }

        	    left++;
        	    top++;
        	    right--;
        	    bottom--;

        	}
        	return result;
    	}
	}

#####17. [包含min函数的栈](https://www.nowcoder.com/practice/4c776177d2c04c2494f2555c9fcc1e49?spm=a2c4e.10696291.0.0.57ef19a4SRohHC&tpId=13&tqId=11173&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

**题目描述**

**解题思路**

	public class Client {
	    private static Stack<Integer> stack = new Stack<>();
	    private static Integer minElement = Integer.MAX_VALUE;

	    public static void main(String[] args) {
	        int[] arr = {1, 2, 3, 4, 8, 12, 0, 15, 14, 13, 9, 5, 6, 7, 11, 10};
	        for (int i = 0; i < arr.length; i++) {
	            push(arr[i]);
	        }
	        System.out.println("栈中的最小数值是：" + min());

	    }

	    /**
	     * 向栈中添加数据时，如果添加的数据最小，先将之前最小的数据添加入栈，然后将所要添加的数据赋值给最小值minElement，再将要添加的数据添加入栈
	     * <p>
	     * 也就是说，如果要添加的数据最小，那就一次添加两个数据，之前最小数据和当前要添加数据
	     *
	     * @param node
	     */
	    public static void push(int node) {
	        if (stack.empty()) {
	            minElement = node;
	            stack.push(node);
	        } else {
	            if (node <= minElement) {
	                stack.push(minElement);
	                minElement = node;
	            }
	            stack.push(node);
	        }
	    }
	
	    /**
	     * 取数据时，如果栈顶是最小数据，并且栈Size>1，那要pop两次
	     */
	    public static void pop() {
	        if (stack.size() == 0) {
	            return;
	        }
	        if (minElement == stack.peek()) {
	            if (stack.size() > 1) {
	                stack.pop();
                minElement = stack.peek();
            	} else {
                	minElement = Integer.MAX_VALUE;
            	}

        	}
        	stack.pop();
    	}

    	public static int top() {
        	return stack.peek();
    	}

    	public static int min() {
        	return minElement;        
    	}

	}

#####18. [栈的压入、弹出序列](https://www.nowcoder.com/practice/d77d11405cc7470d82554cb392585106?spm=a2c4e.10696291.0.0.6f9e19a4eOLJ1t&tpId=13&tqId=11174&tPage=2&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

**题目描述**

	输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否可能为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如序列1,2,3,4,5是某栈的压入顺序，序列5,4,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。（注意：这两个序列的长度是相等的）
	
**解题思路**

	public class Client {
    
        public static void main(String[] args) {
            int[] pushA = {1, 2, 3, 4, 5};
            int[] popA = {5, 3, 4, 2, 1};
            System.out.println(Arrays.toString(popA) + (IsPopOrder01(pushA, popA) ? "是" : "不是") + Arrays.toString(pushA) + "的弹栈序列");
            System.out.println(Arrays.toString(popA) + (IsPopOrder02(pushA, popA) ? "是" : "不是") + Arrays.toString(pushA) + "的弹栈序列");
        }
    
        /**
         * 利用栈
         *
         * @param pushA
         * @param popA
         * @return
         */
        public static boolean IsPopOrder01(int[] pushA, int[] popA) {
            if (pushA == null || popA == null || pushA.length != popA.length) {
                return false;
            }
    
            Stack<Integer> stack = new Stack<>();
            for (int i = 0; i < pushA.length; i++) {
                stack.push(pushA[i]);
            }
            for (int i = 0; i < pushA.length; i++) {
                if (stack.pop() != popA[i]) {
                    return false;
                }
            }
            return true;
    
        }
    
    
        /**
         * 利用数组自身
         *
         * @param pushA
         * @param popA
         * @return
         */
        public static boolean IsPopOrder02(int[] pushA, int[] popA) {
            if (pushA == null || popA == null || pushA.length != popA.length) {
                return false;
            }
            int length = pushA.length;
            for (int i = 0; i < length; i++) {
                if (pushA[i] != popA[length - 1 - i]) {
                    return false;
                }
            }
            return true;
    
        }
    }


