# Leetcode-Python10

## 20. Valid Parentheses, 1047. Remove All Adjacent Duplicates In String, 150. Evaluate Reverse Polish Notation

May 24, 2023  4h

The tenth day for leetcode python study. Today we will learn more the classic use of **stack**.\
The challenges today are about ~~need to be finished later.~~

## 20. Valid Parentheses
[leetcode](https://leetcode.com/problems/valid-parentheses/)\
[Reading link](https://github.com/youngyangyang04/leetcode-master/blob/master/problems/0020.%E6%9C%89%E6%95%88%E7%9A%84%E6%8B%AC%E5%8F%B7.md)\
[video](https://www.bilibili.com/video/BV1AF411w78g/?spm_id_from=pageDriver&vd_source=63f26efad0d35bcbb0de794512ac21f3)\
Here we use stack to solve this problem. \
It would be easier to think about the 3 different types of disagreement first.
```python
# ways 1: use stack
class Solution:
    def isValid(self, s: str) -> bool:
        # create a new empty stack to store the pairing items: )]}
        stack = []
        for item in s:
            if item == '(':
                stack.append(')')
            elif item == '[':
                stack.append(']')
            elif item == '{':
                stack.append('}')
            # 第三种情况：遍历字符串匹配的过程中，栈已经为空了，没有匹配的字符了，说明右括号没有找到对应的左括号 return false
            # 第二种情况：遍历字符串匹配的过程中，发现栈里没有我们要匹配的字符。所以return false
            elif not stack or stack[-1]!=item:
                return False
            else:
                # 此时是属于要求的pairing，弹出右部元素，进入下一个item循环
                stack.pop()

        # 第一种情况：此时我们已经遍历完了字符串，但是栈不为空，说明有相应的左括号没有右括号来匹配，所以return false，否则就return true
        return True if not stack else False
```
```python
# ways 2: use stack and dictionary
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        mapping = {
            '(': ')',
            '[': ']',
            '{': '}'
        }
        for item in s:
            if item in mapping.keys():
                stack.append(mapping[item])
            elif not stack or stack[-1] != item: 
                return False
            else: 
                stack.pop()
        return True if not stack else False
```

## 1047. Remove All Adjacent Duplicates In String
[leetcode](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/)\
The reason why stack is commonly used in the match and removal questions is that it can help to record what the former item of the current one we are pointing to is.\
Here we use a string to mock the stack, so in the end we don't need to change the stack into string for return.\
We can also use **two pointers** to solve this question.
```python
# ways 1: use list to mock the operation of a stack:
class Solution:
    def removeDuplicates(self, s: str) -> str:
        l=[]
        for item in s:
            #if l is empty, or the new item not equal to the top item of stack/string,put the new item in the new list;
            #Otherwise it means the new item equals to the last item, which was just put into the stack, and both items need to be removed/pop.
            if not l or l[-1]!=item:
                l.append(item)
            else:
                l.pop()
        return "".join(l)
```
```python
# ways 2: use two pointers:
class Solution:
    def removeDuplicates(self, s: str) -> str:
        res = list(s)
        slow = fast = 0
        length = len(res)

        while fast < length:
            # 如果一样直接换，不一样会把后面的填在slow的位置
            res[slow] = res[fast]
            
            # 如果发现和前一个一样，就退一格指针
            if slow > 0 and res[slow] == res[slow - 1]:
                slow -= 1
            else:
                slow += 1
            fast += 1
            
        return ''.join(res[0: slow])
```

## 150. Evaluate Reverse Polish Notation
[leetcode](https://leetcode.com/problems/evaluate-reverse-polish-notation/)\
[reading](https://github.com/youngyangyang04/leetcode-master/blob/master/problems/0150.%E9%80%86%E6%B3%A2%E5%85%B0%E8%A1%A8%E8%BE%BE%E5%BC%8F%E6%B1%82%E5%80%BC.md)\
[video](https://www.bilibili.com/video/BV1kd4y1o7on/?spm_id_from=333.788&vd_source=63f26efad0d35bcbb0de794512ac21f3)\
Ha? Polish notation is an easy expression that is easy for computer to process, and doesn't need brackets.\
We use stack to solve this problem. When the item is an number, we put it into the stack; when loop to a notation, we take/pop two numbers out of the stack and do the operation, and then put the result back into the stack, and loop to the next item.\
This is similar to the last two questions.
```python
# ways 1: use stack and eval() function for operations:
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        stack = []
        for item in tokens:
            if item not in {'+','-','*','/'}:
                stack.append(item)
            else:
                first_num, second_num = stack.pop(), stack.pop()
                stack.append(int(eval(f'{second_num}{item}{first_num}')))
        return int(stack.pop()) # 如果一开始只有一个数，那么会是字符串形式的
```
```python
# ways 2: use stack and dictionary:
from operator import add, sub, mul
class Solution:
    op_map = {'+': add, '-': sub, '*': mul, '/': lambda x, y: int(x / y)}
    
    def evalRPN(self, tokens: List[str]) -> int:
        stack = []
        for token in tokens:
            if token not in {'+', '-', '*', '/'}:
                stack.append(int(token))
            else:
                op2 = stack.pop()
                op1 = stack.pop()
                stack.append(self.op_map[token](op1, op2))  # 第一个出来的在运算符后面
        return stack.pop()
```



