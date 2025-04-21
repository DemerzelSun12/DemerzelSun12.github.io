---
title: PAT刷题记录一
draft: false
math: true
date: 2020-09-01 15:04:34
cover: images/featureimages/11.jpg
summary: PAT甲级1001~1003
tags: [算法,PAT]
categories: [PAT]
---

## 前言

&emsp;&emsp;开个新坑，作为从小生活在北方的hiter，一直很向往南方的大学，高考很失败的没去上zju，那就研究生努力吧。看了一下，zju要求的上机测试可以用PAT甲级的成绩抵，那就开个刷题记录的新坑，之后可能也记录一些自己夏令营的经历。
&emsp;&emsp;初步先用 $Java$，之后转 $Python$ 吧。先说这些...

## 1001 A+B Format

### 题目

&emsp;&emsp;Calculate $a+b$ and output the sum in standard format -- that is, the digits must be separated into groups of three by commas (*unless there are less than four digits*).

<font size=5 >**Input Specification**</font>

&emsp;&emsp;Each input file contains one test case. Each case contains a pair of integers $a$ and $b$ where $-10^6\le a,b\le 10^6$​​ . The numbers are separated by a space.

<font size=5 >**Output Specification**</font>

&emsp;&emsp;For each test case, you should output the sum of $a$ and $b$ in one line. The sum must be written in the standard format.

<font size=5 >**Sample Input**</font>

    -1000000 9

<font size=5 >**Sample Output**</font>

    -999,991

### 题解

&emsp;&emsp;采用转化为字符串循环输出的方式，首先判断是否为负数。再对转化为字符串的结果绝对值进行判断，最前面的几位是否可以够3位，单独输出，最后把","和其后的三位数字在一起输出，循环切片即可。

```java
public class PAT1001 {
    public static void main(String[] args) {
        int a, b;
        Scanner sc = new Scanner(System.in);
        a = sc.nextInt();
        b = sc.nextInt();
        if (a + b < 0) {
            System.out.print("-");
        }
        String s = Integer.toString(Math.abs(a + b));
        int i = s.length() % 3 == 0 ? 3 : s.length() % 3;
        System.out.print(s.substring(0, i));
        for (; i < s.length(); i += 3) {
            System.out.print("," + s.substring(i, i + 3));
        }
    }
}
```

## 1002 A+B for Polynomials

### 题目

&emsp;&emsp;This time, you are supposed to find $A+B$ where $A$ and $B$ are two polynomials.

<font size=5 >**Input Specification**</font>

&emsp;&emsp;Each input file contains one test case. Each case occupies 2 lines, and each line contains the information of a polynomial:
$$
K\ N_1\ a_{N_k}\ N_2\ a_{N_2}\cdots N_K\ a_{N_K}
$$
where $K$ is the number of nonzero terms in the polynomial,​​ $N_i$ and $a_{N_i} \left(i=1,2,\cdots,K\right)$are the exponents and coefficients, respectively. It is given that $1\le K\le 10$，$0\le N_K \cdots <N_2<N_1\le 1000$.

<font size=5 >**Output Specification**</font>

&emsp;&emsp;For each test case you should output the sum of $A$ and $B$ in one line, with the same format as the input. Notice that there must be NO extra space at the end of each line. Please be accurate to $1$ decimal place.

<font size=5 >**Sample Input**</font>

    2 1 2.4 0 3.2
    2 2 1.5 1 0.5

<font size=5 >**Sample Output**</font>

    3 2 1.5 1 2.9 0 3.2

### 题解

&emsp;&emsp;采用HashMap存储指数与系数的关系，这里用到了`getOrDefault()`方法，避免第一次添加键值对时取出的value为`null`，还有一个坑，当两式的某项对消时，不仅不要显示相应的指数项，连相应的项数也要减少，就是这里卡了好久，3~6测试用例过不去。
<font size=5 >**Input**</font>

    2 2 0.1 0 0.1
    3 2 -0.2 1 0.1 0 -0.1
<font size=5 >**Output**</font>

    2 2 -0.1 1 0.1
才是正确的，而

    3 2 -0.1 1 0.1
是错误的。所以map会进行一次去除操作，可能有点麻烦了...orz

&emsp;&emsp;用`double`数据会有无法精准表示的问题，用`String.format()`方法保留一位小数，这个也是测试用例2的考察点。

&emsp;&emsp;上正确代码

```java
public class PAT1002 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        Map<Integer, Double> map = new HashMap<>();
        for (int k = 0; k < 2; k++) {
            int num = sc.nextInt();
            for (int i = 0; i < num; i++) {
                int exp = sc.nextInt();
                double coe = sc.nextDouble();
                map.put(exp, map.getOrDefault(exp, 0.0) + coe);
            }
        }
        Set<Integer> keyset = map.keySet();
        Object[] array = keyset.toArray();
        for (Object key : array) {
            if (map.get(key) == 0) {
                map.remove(key);
            }
        }
        System.out.print(map.size());
        Set<Integer> keyset2 = map.keySet();
        Object[] array2 = keyset2.toArray();
        Arrays.sort(array2, Collections.reverseOrder());
        for (Object key : array2) {
            System.out.print(" " + key + " " + String.format("%.1f", map.get(key)));
        }
    }
}


```

