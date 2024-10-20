---
title: 栈
date: 2024-10-20
updated: 2024-10-20
categories: 数据结构与算法
tags: 数据结构
---

## 一、栈的概念
    栈（Stack）是一种 后进先出（LIFO, Last In First Out） 的数据结构。它就像一堆盘子，你只能从顶部添加或移除元素。栈的两个主要操作是：

    压栈（push）：将一个元素放到栈的顶部。
    出栈（pop）：从栈的顶部移除并返回这个元素。
    栈通常还支持一个 peek 操作，用来查看栈顶的元素而不移除它。

    栈的典型应用场景包括：

    表达式求值中的运算符优先级处理。
    程序调用的函数调用栈。
    深度优先搜索（DFS）算法。
    一个简单的栈可以通过数组或链表来实现。
    在数组实现中，我们使用一个指针来跟踪栈顶的位置。在链表实现中，我们使用一个指向栈顶的指针。
## 二、栈的实现
    栈可以通过数组或链表来实现。

### 1. 数组实现
   ```C++
    #include <iostream>
    using namespace std;

    const int MAX_SIZE = 100;

    class Stack {
    private:
        int data[MAX_SIZE];
        int top;
    public:
        // 构造函数，初始化栈顶指针
        Stack() {
            top = -1;
        }
        // 检查栈是否为空
        bool isEmpty() {
            return top == -1;
        }

        // 检查栈是否已满
        bool isFull() {
            return top == MAX_SIZE - 1;
        }

        // 添加元素到栈顶
        void push(int value) {
            if (isFull()) {
                cout << "Stack is full. Cannot push element." << endl;
                return;
            }
            data[++top] = value;
        }

        // 移除并返回栈顶元素
        int pop() {
            if (isEmpty()) {
                cout << "Stack is empty. Cannot pop element." << endl;
                return -1;
            }
            return data[top--];
        }

        // 查看栈顶元素 
        int peek() {
            if (isEmpty()) {
                cout << "Stack is empty. Cannot peek element." << endl;
                return -1;
            }
            return data[top];
        }
    };
   ```


### 2. 链表实现

   ```c++
   class Node {
   public:
       int data;
       Node* next;
       Node(int value) : data(value), next(nullptr) {}
   };

   class Stack {
   private:
       Node* top;
   public:
       Stack() : top(nullptr) {}

       // 添加元素到栈顶
       void push(int value) {
           Node* newNode = new Node(value);
           newNode->next = top;
           top = newNode;
       }

       // 移除并返回栈顶元素
       int pop() {
           if (isEmpty()) {
               cout << "Stack is empty. Cannot pop element." << endl;
               return -1;
           }
           int value = top->data;
           Node* temp = top;
           top = top->next;
           delete temp;
           return value;
       }

       // 查看栈顶元素
       int peek() {
           if (isEmpty()) {
               cout << "Stack is empty. Cannot peek element." << endl;
               return -1;
           }
           return top->data;
       }

       // 判断栈是否为空
       bool isEmpty() {
           return top == nullptr;
       }
   };
   ```

- 实例化：

   ```c++
   int main() {
       Stack stack;
       stack.push(1);
       stack.push(2);
       stack.push(3);
       cout << stack.peek() << endl; // 输出 3
       cout << stack.pop() << endl; // 输出 3
       cout << stack.peek() << endl; // 输出 2
       return 0;
   }
   ```

## 例题
-  有效的括号
   - 思路：使用栈，遇到左括号就入栈，遇到右括号就出栈，最后判断栈是否为空
   - 代码：
     ```c++
     bool isValid(string s) {
         stack<char> st;
         for (char c : s) {
             if (c == '(' || c == '[' || c == '{') {
                 st.push(c);
             } else {
                 if (st.empty()) {
                     return false;
                 }
                 char top = st.top();
                 st.pop();
                 if ((c == ')' && top != '(') ||
                     (c == ']' && top != '[') ||
                     (c == '}' && top != '{')) {
                     return false;
                 }
             }
         }
         return st.empty();
     }
     ```
    -  删除字符串中的所有相邻重复项
         - 思路：使用栈，遇到相邻重复项就出栈，最后将栈中的元素拼接成字符串
         - 代码：
     ```c++
     string removeDuplicates(string s) {
         stack<char> st;
         for (char c : s) {
             if (st.empty() || c != st.top()) {
                 st.push(c);
             } else {
                 st.pop();
             }
         }
         string res;
         while (!st.empty()) {
             res = st.top() + res;
             st.pop();
         }
         return res;
     }
     ```


    ![alt text](6.jpeg)

- 串模式匹配BF算法
     - 思路：使用两个指针i和j，分别指向主串和模式串的当前位置，如果匹配成功，则i和j都向后移动一位，否则i回溯到上一次匹配成功的位置的下一位，j回溯到模式串的开头
     - 代码：
     ```c++
     int BF(string s, string p) {
         int i = 0, j = 0;
         while (i < s.size() && j < p.size()) {
             if (s[i] == p[j]) {
                 i++;
                 j++;
             } else {
                 i = i - j + 1;
                 j = 0;
             }
         }
         if (j == p.size()) {
             return i - j;
         } else {
             return -1;
         }
     }
     ```
     - 时间复杂度：O(mn)，其中m是主串的长度，n是模式串的长度
     - 空间复杂度：O(1)

1. 从 S 串第1个字符开始： i=1， j=1，比较两个字符是否相等，如果相等，则 i++， j++；如果不等则执行第2步；

    ![alt text](2.png)
2. 从 S 串第3个字符开始：即 i 退回到 i- j+2的位置，即 i=3， j=1，比较两个字符是否相等，如果相等，则 i++， j++；如果不等则执行第4步

    ![alt text](3.png)
3. 从 S 串第3个字符开始：即 i 退回到 i- j+2的位置，即 i=3， j=1，比较两个字符是否相等，如果相等，则 i++， j++；如果不等则执行第4步

    ![alt text](4.png)
4. 从 S 串第4个字符开始：即 i 退回到 i- j+2的位置，即 i=4， j=1，比较两个字符是否相等，如果相等，则 i++， j++；由于此时 T 串比较完了，执行第5步

    ![alt text](5.png)
5. 需要返回子串T在主串S中第一个字符出现的位置，j( i ) 移动了T.length次，即 i - T.length=10-6=4。因已匹配，不需要+1回溯到一开始位置的下个位置了。

