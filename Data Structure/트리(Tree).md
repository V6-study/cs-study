# 트리(Tree)    

## 1. 트리의 개념

트리는 노드(node)들이 계층적으로 연결된 비선형 자료구조

### 트리의 주요 용어

- **노드(Node)**: 트리를 구성하는 기본 요소
- **루트(Root)**: 트리의 최상위에 있는 노드
- **부모 노드(Parent Node)**: 특정 노드의 상위 노드
- **자식 노드(Child Node)**: 특정 노드의 하위 노드
- **리프 노드(Leaf Node)**: 자식이 없는 말단 노드
- **레벨(Level)**: 루트로부터의 깊이
- **높이(Height)**: 트리의 최대 레벨    

## 2. 트리의 특징

1. 순환(Cycle)이 없는 연결 구조 
    - 만약 사이클이 만들어진다면, 그것은 트리가 아니고 그래프다.
    - 사이클이 존재하지 않는 그래프라 해서 무조건 트리인 것은 아니다. 사이클이 존재하지 않고 모든 노드가 간선으로 이어져야 트리이다.
2. 모든 노드는 0개 이상의 자식 노드를 가질 수 있음
3. 하나의 부모 노드만을 가짐
4. 노드가 N개인 트리는 항상 N-1개의 간선(edge)을 가짐    

## 3. 트리 순회 방법

1. **전위 순회(Preorder)**: 루트 → 왼쪽 자식 → 오른쪽 자식
2. **중위 순회(Inorder)**: 왼쪽 자식 → 루트 → 오른쪽 자식
3. **후위 순회(Postorder)**: 왼쪽 자식 → 오른쪽 자식 → 루트
4. **레벨 순회(Level-order)**: 레벨 단위로 왼쪽에서 오른쪽으로 순회    

## 4. 트리의 종류

### 4.1 이진 트리(Binary Tree)

- 각 노드가 최대 2개의 자식을 가지는 트리
- 종류:
    - 완전 이진 트리(Complete Binary Tree)
    - 포화 이진 트리(Perfect Binary Tree)
    - 균형 이진 트리(Balanced Binary Tree)

### 4.2 이진 탐색 트리(Binary Search Tree)

- 왼쪽 자식 노드 < 부모 노드 < 오른쪽 자식 노드
- 중위 순회시 정렬된 결과
- 탐색, 삽입, 삭제 연산의 시간복잡도: O(log n) (균형잡힌 경우)

### 4.3 B-트리

- 하나의 노드가 가질 수 있는 자식 노드의 최대 숫자가 2보다 큰 트리 구조
- 디스크 접근을 최소화하기 위해 사용, 데이터베이스와 파일 시스템에서 주로 사용
- 종류:
    - B-트리
    - B+트리
    - B*트리    

## 5. 트리의 구현 예시 (Java)

### 5.1 기본 노드 클래스

```java
public class Node<T> {
    private T data;
    private Node<T> left;
    private Node<T> right;

    public Node(T data) {
        this.data = data;
        this.left = null;
        this.right = null;
    }
    
    // Getter와 Setter
    public T getData() { return data; }
    public void setData(T data) { this.data = data; }
    public Node<T> getLeft() { return left; }
    public void setLeft(Node<T> left) { this.left = left; }
    public Node<T> getRight() { return right; }
    public void setRight(Node<T> right) { this.right = right; }
}
```

### 5.2 이진 트리 구현

```java
public class BinaryTree<T> {
    private Node<T> root;

    public BinaryTree() {
        this.root = null;
    }

    // 노드 삽입 (레벨 순서 삽입)
    public void insert(T data) {
        if (root == null) {
            root = new Node<>(data);
            return;
        }

        Queue<Node<T>> queue = new LinkedList<>();
        queue.add(root);

        while (!queue.isEmpty()) {
            Node<T> current = queue.poll();

            if (current.getLeft() == null) {
                current.setLeft(new Node<>(data));
                return;
            }
            if (current.getRight() == null) {
                current.setRight(new Node<>(data));
                return;
            }

            queue.add(current.getLeft());
            queue.add(current.getRight());
        }
    }

    // 전위 순회 (Preorder)
    public void preorder(Node<T> node) {
        if (node == null) return;
        
        System.out.print(node.getData() + " ");
        preorder(node.getLeft());
        preorder(node.getRight());
    }

    // 중위 순회 (Inorder)
    public void inorde(Node<T> node) {
        if (node == null) return;
        
        inorderTraversal(node.getLeft());
        System.out.print(node.getData() + " ");
        inorder(node.getRight());
    }

    // 후위 순회 (Postorder)
    public void postorder(Node<T> node) {
        if (node == null) return;
        
        postorde(node.getLeft());
        postorderTraversal(node.getRight());
        System.out.print(node.getData() + " ");
    }

    // 레벨 순회 (Level-order)
    public void levelOrder() {
        if (root == null) return;

        Queue<Node<T>> queue = new LinkedList<>();
        queue.add(root);

        while (!queue.isEmpty()) {
            Node<T> current = queue.poll();
            System.out.print(current.getData() + " ");

            if (current.getLeft() != null)
                queue.add(current.getLeft());
            if (current.getRight() != null)
                queue.add(current.getRight());
        }
    }

    // 트리의 높이 계산
    public int getHeight(Node<T> node) {
        if (node == null) return 0;
        
        int leftHeight = getHeight(node.getLeft());
        int rightHeight = getHeight(node.getRight());
        
        return Math.max(leftHeight, rightHeight) + 1;
    }

    public Node<T> getRoot() {
        return root;
    }
}
```

---

## 6. 시간 복잡도 분석

- 탐색: O(log n) ~ O(n)
- 삽입: O(log n) ~ O(n)
- 삭제: O(log n) ~ O(n)
- 트리의 균형 상태에 따라 달라짐

## 7. 트리의 활용

1. 계층적 데이터 표현
    - 파일 시스템
    - 조직도
    - HTML/XML 문서
2. 검색 기능 구현
    - 이진 탐색 트리
    - B-트리, B+트리
3. 힙(Heap) 구현
    - 우선순위 큐
    - 힙 정렬
