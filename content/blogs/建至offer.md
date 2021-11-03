Title: leetCode题目
Date: 2021-09-02 11:58
Category: 算法题目
Tags: java, python, leetCode
Authors: Fanson
Summary: 对java基础知识的一些总结

---------------------------------

### 用两个栈实现队列

```java
class CQueue {
    private Stack<Integer> stack1;
    private Stack<Integer> stack2;

    public CQueue() {
        stack1 = new Stack<>();
        stack2 = new Stack<>();
    }

    public void appendTail(int value) {
        stack1.append(value);
    }

    public int deleteHead() {
        if (!stack2.isEmpty) {
            return stack2.pop();
        }
        if (stack1.isEmpty) {
            return -1;
        }
        while (!stack1.isEmpty) {
            stack2.push(stack1.pop());
        }
        return stack2.pop();
    }
}
```

```python
class CQueue:
    def __init__(self):
        self.stack1 = []
        self.stack2 = []
    
    def appendTail(self, value: int) -> None:
        self.stack1.append(value)
    

    def deleteHead(self) -> int:
        if self.stack2:
            return self.stack2.pop()
        if not self.stack1:
            return -1
        while self.stack1:
            self.stack2.append(self.stack1.pop())
        return self.stack2.pop()
```
