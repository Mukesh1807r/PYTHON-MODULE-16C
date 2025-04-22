# Inserting Nodes in a B Tree

## 📌 Aim
To implement a Python function `insert(self, k)` that inserts nodes into a B Tree, demonstrating the structure before and after insertion.

---

## 🛠 Procedure

1. Define a class `BTreeNode`:
   - Contains `keys` and `children`.
   - Can be marked as a `leaf`.

2. Define a class `BTree`:
   - Initializes with a minimum degree `t`.
   - `insert(self, k)`:
     - Checks if the root is full.
     - If full, splits it before inserting.
     - Otherwise, inserts into the non-full node.
   - `insert_non_full(self, x, k)`:
     - Inserts a key in a non-full node.
   - `split_child(self, x, i)`:
     - Splits a child node during insertion.
   - `print_tree(self, x, l=0)`:
     - Prints the structure of the tree by level.

---

## 💻 Program

```python
class BTreeNode:
  def __init__(self, leaf=False):
    self.leaf = leaf
    self.keys = []
    self.child = []

class BTree:
  def __init__(self, t):
    self.root = BTreeNode(True)
    self.t = t

  def insert(self, k):
    root = self.root
    if len(root.keys) == (2 * self.t) - 1:
      temp = BTreeNode()
      self.root = temp
      temp.child.insert(0, root)
      self.split_child(temp, 0)
      self.insert_non_full(temp, k)
    else:
      self.insert_non_full(root, k)

  def insert_non_full(self, x, k):
    i = len(x.keys) - 1
    if x.leaf:
      x.keys.append((None, None))
      while i >= 0 and k[0] < x.keys[i][0]:
        x.keys[i + 1] = x.keys[i]
        i -= 1
      x.keys[i + 1] = k
    else:
      while i >= 0 and k[0] < x.keys[i][0]:
        i -= 1
      i += 1
      if len(x.child[i].keys) == (2 * self.t) - 1:
        self.split_child(x, i)
        if k[0] > x.keys[i][0]:
          i += 1
      self.insert_non_full(x.child[i], k)

  def split_child(self, x, i):
    t = self.t
    y = x.child[i]
    z = BTreeNode(y.leaf)
    x.child.insert(i + 1, z)
    x.keys.insert(i, y.keys[t - 1])
    z.keys = y.keys[t: (2 * t) - 1]
    y.keys = y.keys[0: t - 1]
    if not y.leaf:
      z.child = y.child[t: 2 * t]
      y.child = y.child[0: t - 1]

  def print_tree(self, x, l=0):
    print("Level ", l, " ", len(x.keys), end=":")
    for i in x.keys:
      print(i, end=" ")
    print()
    l += 1
    if len(x.child) > 0:
      for i in x.child:
        self.print_tree(i, l)

def main():
  B = BTree(3)
  for i in range(10):
    B.insert((i, 2 * i))
  print("B Tree :")
  B.print_tree(B.root)
  B.insert((11,))
  print("\nB Tree after insertion")
  B.print_tree(B.root)

if __name__ == '__main__':
  main()
```
---
## Output 

![image](https://github.com/user-attachments/assets/81412970-8d67-4ee0-8b59-e26ca0da9391)

---
## Result
Successfully implemented B Tree insertion and displayed its structure before and after node insertion.

