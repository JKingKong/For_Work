# Array.Sort()探秘

## Array.sort(Object[] a)

1、此方法默认升序排列

```java
    public static void sort(Object[] a) {
        if (LegacyMergeSort.userRequested)
            legacyMergeSort(a);
        else
            //执行这个，上边的到时候jdk升级就不在支持
            ComparableTimSort.sort(a, 0, a.length, null, 0, 0);
    }
```

2、static void sort(Object[] a, int lo, int hi, Object[] work, int workBase, int workLen) 

```java
//当传入的长度较小时，执行二分插入排序
        if (nRemaining < MIN_MERGE) {
            //先对数组的前“一些部分”进行升序处理，即：降序的“前边序列”翻转成升序的
            int initRunLen = countRunAndMakeAscending(a, lo, hi);
            
            binarySort(a, lo, hi, lo + initRunLen);
            return;
        }


    private static int countRunAndMakeAscending(Object[] a, int lo, int hi) {
        //省略代码//
        
		//以下为函数核心代码，其他部分省略
        // Find end of run, and reverse range if descending
        //a[runHi++]是比较数   a[lo]是被比较数
        if (((Comparable) a[runHi++]).compareTo(a[lo]) < 0) { // Descending
            //如果第1个数和第0个数比较，     第0个数 > 第1个数 即--->降序
            //传入序列：lo--runHi降序的“前边序列”
            //处理完后：lo--runHi升序的“前边序列”

            while (runHi < hi && ((Comparable) a[runHi]).compareTo(a[runHi - 1]) < 0)
                runHi++;
            //传入序列：lo--runHi降序的“前边序列”
            //处理完后：lo--runHi升序的“前边序列”
            reverseRange(a, lo, runHi); 
        } else {                              // Ascending
            // lo--runHi升序的“前边序列”
            while (runHi < hi && ((Comparable) a[runHi]).compareTo(a[runHi - 1]) >= 0)
                runHi++;
        }
        //省略代码//
    }
//以下为比较函数
	public int compareTo(Integer anotherInteger) {
        return compare(this.value, anotherInteger.value);
    }
    public static int compare(int x, int y) {
        //x = 比较数a[lo]    y被比较数a[runHi]
        return (x < y) ? -1 : ((x == y) ? 0 : 1);
    }
	
```

## **总结：**

countRunAndMakeAscending（）里使用compareTo（）方法----->compare(比较数--下标大，被比较数--下标小)

### 升序操作

```java
public static int compare(int x, int y) {
    //x = 比较数a[runHi]    y被比较数a[lo]
    if(x < y) return - 1;      //后边 < 前边 为真 返回-1  使得执行翻转操作 升序操作
    else if(x == y) return 0
    else return 1;			   //后边 > 前边 为真 返回-1  使得执行翻转操作 升序操作
}
```

### 降序操作

```java
public static int compare(int x, int y) {
    //x = 比较数a[runHi]    y被比较数a[lo]      runHi > lo 
    if(x < y) return 1;      // 后边 < 前边 为真 返回1 使得不执行翻转操作 降序
    else if(x == y) return 0
    else return -1;
}
```



