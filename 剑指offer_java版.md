###剑指offer JAVA版
[剑指offer JAVA版](https://yq.aliyun.com/articles/642728)

1. [二维数组中的查找](https://www.nowcoder.com/practice/abc3fe2ce8e146608e868a70efebf62e?spm=a2c4e.10696291.0.0.3b9619a4BHbfYK&tpId=13&tqId=11154&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

 **题目描述**
	   
    在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。
 
 **解题思路**
 
      
 
    /**
     * 方法一    右上角开始查找（左下角也是这样）
     * 分析
     * 1.当右上角的数大于要找的数，那么要找的数在该列的左边，col--；
     * 2.当右上角的数小于要找的数，那么要找的数在该行的下边，row++；
     * 3.当右上角的数等于要找的数，那么就返回true；
     *
     * @param target
     * @param array
     * @return
     */

    public static boolean find(int target, int[][] array) {
        int rows = array.length;
        if (rows == 0) {
            return false;
        }
        int cols = array[0].length;
        if (cols == 0) {
            return false;
        }

        int row = 0;
        int col = cols - 1;

        while (row < rows && col >= 0) {
            if (array[row][col] > target) {
                col--;
            } else if (array[row][col] < target) {
                row++;
            } else {
                return true;
            }
        }
        return false;
    }


    /**
     * 方法二    暴力法
     * 双重for循环遍历查找
     *
     * @param target
     * @param array
     * @return
     */
    public boolean find1(int target, int[][] array) {
        for (int i = 0; i < array.length; i++) {
            for (int j = 0; j < array[0].length; j++) {
                if (array[i][j] == target) {
                    return true;
                }
            }
        }
        return false;
    }
      

2. [替换空格](https://www.nowcoder.com/practice/4060ac7e3e404ad1a894ef3e17650423?spm=a2c4e.10696291.0.0.132919a4UbhRdJ&tpId=13&tqId=11155&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
3. [从尾到头打印链表](https://www.nowcoder.com/practice/d0267f7f55b3412ba93bd35cfa8e8035?spm=a2c4e.10696291.0.0.393919a4tAP4fH&tpId=13&tqId=11156&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
4. [重建二叉树](https://www.nowcoder.com/practice/8a19cbe657394eeaac2f6ea9b0f6fcf6?spm=a2c4e.10696291.0.0.12e419a4cOWiHX&tpId=13&tqId=11157&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
5. [用两个栈实现队列](https://www.nowcoder.com/practice/54275ddae22f475981afa2244dd448c6?spm=a2c4e.10696291.0.0.72cb19a4z5lS8c&tpId=13&tqId=11158&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
6. [旋转数组中的最小数字](https://www.nowcoder.com/practice/9f3231a991af4f55b95579b44b7a01ba?spm=a2c4e.10696291.0.0.615e19a4f9Vs52&tpId=13&tqId=11159&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
7. [斐波那契数列](https://www.nowcoder.com/practice/c6c7742f5ba7442aada113136ddea0c3?spm=a2c4e.10696291.0.0.11de19a4zp3w3m&tpId=13&tqId=11160&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
8. [二进制中1的个数](https://www.nowcoder.com/practice/8ee967e43c2c4ec193b040ea7fbb10b8?spm=a2c4e.10696291.0.0.3c0619a4kn2dMY&tpId=13&tqId=11164&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
9. [数值的整数次方](https://www.nowcoder.com/practice/1a834e5e3e1a4b7ba251417554e07c00?spm=a2c4e.10696291.0.0.9a5119a4CO537e&tpId=13&tqId=11165&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
10. [调整数组顺序使奇数位于偶数前面](https://www.nowcoder.com/practice/beb5aa231adc45b2a5dcc5b62c93f593?spm=a2c4e.10696291.0.0.7adf19a4qaxJ31&tpId=13&tqId=11166&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
11. [链表中倒数第k个结点](https://www.nowcoder.com/practice/529d3ae5a407492994ad2a246518148a?spm=a2c4e.10696291.0.0.10b919a4Q6SU4L&tpId=13&tqId=11167&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
12. [反转链表](https://www.nowcoder.com/practice/75e878df47f24fdc9dc3e400ec6058ca?spm=a2c4e.10696291.0.0.583f19a4QpVaNM&tpId=13&tqId=11168&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
13. [合并两个排序的链表](https://www.nowcoder.com/practice/d8b6b4358f774294a89de2a6ac4d9337?spm=a2c4e.10696291.0.0.1cc019a4K8IEH2&tpId=13&tqId=11169&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
14. [树的子结构](https://www.nowcoder.com/practice/6e196c44c7004d15b1610b9afca8bd88?spm=a2c4e.10696291.0.0.42d219a4uKU6t6&tpId=13&tqId=11170&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
15. [二叉树的镜像](https://www.nowcoder.com/practice/564f4c26aa584921bc75623e48ca3011?spm=a2c4e.10696291.0.0.69ba19a4RFAZZ0&tpId=13&tqId=11171&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
16. [顺时针打印矩阵](https://www.nowcoder.com/practice/9b4c81a02cd34f76be2659fa0d54342a?spm=a2c4e.10696291.0.0.6e4b19a4h3DKSK&tpId=13&tqId=11172&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
17. [包含min函数的栈](https://www.nowcoder.com/practice/4c776177d2c04c2494f2555c9fcc1e49?spm=a2c4e.10696291.0.0.57ef19a4SRohHC&tpId=13&tqId=11173&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
18. [栈的压入、弹出序列](https://www.nowcoder.com/practice/d77d11405cc7470d82554cb392585106?spm=a2c4e.10696291.0.0.6f9e19a4eOLJ1t&tpId=13&tqId=11174&tPage=2&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
19. [从上往下打印二叉树](https://www.nowcoder.com/practice/7fe2212963db4790b57431d9ed259701?spm=a2c4e.10696291.0.0.5bcf19a4N3Wl0U&tpId=13&tqId=11175&tPage=2&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
20. [二叉搜索树的后序遍历序列](https://www.nowcoder.com/practice/a861533d45854474ac791d90e447bafd?spm=a2c4e.10696291.0.0.655d19a4Vm9FVe&tpId=13&tqId=11176&tPage=2&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
21. [二叉树中和为某一值的路径](https://www.nowcoder.com/practice/b736e784e3e34731af99065031301bca?spm=a2c4e.10696291.0.0.8ba719a44kT9BM&tpId=13&tqId=11177&tPage=2&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
22. [复杂链表的复制](https://www.nowcoder.com/practice/f836b2c43afc4b35ad6adc41ec941dba?spm=a2c4e.10696291.0.0.488019a4f1CoIr&tpId=13&tqId=11178&tPage=2&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
23. [ 二叉搜索树与双向链表](https://www.nowcoder.com/practice/947f6eb80d944a84850b0538bf0ec3a5?spm=a2c4e.10696291.0.0.cfd319a4nbSvOO&tpId=13&tqId=11179&tPage=2&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
24. [字符串的排列](https://www.nowcoder.com/practice/fe6b651b66ae47d7acce78ffdd9a96c7?spm=a2c4e.10696291.0.0.51c719a48TJaOp&tpId=13&tqId=11180&tPage=2&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
25. [数组中出现次数超过一半的数字](https://www.nowcoder.com/practice/e8a1b01a2df14cb2b228b30ee6a92163?spm=a2c4e.10696291.0.0.247a19a40a4Nxp&tpId=13&tqId=11181&tPage=2&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
26. [最小的K个数](https://www.nowcoder.com/practice/6a296eb82cf844ca8539b57c23e6e9bf?spm=a2c4e.10696291.0.0.3b0f19a4AJvTOW&tpId=13&tqId=11182&tPage=2&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
27. [连续子数组的最大和](https://www.nowcoder.com/practice/459bd355da1549fa8a49e350bf3df484?spm=a2c4e.10696291.0.0.7cb719a4ZA8Xa7&tpId=13&tqId=11183&tPage=2&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
28. [整数中1出现的次数（从1到n整数中1出现的次数）](https://www.nowcoder.com/practice/bd7f978302044eee894445e244c7eee6?spm=a2c4e.10696291.0.0.106a19a4f2OtI7&tpId=13&tqId=11184&tPage=2&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
29. [把数组排成最小的数](https://www.nowcoder.com/practice/8fecd3f8ba334add803bf2a06af1b993?spm=a2c4e.10696291.0.0.796419a4abdXi8&tpId=13&tqId=11185&tPage=2&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
30. [丑数](https://www.nowcoder.com/practice/6aa9e04fc3794f68acf8778237ba065b?spm=a2c4e.10696291.0.0.aebc19a4HNT7f0&tpId=13&tqId=11186&tPage=2&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
31. [第一个只出现一次的字符](https://www.nowcoder.com/practice/1c82e8cf713b4bbeb2a5b31cf5b0417c?spm=a2c4e.10696291.0.0.191419a4Pez62w&tpId=13&tqId=11187&tPage=2&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
32. [数组中的逆序对](https://www.nowcoder.com/practice/96bd6684e04a44eb80e6a68efc0ec6c5?spm=a2c4e.10696291.0.0.6d6c19a4bMidWH&tpId=13&tqId=11188&tPage=2&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
33. [两个链表的第一个公共结点](https://www.nowcoder.com/practice/6ab1d9a29e88450685099d45c9e31e46?spm=a2c4e.10696291.0.0.4ecf19a4JzbTAY&tpId=13&tqId=11189&tPage=2&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
34. [数字在排序数组中出现的次数](https://www.nowcoder.com/practice/70610bf967994b22bb1c26f9ae901fa2?spm=a2c4e.10696291.0.0.7b6819a41WVMhF&tpId=13&tqId=11190&tPage=2&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
35. [二叉树的深度](https://www.nowcoder.com/practice/435fb86331474282a3499955f0a41e8b?spm=a2c4e.10696291.0.0.667019a4SSe99v&tpId=13&tqId=11191&tPage=2&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
36. [数组中只出现一次的数字](https://www.nowcoder.com/practice/e02fdb54d7524710a7d664d082bb7811?spm=a2c4e.10696291.0.0.f96319a4QuubLj&tpId=13&tqId=11193&tPage=2&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
37. [和为S的两个数字](https://www.nowcoder.com/practice/390da4f7a00f44bea7c2f3d19491311b?spm=a2c4e.10696291.0.0.147d19a4F661af&tpId=13&tqId=11195&tPage=3&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)    和    [和为S的连续正数序列](https://www.nowcoder.com/practice/c451a3fd84b64cb19485dad758a55ebe?spm=a2c4e.10696291.0.0.43f819a4pQoEYz&tpId=13&tqId=11194&tPage=3&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
38. [翻转单词顺序列](https://www.nowcoder.com/practice/3194a4f4cf814f63919d0790578d51f3?spm=a2c4e.10696291.0.0.53b719a4fkwsbc&tpId=13&tqId=11197&tPage=3&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)  和  [左旋转字符串](https://www.nowcoder.com/practice/12d959b108cb42b1ab72cef4d36af5ec?spm=a2c4e.10696291.0.0.8e4219a4vzRkSQ&tpId=13&tqId=11196&tPage=3&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
39. [扑克牌顺子](https://www.nowcoder.com/practice/762836f4d43d43ca9deb273b3de8e1f4?spm=a2c4e.10696291.0.0.397919a4vvSglS&tpId=13&tqId=11198&tPage=3&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
40. [孩子们的游戏(圆圈中最后剩下的数)](https://www.nowcoder.com/practice/f78a359491e64a50bce2d89cff857eb6?spm=a2c4e.10696291.0.0.658519a4OSpvIN&tpId=13&tqId=11199&tPage=3&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
41. [求1+2+3+...+n](https://www.nowcoder.com/practice/7a0da8fc483247ff8800059e12d7caf1?spm=a2c4e.10696291.0.0.52b119a4RgRKZj&tpId=13&tqId=11200&tPage=3&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
42. [不用加减乘除做加法](https://www.nowcoder.com/practice/59ac416b4b944300b617d4f7f111b215?spm=a2c4e.10696291.0.0.518119a4KUyAAK&tpId=13&tqId=11201&tPage=3&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
43. [把字符串转换成整数](https://www.nowcoder.com/practice/1277c681251b4372bdef344468e4f26e?spm=a2c4e.10696291.0.0.23ad19a4TcnqFc&tpId=13&tqId=11202&tPage=3&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
44. [数组中重复的数字](https://www.nowcoder.com/practice/623a5ac0ea5b4e5f95552655361ae0a8?spm=a2c4e.10696291.0.0.3b8919a4WtuBaU&tpId=13&tqId=11203&tPage=3&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
45. [构建乘积数组](https://www.nowcoder.com/practice/94a4d381a68b47b7a8bed86f2975db46?spm=a2c4e.10696291.0.0.297719a4Bj6tUN&tpId=13&tqId=11204&tPage=3&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
46. [正则表达式匹配](https://www.nowcoder.com/practice/45327ae22b7b413ea21df13ee7d6429c?spm=a2c4e.10696291.0.0.6b8619a4q7NBma&tpId=13&tqId=11205&tPage=3&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
47. [表示数值的字符串](https://www.nowcoder.com/practice/6f8c901d091949a5837e24bb82a731f2?spm=a2c4e.10696291.0.0.6d6419a4h7nCIZ&tpId=13&tqId=11206&tPage=3&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
48. [字符流中第一个不重复的字符](https://www.nowcoder.com/practice/00de97733b8e4f97a3fb5c680ee10720?spm=a2c4e.10696291.0.0.382019a430ySvD&tpId=13&tqId=11207&tPage=3&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
49. [链表中环的入口结点](https://www.nowcoder.com/practice/253d2c59ec3e4bc68da16833f79a38e4?spm=a2c4e.10696291.0.0.7a2819a4a0w0EX&tpId=13&tqId=11208&tPage=3&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
50. [删除链表中重复的结点](https://www.nowcoder.com/practice/fc533c45b73a41b0b44ccba763f866ef?spm=a2c4e.10696291.0.0.24ef19a4AexzGl&tpId=13&tqId=11209&tPage=3&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
51. [二叉树的下一个结点](https://www.nowcoder.com/practice/9023a0c988684a53960365b889ceaf5e?spm=a2c4e.10696291.0.0.4a5f19a4Wa07Mu&tpId=13&tqId=11210&tPage=3&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
52. [对称的二叉树](https://www.nowcoder.com/practice/ff05d44dfdb04e1d83bdbdab320efbcb?spm=a2c4e.10696291.0.0.214019a4aOLqNT&tpId=13&tqId=11211&tPage=3&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
53. [按之字形顺序打印二叉树](https://www.nowcoder.com/practice/91b69814117f4e8097390d107d2efbe0?spm=a2c4e.10696291.0.0.6c1719a43R8ruO&tpId=13&tqId=11212&tPage=3&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
54. [把二叉树打印成多行](https://www.nowcoder.com/practice/445c44d982d04483b04a54f298796288?spm=a2c4e.10696291.0.0.60bc19a47QlWYF&tpId=13&tqId=11213&tPage=3&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
55. [序列化二叉树](https://www.nowcoder.com/practice/cf7e25aa97c04cc1a68c8f040e71fb84?spm=a2c4e.10696291.0.0.421819a41eWetE&tpId=13&tqId=11214&tPage=4&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
56. [二叉搜索树的第k个结点](https://www.nowcoder.com/practice/ef068f602dde4d28aab2b210e859150a?spm=a2c4e.10696291.0.0.7d6119a4C8OLLV&tpId=13&tqId=11215&tPage=4&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
57. [数据流中的中位数](https://www.nowcoder.com/practice/9be0172896bd43948f8a32fb954e1be1?spm=a2c4e.10696291.0.0.60bc19a4KHJCaY&tpId=13&tqId=11216&tPage=4&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
58. [滑动窗口的最大值](https://www.nowcoder.com/practice/1624bc35a45c42c0bc17d17fa0cba788?spm=a2c4e.10696291.0.0.42b919a4QTmYuN&tpId=13&tqId=11217&tPage=4&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
59. [矩阵中的路径](https://www.nowcoder.com/practice/c61c6999eecb4b8f88a98f66b273a3cc?spm=a2c4e.10696291.0.0.226019a4SwB6nc&tpId=13&tqId=11218&tPage=4&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
60. [机器人的运动范围](https://www.nowcoder.com/practice/6e5207314b5241fb83f2329e89fdecc8?spm=a2c4e.10696291.0.0.2b6f19a4nsn1KW&tpId=13&tqId=11219&tPage=4&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)























