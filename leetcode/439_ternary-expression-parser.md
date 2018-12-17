# 439. Ternary Expression Parser


Given a string representing arbitrarily nested ternary expressions, calculate the result of the expression. You can always assume that the given expression is valid and only consists of digits 0-9, ?, :, T and F (T and F represent True and False respectively).

Note:

The length of the given string is ≤ 10000.
Each number will contain only one digit.
The conditional expressions group right-to-left (as usual in most languages).
The condition will always be either T or F. That is, the condition will never be a digit.
The result of the expression will always evaluate to either a digit 0-9, T or F.

Example 1:

```
Input: "T?2:3"

Output: "2"

Explanation: If true, then result is 2; otherwise result is 3.
```

Example 2:

```
Input: "F?1:T?4:5"

Output: "4"

Explanation: The conditional expressions group right-to-left. Using parenthesis, it is read/evaluated as:

             "(F ? 1 : (T ? 4 : 5))"                   "(F ? 1 : (T ? 4 : 5))"
          -> "(F ? 1 : 4)"                 or       -> "(T ? 4 : 5)"
          -> "4"                                    -> "4"
```
  
Example 3:

```
Input: "T?T?F:5:3"

Output: "F"

Explanation: The conditional expressions group right-to-left. Using parenthesis, it is read/evaluated as:

             "(T ? (T ? F : 5) : 3)"                   "(T ? (T ? F : 5) : 3)"
          -> "(T ? F : 3)"                 or       -> "(T ? F : 5)"
          -> "F"                                    -> "F"

```  

## Idea

1. 把Tenary expression变成binary Tree, 然后“T”往做，“F”往右。
2. 问题就变成：Tenary expression to binary Tree了。 

## Code 
  

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.left = None 
        self.right = None 

class Solution(object):
    def parseTernary(self, expression):
        """
        :type expression: str
        :rtype: str
        """
        root = self.ternary2tree(expression)
        while root.left:
            if root.data == "T":
                root = root.left 
            else:
                root = root.right         
        return root.data 
        
    def ternary2tree(self, expression):
        root = Node(expression[0])
        stack = []
        stack.append(root)
        for i in range(1, len(expression),2):
            node = Node(expression[i+1])
            if expression[i] == "?":
                stack[-1].left = node
            else: 
                stack.pop()
                while stack[-1].right != None:
                    stack.pop()
                stack[-1].right = node
            stack.append(node)
        return root
```  