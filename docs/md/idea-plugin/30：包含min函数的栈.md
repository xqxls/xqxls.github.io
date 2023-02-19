## 题目描述
> 定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O(1)。
> 示例:
> MinStack minStack = new MinStack(); 
> minStack.push(-2);
> minStack.push(0); 
> minStack.push(-3); 
> minStack.min(); --> 返回 -3.
> minStack.pop(); 
> minStack.top();  --> 返回 0. 
> minStack.min(); --> 返回 -2.  
> 提示：各函数的调用总次数不超过 20000 次

## 方法一（辅助栈）
#### 1.解题思路
用一个data栈正常入栈和出栈，用一个min栈来记录每次的最小值。当入栈元素小于等于min栈顶元素或min栈为空时，才push到min栈。当出栈元素等于min栈时，min栈也要出栈。
比如，将3 2 4 5 1这几个元素依次入栈的过程如下：

```bash
3入栈，data=[3]                min=[3]
2入栈，data=[3,2]              min=[3,2]
4入栈，data=[3,2,4]            min=[3,2]
5入栈，data=[3,2,4,5]          min=[3,2]
1入栈，data=[3,2,4,5,1]        min=[3,2,1]
```

当出栈元素为min的栈顶时，min才出栈。
#### 2.代码实现

```java
class MinStack {
    Deque<Integer> data;
    Deque<Integer> min;
    
    public MinStack() {
        data=new LinkedList<>();
        min=new LinkedList<>();
    }
    
    public void push(int x) {
        data.push(x);
        if(min.isEmpty()||x<=min.peek()){
            min.push(x);
        }
    }
    
    public void pop() {
        if(data.pop().equals(min.peek())){
            min.pop();
        }
    }
    
    public int top() {
        return data.peek();
    }
    
    public int min() {
        return min.peek();
    }
}
```
#### 3.复杂度分析
 - 时间复杂度：push(), pop(), top(), min() 四个函数的时间复杂度均为常数级别，所以时间复杂度为O(1)
 - 空间复杂度：最坏情况下，辅助栈min要存储所有元素，所以空间复杂度为O(n)
## 方法二（单栈）
#### 1.解题思路
用一个全局变量记录最小值，当入栈元素小于等于min时，不仅要将元素入栈，还要把上一个min入栈，并且跟新min；否则直接将元素入栈。
出栈的时候，如果出栈元素等于min，则回退到上一个min。
比如，将3 2 4 5 1这几个元素依次入栈的过程如下：

```bash
3入栈，data=[Integer.MAX_VALUE,3]                  min=3
2入栈，data=[Integer.MAX_VALUE,3,3,2]              min=2
4入栈，data=[Integer.MAX_VALUE,3,3,2,4]            min=2
5入栈，data=[Integer.MAX_VALUE,3,3,2,4,5]          min=2
1入栈，data=[Integer.MAX_VALUE,3,3,2,4,5,2,1]      min=1
```
#### 2.代码实现

```java
class MinStack {
    int min = Integer.MAX_VALUE;
    Stack<Integer> stack = new Stack<>();

    public void push(int x) {
        //如果加入的值小于等于最小值，要更新最小值
        if (x <= min) {
            stack.push(min);
            min = x;
        }
        stack.push(x);
    }

    public void pop() {
        //如果把最小值出栈了，就回退到上一个最小值
        if (stack.pop() == min)
            min = stack.pop();
    }

    public int top() {
        return stack.peek();
    }

    public int min() {
        return min;
    }
}
```
#### 3.复杂度分析
 - 时间复杂度：push(), pop(), top(), min() 四个函数的时间复杂度均为常数级别，所以时间复杂度为O(1)
 - 空间复杂度：不需要额外的内存空间，所以空间复杂度为O(1)

## 方法三（构造链表实现）
#### 1.解题思路
在链表的结构上，新增一个min变量，用于存储最小值。
当链表为空的时候，新增一个head头节点，并初始化val，min和next，当链表不为空的时候，用头插法在head前面插入一个节点即可。弹出元素的时候，只需将head指针后移。
#### 2.代码实现

```java
class MinStack {
    private class Node {
       int val;
       int min;
       Node next;
       public Node(int val, int min, Node next) {
           this.val = val;
           this.min = min;
           this.next = next;
       }
    }

    private Node head;
    
    public MinStack() {

    }

    public void push(int x) {
        if (head == null)
            head = new Node(x, x, null);
        else
            head = new Node(x, Math.min(head.min, x), head);
    }
    
    public void pop() {
        head = head.next;
    }

    public int top() {
        return head.val;
    }

    public int min() {
        return head.min;
    }

}
```
#### 3.复杂度分析
 - 时间复杂度：push(), pop(), top(), min() 四个函数的时间复杂度均为常数级别，所以时间复杂度为O(1)
 - 空间复杂度：不需要额外的内存空间，所以空间复杂度为O(1)
