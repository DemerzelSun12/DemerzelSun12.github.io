---
title: PAT刷题记录二
draft: false
math: true
date: 2020-09-07 15:04:34
cover: images/featureimages/10.jpg
summary: PAT甲级1004~1006
tags: [算法,PAT]
categories: [PAT]
---

## 1004 Counting Leaves

### 题目

&emsp;&emsp;A family hierarchy is usually presented by a pedigree tree. Your job is to count those family members who have no child.

<font size=5 >**Input Specification**</font>

&emsp;&emsp;Each input file contains one test case. Each case starts with a line containing $0<N<100$, the number of nodes in a tree, and $M (<N)$, the number of non-leaf nodes. Then $M$ lines follow, each in the format:
``ID K ID[1] ID[2] ... ID[K]``

where `ID` is a two-digit number representing a given non-leaf node, `K` is the number of its children, followed by a sequence of two-digit `ID`'s of its children. For the sake of simplicity, let us fix the root ID to be `01`.

&emsp;&emsp;The input ends with $N$ being $0$. That case must NOT be processed.

<font size=5 >**Output Specification**</font>

&emsp;&emsp;For each test case, you are supposed to count those family members who have no child **for every seniority level** starting from the root. The numbers must be printed in a line, separated by a space, and there must be no extra space at the end of each line.

&emsp;&emsp;The sample case represents a tree with only 2 nodes, where `01` is the root and `02` is its only child. Hence on the root `01` level, there is `0` leaf node; and on the next level, there is `1` leaf node. Then we should output `0 1` in a line.

<font size=5 >**Sample Input**</font>

    2 1
    01 1 02

<font size=5 >**Sample Output**</font>

    0 1

### 题解

&emsp;&emsp;总体思想是将树结构存储，层序遍历每一层，输出每一层中没有子节点的节点数量。和一般的`bfs`不太一样。我的做法是，统计完一层删除一层，同时将所有以这层节点为父节点的节点的父节点置空。没有类似网上其他题解用的矩阵存储，我自己写了一棵树，然后依次读入节点，这样的好处是无论从哪一层的节点开始输入，构建的树是唯一的，网上其他好多题解要求第一层输入一定为`01`节点的子节点，其实不需要这样。自己写树虽然麻烦了一点，但是我真的不喜欢用二维矩阵...orz
&emsp;&emsp;还有一个坑，所有节点都没有叶节点时，应该直接输出节点的个数，也是测试用例2（有测试用例0）的考察点。（疯狂踩测试用例挖的坑，不愧是我）...

```java
import java.util.*;

public class PAT1004 {

    static List<TreeNode> nodes = new ArrayList<>();

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int nodeNum = sc.nextInt();
        int noLeafNum = sc.nextInt();
        if (nodeNum == 0 || noLeafNum >= nodeNum) {
            return;
        }
        if (noLeafNum==0){
            System.out.print(nodeNum);
            return;
        }
        for (int i = 0; i < noLeafNum; i++) {
            String id = sc.next();
            int childNum = sc.nextInt();
            TreeNode treeNode = findTreeNode(id);
            if (treeNode == null) {
                treeNode = new TreeNode(id);
                nodes.add(treeNode);
            }
            List<TreeNode> childList = new ArrayList<>();
            for (int j = 0; j < childNum; j++) {
                String childID = sc.next();
                TreeNode childNode = findTreeNode(childID);
                if (childNode == null) {
                    childNode = new TreeNode(childID);
                    nodes.add(childNode);
                }
                childNode.setParentNode(treeNode);
                childList.add(childNode);
            }
            treeNode.setChildNodes(childList);
        }
        Queue<TreeNode> queue = new LinkedList<>();
        addNodes(queue);
        int num = 0;
        while (!queue.isEmpty()) {
            TreeNode tr = queue.poll();
            if (tr.getChildNodes().size() == 0) {
                num++;
            }
            for (TreeNode node : nodes) {
                if (node.getParentNode() != null && node.getParentNode().getId().equals(tr.getId())) {
                    node.setParentNode(null);
                }
            }
            nodes.remove(tr);
        }
        System.out.print(num);
        while (nodes.size() != 0) {
            addNodes(queue);
            int noChildNum = 0;
            while (!queue.isEmpty()) {
                TreeNode tr = queue.poll();
                if (tr.getChildNodes().size() == 0) {
                    noChildNum++;
                }
                for (TreeNode node : nodes) {
                    if (node.getParentNode() != null && node.getParentNode().getId().equals(tr.getId())) {
                        node.setParentNode(null);
                    }
                }
                nodes.remove(tr);
            }
            System.out.print(" " + noChildNum);
        }

    }

    public static TreeNode findTreeNode(String id) {
        for (TreeNode node : nodes) {
            if (id.equals(node.getId())) {
                return node;
            }
        }
        return null;
    }

    public static void addNodes(Queue<TreeNode> queue) {
        for (TreeNode node : nodes) {
            if (node.getParentNode() == null) {
                queue.add(node);
            }
        }
    }
}

class TreeNode {
    private String id;
    private TreeNode parentNode;
    private List<TreeNode> childNodes;

    public TreeNode(String id) {
        this.id = id;
        childNodes = new ArrayList<>();
        parentNode = null;
    }

    public void setChildNodes(List<TreeNode> childNodes) {
        this.childNodes = childNodes;
    }

    public void setParentNode(TreeNode parentNode) {
        this.parentNode = parentNode;
    }

    public String getId() {
        return id;
    }

    public List<TreeNode> getChildNodes() {
        return childNodes;
    }

    public TreeNode getParentNode() {
        return parentNode;
    }
}
```

