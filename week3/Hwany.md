# 104. Maximum Depth of Binary Tree

## 문제 설명
이진 트리의 루트 노드가 주어졌을때 트리의 최대 깊이를 구하는 문제 
---

## solution

```java

class Solution {
    public int maxDepth(TreeNode root) {
        if(root == null) return 0;

        int leftDepth = maxDepth(root.left);
        int rightDept = maxDepth(root.right);

        int max = leftDepth < rightDepth ? rightDepth : leftDepth;

        return max + 1; // 1은 자기 노드
    }
}

```

## 접근 아이디어 
왼쪽 서브트리의 깊이를 알기 위해 왼쪽 하단의 노드까지 내려간다. 각 노드에서 더 이상 자식이 없으면 깊이 갚을 부모에게 반환하고 부모노드는 왼쪽 자식으로 부터 받은 깊이를 변수에 저장 (오른쪽도 동일한) 재귀를 사용