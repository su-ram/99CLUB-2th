### 문제
[문제링크](https://leetcode.com/problems/reverse-odd-levels-of-binary-tree/description/)
- python
- 06/03

### 풀이
- 핵심개념 : `그래프 탐색`
- 홀수 레벨에 있는 값들을 reverse 시켜서 값을 재정렬한다.
- 특정 레벨에 있는 노드들의 값을 바꾸는 문제이므로 전체 탐색이 필요하다. 

### 셀프 피드백
- BFS로 풀긴 풀었는데 메모리 사용량이 너무 많이 나와 효율적이지 못한 코드였다.
- 총 2번의 탐색으로 풀었는데 1번의 재귀 탐색만으로 가능한 문제.
- 다른 사람의 DFS 코드를 봤는데
  - 왜 left, right를 바꾸는지,
  - 왜 left <-> right 교차시켜서 다시 재귀적으로 호출하는지 직관적으로 이해가 가지 않았다. 


### 코드
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
from collections import deque

class Solution:
    def reverseOddLevels(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        queue = deque()
        visited = []
        lvl = 0
        odds = dict()
        queue.append((root, lvl))
        # 첫번째 탐색
        while queue:
            node, lvl = queue.popleft()
            visited.append(node)
            if lvl % 2 == 1 : 
                if odds.get(lvl) == None :
                    odds[lvl] = deque([node.val])
                else:
                    odds[lvl].append(node.val)
            if node.left != None and node.left not in visited:
                queue.append((node.left, lvl + 1))
            if node.right != None and node.right not in visited:
                queue.append((node.right, lvl + 1))
        
        visited = []
        queue.clear()
        lvl = 0 
        queue.append((root, lvl))
        # 두번째 탐색
        while queue:
            node, lvl = queue.pop()
            if lvl % 2 == 1:
                node.val = odds.get(lvl).popleft()
            if node.left != None and node.left not in visited:
                queue.append((node.left, lvl + 1))
            if node.right != None and node.right not in visited:
                queue.append((node.right, lvl + 1))

        return root
```

```python
# DFS 사용. 다른 사람 풀이 코드

class Solution:
    def reverseOddLevels(self, root: Optional[TreeNode]) -> Optional[TreeNode]:        
        def reverse(root1, root2, level):
            if root1 is None and root2 is None:
                return None
            
            if level %2 != 0:
                root1.val, root2.val = root2.val, root1.val
            #moving towards the outer nodes of the next level <- 이부분이 이해가 안 간다.
            reverse(root1.left, root2.right, level+1)
            #moving towards the inner nodes of the next level. <- 역시 이부분이 이해가 안 간다. 
            reverse(root1.right, root2.left, level+1)
        
        reverse(root.left, root.right, 1)
        return root
```
