---
Creation Time: Thursday, September 12th 2024
Modified Time: Thursday, September 12th 2024
---


Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the `MinStack` class:

- `MinStack()` initializes the stack object.
- `void push(int val)` pushes the element `val` onto the stack.
- `void pop()` removes the element on the top of the stack.
- `int top()` gets the top element of the stack.
- `int getMin()` retrieves the minimum element in the stack.

You must implement a solution with `O(1)` time complexity for each function.

```java

class MinStack {

Stack<StackNode> st;

class StackNode{

int current;

int currentMin;

}

  

public MinStack() {

st = new Stack();

}

public void push(int val) {

StackNode node1 = new StackNode();

node1.current = val;

if(!st.isEmpty()){

if(st.peek().currentMin > val){

node1.currentMin = val;

}else{

node1.currentMin = st.peek().currentMin;

}

}else{

node1.currentMin = val;

}

  

st.push(node1);

}

public void pop() {

st.pop();

}

public int top() {

return st.peek().current;

}

public int getMin() {

return st.peek().currentMin;

}

}
```