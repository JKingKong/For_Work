# Java笔试常用格式

## 算法输入输出格式

### 大致架构

```java
import java.util.*;  //导入标准库

public class Main{ //主类
    
    //主函数
    public static void main(String[] args){
        //打开输入流
        Scanner sc = new Scanner(System.in);
        //获取输入流中的值
        int n = sc.nextInt();
        int m = sc.nextInt();
        
        fun(n, m); //静态方法
        sc.close();
    }
    
 	//静态方法方便调用
    public static void fun(int n, int m){
        int[] nums = new int[]{n , m};
        System.out.println(nums[(n - m) >>> 31]);
    }
}
```

### Scanner常用方法：

```java
// 输入流
Scanner sc = new Scanner(System.in); 
//获取一个整数
int a = sc.nextInt();

double b = sc.nextDouble();
//是否还有下一个数
boolean isHas = sc.hasNext();
//获取一行(以以Enter为结束符,可以获得空白)
String line = sc.nextLine();


//关闭流：
sc.close();

//写算法要有输出：
//格式化输出
System.out.printf("%d个数的和为%f\n",m,sum);
System.out.printf("%d个数的平均值是%f\n",m,sum/m);
```

`String s = sc.next()`:

```java
1、一定要读取到有效字符后才可以结束输入。
2、对输入有效字符之前遇到的空白，next() 方法会自动将其去掉。
3、只有输入有效字符后才将其后面输入的空白作为分隔符或者结束符。
next() 不能得到带有空格的字符串。
```

### 写算法流程

1. 导入标准包
2. 写主类
3. 写主方法
   1. 打开输入流
   2. 从输入流中获取数字、字符串等输入
   3. 调用静态算法处理方法
   4. 输出结构
4. 静态算法处理方法



# 常用API

## 数据结构

### 栈

```
Stack<Integer> s = new Stack<>();
s.pop();
s.push(value);
s.peek();
s.isEmpty()
```

### 队列

```java
LinkedList<Integer> q = new LinkedList<>();
q.isEmpty();
q.offerLast(value); 入队
q.pollFirst();  出队
q.peekFirst();  查看队头
```

### 优先队列

```
//1，默认实现的是最小堆
PriorityQueue<Integer> minHeap = new PriorityQueue<Integer>();
// 最大堆：

//2，通过比较器排序，实现最大堆
PriorityQueue<Integer> maxHeap = new PriorityQueue<Integer>(new Comparator<Integer>() {
			@Override
			public int compare(Integer o1, Integer o2) {
				/**以下是对比较器升序、降序的理解.
				 *(1) 写成return o1.compareTo(o2) 或者 return o1-o2表示升序
				 *(2) 写成return o2.compareTo(o1) 或者return o2-o1表示降序
				 */
				return o2.compareTo(o1);
			}
});

PriorityQueue<Integer> priorityQueue = new PriorityQueue<>((a, b) -> {
    return b - a;  // a为原后 b为原前
});

                                                            
注意4：插入方法（offer()、poll()、remove() 、add() 方法）时间复杂度为O(log(n)) ；
remove(Object) 和 contains(Object) 时间复杂度为O(n)；
检索方法（peek、element 和 size）时间复杂度为常量。
                                                            
			
```

## 集合

### List

1. 数组---->ArrayList

   ```java
   ArrayList<Element> arrayList = new ArrayList<Element>(Arrays.asList(array));
   
   List<Element> list = Arrays.asList(array);
   ```

2. ArrayList---->数组

   ```java
   
   ```

   

3. 

## 数组

### 二维不定列长度

```java
ArrayList<int[]> res = new ArrayList<>();
res.toArray(new int[0][]);
```



## 字符串

1. String to Char

2.  String.valueOf(char[] )

3. s.split(String)

   ```java
   String s = "  hello world!  ";
   String[] ss = s.split(" ");   ---> {"","","hello","world!"}
   ss[i].equals(""); -->true
   ```




头尾去除使用while判断



# 常用算法

### 异或做加法

```java
class Solution {
    public int add(int a, int b) {
        int res = 0;
        int carry = 0;
        do{
            res = (a ^ b);
            carry = (a & b) << 1;
            a = res;
            b = carry;
        }while(carry!=0);
        return res;
    }
}
```

### 短路原理求和sum(n):

```java
class Solution {
    
    public int sumNums(int n) {
        int sum = n;
        boolean isContinueRun =  (n > 0) && ((sum += sumNums(n-1)) > 0); 
        return sum;
   }
}
```

### 归并排序

```java
class Solution {
    int[] tmp;
    public void reversePairs(int[] nums) {
        tmp = new int[nums.length];
        mergeSort(nums,0,nums.length-1);
        return;
    }
    private void mergeSort(int[] nums,int low,int high){
        if(high-low < 1) return;
        int m = low + (high-low)/2;
        mergeSort(nums,low,m);
        mergeSort(nums,m+1,high);
        merge(nums,low,m,high);
    }
    private void merge(int[] nums,int low,int m,int high){
        int i=low,j=m+1;
        int Idx = low;
        while(i<=m || j <= high){
            if(i > m) tmp[Idx] = nums[j++];
            else if(j > high) tmp[Idx] = nums[i++];
            else if(nums[i] <= nums[j]) tmp[Idx] = nums[i++];
            else if(nums[i] > nums[j]){
                tmp[Idx] = nums[j++];
            }
            Idx++;
        }
        for(int k=low;k<=high;k++){
            nums[k] = tmp[k];
        }
    }

}
```



### 快速排序





## 问题

1. 单调栈
2. 约瑟夫环问题



