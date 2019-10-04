# Binary Tree (이진트리)
---
> 배열, Linked List, Stack, Queue는 선형 자료구조이다. 이와 다르게 Tree는 계층형 자료구조다.

### 특징
> - 트리 자료구조는 `Search/Access`에 대해서 Linked List 보다 빠르고 Array보다 느린 중간 정도 
  > > (탐색/참조) Linked List < Tree < Array
> - `Insertion / Deletion`에 대해서는 Array 보다 빠르고 정렬안된 Linked List 보다 느리다
 > > (삽입/삭제) Array < Tree < Unordered Linked List
  
  
### Tree가 사용되는 경우
- 계층구조의 데이터를 다룰때
- 쉽게 data를 찾아야 할때
- 정렬된 데이터를 조작할 떄
- 라우터 알고리즘
- 다단계 의사 결정 형태

### Node 구현
- C
~~~C
struct node
{
  int data;
  struct node *left;
  struct node *right;
};

//새로들어온 노드에 대해서 동적할당하고 left,right chlid를 NULL로 두기
struct node* newNode(int data)
{
  //동적할당
  struct node* node = (struct node*)malloc(sizeof(struct node));
  
  node->data = data;
  node->left = NULL;
  node->right = NULL;
  return(node);
}

int main()
{
  //create root
  struct node *root = newNode(1);
  
  root->left = newNode(2);
  root->right = newNode(3);
  
  root->left->left = newNode(4);
  
  /* 4 becomes left child of 2 
           1 
       /       \ 
      2          3 
    /   \       /  \ 
   4    NULL  NULL  NULL 
  /  \ 
NULL NULL 
*/

  getchar();
  return 0;
}
~~~


- Python
~~~Python
  class Node:
    def __init__(self,key):
      self.left = None
      self.right = None
      self.val = key
      
  # create root
  root = Node(1)
  root.left = Node(2)
  root.right = Node(3)
  root.left.left = Node(4)
~~~

## 특징2
  
  - Height : root 노드에서 leaf 노드까지의 최장거리임, O(logn)
  - Depth : 특정노드에서 root 노드까지 edges 갯수
  - Level = Depth + 1
  
> - (Level) n일때 그 레벨에서 Maximum인 노드의 갯수는 `2^(n-1)`
  > - 그 레벨에서 (가로로) 최대일 수 있는 노드의 수
> - 높이(height)가 h 일때 전체 트리의 Maximum 노드의 갯수는 `(2^h) - 1`
> - 이진트리에서 최소로, 미니멈으로 혀용가능한 height or levels은? ---> `|log2(N+1)| - 1`
> - 이진트리에서 최소인 Depth를 찾는법
~~~C
  int minDepth(Node *root)
  {
    if(root == NULL)
      return 0;
    if(root->left == NULL && root->right == NULL)
      return 1;
    if(!root->left)
      return minDepth(root->right) + 1;
    if(!root->right)
      return minDepth(root->left) + 1;
    
    return min(minDepth(root->left), minDepth(root->right)) + 1;
  }
~~~

> 모든 노드를 보고 +1을 하는 방법보다 BFS처럼 level별로 보다가 leaf 노드를 만나면 그만두는 방법이 더 좋다.

~~~C++
  while(q.empty() == false)
  {
    qi = q.front();
    q.pop();
    
    NOde *node = qi.node;
    int depth = qi.depth;
    
    if(node->left == NULL && node->right == NULL)
      return depth;
    
    // left subtree가 NULL이 아님 큐에 넣음
    if(node->left != NULL)
    {
      qi.node = node->left;
      qi.depth = depth + 1;
      q.push(qi);
    }
    
    if(node->right != NULL)
    {
      qi.node = node->right;
      qi.depth = depth + 1;
      q.push(qi);
    }
  }
~~~

> - L 개의 leaf node를 가지는 이진트리에서 최소인 level은 `Log2(L) + 1`


## Reference
https://www.quora.com/What-is-the-difference-between-the-height-and-level-of-a-full-binary-tree
http://typeocaml.com/2014/11/26/height-depth-and-level-of-a-tree/
https://www.geeksforgeeks.org/binary-tree-set-2-properties/