## 1003 Emergency

### 题目

&emsp;&emsp;As an emergency rescue team leader of a city, you are given a special map of your country. The map shows several scattered cities connected by some roads. Amount of rescue teams in each city and the length of each road between any pair of cities are marked on the map. When there is an emergency call to you from some other city, your job is to lead your men to the place as quickly as possible, and at the mean time, call up as many hands on the way as possible.

<font size=5 >**Input Specification**</font>

&emsp;&emsp;Each input file contains one test case. For each test case, the first line contains 4 positive integers: $N \left(\le 500\right)$ - the number of cities (and the cities are numbered from $0$ to $N−1$), $M$ - the number of roads, ​​$C_1 $ and $C_2$ - the cities that you are currently in and that you must save, respectively. The next line contains $N$ integers, where the i-th integer is the number of rescue teams in the $i$-th city. Then M lines follow, each describes a road with three integers $c_1,c_2$ and $L$, which are the pair of cities connected by a road and the length of that road, respectively. It is guaranteed that there exists at least one path from ​​$C_1$ to $C_2$ .

<font size=5 >**Output Specification**</font>

&emsp;&emsp;For each test case, print in one line two numbers: the number of different shortest paths between $C_1$ and $C_2$ , and the maximum amount of rescue teams you can possibly gather. All the numbers in a line must be separated by exactly one space, and there is no extra space allowed at the end of a line.

<font size=5 >**Sample Input**</font>

    5 6 0 2
    1 2 1 5 3
    0 1 1
    0 2 2
    0 3 1
    1 2 1
    2 4 1
    3 4 1

<font size=5 >**Sample Output**</font>

    2 4

### 题解

&emsp;&emsp;简单说一下题目的意思，是求一个从出发位置到目标位置的最短路径，同时搜集最短路径上所有的消防员，如果有多条最短路径相等，搜集多条路径上的所有消防员。
&emsp;&emsp;简单来说就是*dijkstra*的一个变形，在更新最短路径时同时更新到该点的救援队的最大数量，`shortest[i]`表示从源点到`i`点的最短路径，同时维护一个`man[i]`数组,表示到`i`点的消防员的数量。在更新最短路径时，增加一步相等判断，为的是增加消防员的数量。

&emsp;&emsp;上正确代码

```java
import java.util.*;

public class PAT1003 {
    static int MAX = 505;
    static int Max = 99999999;
    static int verNum;
    static int edgNum;
    static int sorPos;
    static int tarPos;
    static int[] shortest = new int[MAX];
    static int[] visited = new int[MAX];
    static int[] num = new int[MAX];
    static int[] man = new int[MAX];


    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        verNum = sc.nextInt();
        edgNum = sc.nextInt();
        sorPos = sc.nextInt();
        tarPos = sc.nextInt();
        int[] peoNum = new int[verNum];
        int[][] graph = new int[MAX][MAX];
        Arrays.fill(shortest, Max);
        for (int i = 0; i < MAX; i++) {
            for (int j = 0; j < MAX; j++) {
                graph[i][j] = Max;
            }
        }
        for (int i = 0; i < verNum; i++) {
            peoNum[i] = sc.nextInt();
        }

        for (int i = 0; i < edgNum; i++) {
            int x = sc.nextInt();
            int y = sc.nextInt();
            int z = sc.nextInt();
            graph[x][y] = z;
            graph[y][x] = z;
        }
        graph[sorPos][sorPos] = 0;
        dijstra(graph, sorPos, tarPos, peoNum);
    }


    public static void dijstra(int[][] matrix, int source, int target, int[] people) {
        shortest[source] = 0;
        num[source] = 1;
        man[source] = people[source];

        for (int i = 0; i < verNum; i++) {
            int k = search();
            if (k == -1) {
                break;
            }
            visited[k] = 1;
            for (int j = 0; j < verNum; j++) {
                if (visited[j] == 0 && matrix[k][j] != Max) {
                    if (shortest[j] > matrix[k][j] + shortest[k]) {
                        shortest[j] = matrix[k][j] + shortest[k];
                        num[j] = num[k];
                        man[j] = people[j] + man[k];
                    } else if (shortest[j] == matrix[k][j] + shortest[k]) {
                        num[j] += num[k];
                        if (people[j] + man[k] > man[j]) {
                            man[j] = people[j] + man[k];
                        }
                    }
                }
            }
        }
        System.out.println(num[target] + " " + man[target]);
    }

    public static int search() {
        int k = -1;
        int mmin = Max;
        for (int i = 0; i < verNum; i++) {
            if (visited[i] == 0 && shortest[i] < mmin) {
                k = i;
                mmin = shortest[i];
            }
        }
        return k;
    }
}


```