## 1005 Spell It Right

### 题目

&emsp;&emsp;Given a non-negative integer $N$, your task is to compute the sum of all the digits of $N$, and output every digit of the sum in English.

<font size=5 >**Input Specification**</font>

&emsp;&emsp;Each input file contains one test case. Each case occupies one line which contains an $N \left(≤10​^{100}\right)$.

<font size=5 >**Output Specification**</font>

&emsp;&emsp;For each test case, output in one line the digits of the sum in English words. There must be one space between two consecutive words, but no extra space at the end of a line.

<font size=5 >**Sample Input**</font>

    12345

<font size=5 >**Sample Output**</font>

    one five

### 题解

&emsp;&emsp;没啥说的，很简单的一道题，和前面的不是一个等级的...字符串读入，分割成字符数组，再转化为数字，逐位相加，再转换回字符串，再转换成字符数组，由于输出格式的要求，先输出首位，再循环输出后面的。唯一注意的一点，数字必须用字符串读入，`int`和`long`都是不够的，或者用`BigInteger`，这个没操作，不知道能不能通过。

```java
import java.util.Scanner;

public class PAT1005 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String number = sc.next();
        int sum = 0;
        String[] arr = number.split("");
        for (String s : arr) {
            sum += Integer.valueOf(s);
        }
        String[] arr2 = String.valueOf(sum).split("");
        System.out.print(outPut(arr2[0]));
        for (int i = 1; i < arr2.length; i++) {
            System.out.print(" " + outPut(arr2[i]));
        }
    }

    public static String outPut(String num) {
        switch (num) {
            case "1":
                return "one";
            case "2":
                return "two";
            case "3":
                return "three";
            case "4":
                return "four";
            case "5":
                return "five";
            case "6":
                return "six";
            case "7":
                return "seven";
            case "8":
                return "eight";
            case "9":
                return "nine";
            case "0":
                return "zero";
        }
        return null;
    }
}
```

## 1006 Sign In and Sign Out

### 题目

&emsp;&emsp;At the beginning of every day, the first person who signs in the computer room will unlock the door, and the last one who signs out will lock the door. Given the records of signing in's and out's, you are supposed to find the ones who have unlocked and locked the door on that day.

<font size=5 >**Input Specification**</font>

&emsp;&emsp;Each input file contains one test case. Each case contains the records for one day. The case starts with a positive integer $M$, which is the total number of records, followed by $M$ lines, each in the format:
    ``ID_number Sign_in_time Sign_out_time``
where times are given in the format `HH:MM:SS`, and `ID_number` is a string with no more than 15 characters.

<font size=5 >**Output Specification**</font>

&emsp;&emsp;For each test case, output in one line the ID numbers of the persons who have unlocked and locked the door on that day. The two ID numbers must be separated by one space.

&emsp;&emsp;Note: It is guaranteed that the records are consistent. That is, the sign in time must be earlier than the sign out time for each person, and there are no two persons sign in or out at the same moment.

<font size=5 >**Sample Input**</font>

    3
    CS301111 15:30:28 17:00:10
    SC3021234 08:00:00 11:25:25
    CS301133 21:45:00 21:58:40

<font size=5 >**Sample Output**</font>

    SC3021234 CS301133

### 题解

&emsp;&emsp;感谢上学期的软件构造的*lab2*，学会了对时间的切片操作，要不然这道题考试的时候出了绝对不会。思路就是用`SimpleDateFormat`类对读入的字符串切片，获取时间；`Date`类型存储时间，使用`Date`类型的`getTime()`方法比较时间的先后，不停比较更新就行了。
&emsp;&emsp;这里有一个点，使用一个空格对字符串切片时应该使用`\\s`，`+`的意思是多个空格，而不应该使用`split(" ")`。其他没啥注意事项了。

```java
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Scanner;

public class PAT1006 {
    public static void main(String[] args) throws ParseException {
        String early = null;
        String late = null;
        SimpleDateFormat sdf = new SimpleDateFormat("HH:mm:ss");
        Scanner sc = new Scanner(System.in);
        Date earlyTime = new Date(86400000);
        Date lateTime = new Date(0);
        int num = sc.nextInt();
        sc.nextLine();
        for (int i = 0; i < num; i++) {
            String readline = sc.nextLine();
            String[] arr = readline.split("\\s+");
            Date earlyDate = sdf.parse(arr[1]);
            Date lateDate = sdf.parse(arr[2]);
            if (earlyDate.getTime() < earlyTime.getTime()) {
                earlyTime = earlyDate;
                early = arr[0];
            }
            if (lateDate.getTime() > lateTime.getTime()) {
                lateTime = lateDate;
                late = arr[0];
            }
        }
        System.out.println(early + " " + late);
    }
}


```
