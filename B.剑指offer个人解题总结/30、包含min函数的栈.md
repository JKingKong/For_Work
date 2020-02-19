## 1、

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

