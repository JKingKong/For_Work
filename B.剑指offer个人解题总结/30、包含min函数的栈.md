## 1、暴力法

### 1.分析	



### 2.代码

```java
import java.util.Stack;
import java.util.Iterator;
public class Solution {
        private Stack<Integer> stack = new Stack<>();
        private int min = Integer.MAX_VALUE; //min初始化成int的最大值
        public void push(int node) {
            stack.push(node);
            if(node < min) //入栈时，发现node比min还小，重新设置最小值
                min = node;
        }
        public void pop() {
            if(stack.peek() == min){
                // 当最小值被弹出时，要寻找新的最小值
                stack.pop();
                min = Integer.MAX_VALUE;
                // 寻找新的最小值
                Iterator itr = stack.iterator();
                while(itr.hasNext()){
                    int tmp = (int)itr.next();
                    if(tmp < min)
                        min = tmp;
                }
            }else
                stack.pop();
        }

        public int top() {
            return stack.peek();
        }
        public int min() {
            return min;
        }
}
```

### 3.复杂度

* 时间复杂度：O()

* 空间复杂度：O()

## 2、双栈法

```java
    //正常stack
    Stack<Integer> stack = new Stack<>();
    //存放min的stack
    Stack<Integer> help = new Stack<>();

    public void push(int node) {
        stack.push(node);
        if (help.isEmpty()) {
            help.push(node);
        } else {
            //help里面存放的都是最小值,也就是pop之后里面是当前的最小值
            int res = help.peek() > node ? node : help.peek();
            help.push(res);
        }
    }

    public void pop() {
        stack.pop();
        help.pop();
    }

    public int top() {
        return stack.peek();
    }

    public int min() {
        return help.peek();
    }
```

