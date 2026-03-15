# OOPS and DSA in Python

> A comprehensive reference covering **Object-Oriented Programming**, **Data Structures**, and **Algorithms** in Python — from foundational concepts to modern data management and governance.
> A companion LaTeX document (`README.tex`) contains TikZ diagrams and full academic references.

---

## Table of Contents

1. [Object-Oriented Programming (OOP)](#1-object-oriented-programming-oop)
   - [Classes and Objects](#11-classes-and-objects)
   - [Encapsulation](#12-encapsulation)
   - [Inheritance](#13-inheritance)
   - [Polymorphism](#14-polymorphism)
   - [Abstraction](#15-abstraction)
   - [Magic / Dunder Methods](#16-magic--dunder-methods)
   - [Class Methods, Static Methods, and Properties](#17-class-methods-static-methods-and-properties)
   - [Dataclasses](#18-dataclasses)
   - [Design Patterns](#19-design-patterns)
2. [Basic Data Structures](#2-basic-data-structures)
   - [Arrays and Lists](#21-arrays-and-lists)
   - [Linked Lists](#22-linked-lists)
   - [Stacks](#23-stacks)
   - [Queues and Deques](#24-queues-and-deques)
   - [Hash Tables / Dictionaries](#25-hash-tables--dictionaries)
   - [Sets](#26-sets)
3. [Trees](#3-trees)
   - [Binary Trees](#31-binary-trees)
   - [Binary Search Trees (BST)](#32-binary-search-trees-bst)
   - [AVL Trees](#33-avl-trees)
   - [Red-Black Trees](#34-red-black-trees)
   - [Heaps and Priority Queues](#35-heaps-and-priority-queues)
   - [Tries (Prefix Trees)](#36-tries-prefix-trees)
   - [Segment Trees](#37-segment-trees)
   - [Fenwick Trees (Binary Indexed Trees)](#38-fenwick-trees-binary-indexed-trees)
   - [B-Trees and B+ Trees](#39-b-trees-and-b-trees)
4. [Graphs](#4-graphs)
   - [Graph Representations](#41-graph-representations)
   - [BFS and DFS](#42-bfs-and-dfs)
   - [Shortest Path Algorithms](#43-shortest-path-algorithms)
   - [Minimum Spanning Trees](#44-minimum-spanning-trees)
   - [Topological Sort](#45-topological-sort)
   - [Strongly Connected Components](#46-strongly-connected-components)
5. [Advanced and Modern Data Structures](#5-advanced-and-modern-data-structures)
   - [Bloom Filters](#51-bloom-filters)
   - [Skip Lists](#52-skip-lists)
   - [Disjoint Set Union (Union-Find)](#53-disjoint-set-union-union-find)
   - [LRU Cache](#54-lru-cache)
   - [Suffix Arrays and Suffix Trees](#55-suffix-arrays-and-suffix-trees)
   - [Persistent Data Structures](#56-persistent-data-structures)
   - [Rope (String Data Structure)](#57-rope-string-data-structure)
   - [Van Emde Boas Trees](#58-van-emde-boas-trees)
6. [Algorithms](#6-algorithms)
   - [Sorting Algorithms](#61-sorting-algorithms)
   - [Searching Algorithms](#62-searching-algorithms)
   - [Divide and Conquer](#63-divide-and-conquer)
   - [Dynamic Programming](#64-dynamic-programming)
   - [Greedy Algorithms](#65-greedy-algorithms)
   - [Backtracking](#66-backtracking)
   - [Bit Manipulation](#67-bit-manipulation)
   - [String Algorithms](#68-string-algorithms)
7. [Complexity Analysis](#7-complexity-analysis)
8. [Data Management and Governance](#8-data-management-and-governance)
   - [Data Pipelines in Python](#81-data-pipelines-in-python)
   - [Data Quality and Validation](#82-data-quality-and-validation)
   - [Metadata Management](#83-metadata-management)
   - [Data Versioning](#84-data-versioning)
   - [Distributed Data Structures](#85-distributed-data-structures)
9. [References](#9-references)

---

## 1. Object-Oriented Programming (OOP)

### 1.1 Classes and Objects

A **class** is a blueprint for creating objects. An **object** is an instance of a class.

```python
class Animal:
    species_count = 0          # class variable (shared by all instances)

    def __init__(self, name: str, sound: str) -> None:
        self.name = name       # instance variable
        self.sound = sound
        Animal.species_count += 1

    def speak(self) -> str:
        return f"{self.name} says {self.sound}"

dog = Animal("Dog", "Woof")
cat = Animal("Cat", "Meow")
print(dog.speak())             # Dog says Woof
print(Animal.species_count)   # 2
```

### 1.2 Encapsulation

Encapsulation bundles data and methods together while restricting direct access to internal state via access modifiers.

| Prefix | Convention | Access |
|--------|-----------|--------|
| `name` | public | anywhere |
| `_name` | protected | class and subclasses |
| `__name` | private (name-mangled) | class only |

```python
class BankAccount:
    def __init__(self, owner: str, balance: float = 0.0) -> None:
        self.owner = owner
        self.__balance = balance   # private

    def deposit(self, amount: float) -> None:
        if amount > 0:
            self.__balance += amount

    def withdraw(self, amount: float) -> bool:
        if 0 < amount <= self.__balance:
            self.__balance -= amount
            return True
        return False

    @property
    def balance(self) -> float:
        return self.__balance

account = BankAccount("Alice", 1000)
account.deposit(500)
print(account.balance)  # 1500
```

### 1.3 Inheritance

Inheritance allows a class (child) to derive attributes and methods from another class (parent), promoting code reuse.

```python
import math

class Shape:
    def area(self) -> float:
        raise NotImplementedError

    def perimeter(self) -> float:
        raise NotImplementedError

class Circle(Shape):
    def __init__(self, radius: float) -> None:
        self.radius = radius

    def area(self) -> float:
        return math.pi * self.radius ** 2

    def perimeter(self) -> float:
        return 2 * math.pi * self.radius

class Rectangle(Shape):
    def __init__(self, width: float, height: float) -> None:
        self.width = width
        self.height = height

    def area(self) -> float:
        return self.width * self.height

    def perimeter(self) -> float:
        return 2 * (self.width + self.height)
```

**Multiple Inheritance and MRO (C3 Linearisation)**

```python
class A:
    def greet(self): return "A"

class B(A):
    def greet(self): return "B"

class C(A):
    def greet(self): return "C"

class D(B, C):   # MRO: D -> B -> C -> A
    pass

print(D().greet())   # B
print(D.__mro__)
```

### 1.4 Polymorphism

Polymorphism allows objects of different classes to be treated through a shared interface.

```python
from typing import List

shapes: List[Shape] = [Circle(5), Rectangle(4, 6)]
for shape in shapes:
    print(f"Area: {shape.area():.2f}")   # dispatches to the correct method
```

**Duck Typing** — Python's idiomatic polymorphism:

```python
class Duck:
    def quack(self): return "Quack!"

class Person:
    def quack(self): return "I'm quacking like a duck!"

def make_it_quack(obj):
    print(obj.quack())   # works for any object with .quack()

make_it_quack(Duck())
make_it_quack(Person())
```

### 1.5 Abstraction

Abstract classes define interfaces without providing full implementations. Use the `abc` module.

```python
from abc import ABC, abstractmethod

class DataStore(ABC):
    @abstractmethod
    def read(self, key: str) -> bytes: ...

    @abstractmethod
    def write(self, key: str, value: bytes) -> None: ...

    def exists(self, key: str) -> bool:
        try:
            self.read(key)
            return True
        except KeyError:
            return False

class InMemoryStore(DataStore):
    def __init__(self):
        self._store = {}

    def read(self, key):
        return self._store[key]

    def write(self, key, value):
        self._store[key] = value
```

### 1.6 Magic / Dunder Methods

Dunder (double underscore) methods let you define custom behavior for built-in operations.

```python
class Vector:
    def __init__(self, x: float, y: float):
        self.x, self.y = x, y

    def __repr__(self) -> str:
        return f"Vector({self.x}, {self.y})"

    def __add__(self, other: "Vector") -> "Vector":
        return Vector(self.x + other.x, self.y + other.y)

    def __mul__(self, scalar: float) -> "Vector":
        return Vector(self.x * scalar, self.y * scalar)

    def __len__(self) -> int:
        return 2

    def __eq__(self, other: object) -> bool:
        if not isinstance(other, Vector):
            return NotImplemented
        return self.x == other.x and self.y == other.y

    def __hash__(self) -> int:
        return hash((self.x, self.y))

v1 = Vector(1, 2)
v2 = Vector(3, 4)
print(v1 + v2)    # Vector(4, 6)
```

### 1.7 Class Methods, Static Methods, and Properties

```python
import datetime

class Employee:
    _headcount = 0

    def __init__(self, name: str, birth_year: int):
        self.name = name
        self.birth_year = birth_year
        Employee._headcount += 1

    @classmethod
    def from_string(cls, data: str) -> "Employee":
        name, year = data.split("-")
        return cls(name, int(year))

    @staticmethod
    def is_valid_year(year: int) -> bool:
        return 1900 < year < datetime.date.today().year

    @property
    def age(self) -> int:
        return datetime.date.today().year - self.birth_year

    @classmethod
    def headcount(cls) -> int:
        return cls._headcount

emp = Employee.from_string("Alice-1990")
print(emp.age)
```

### 1.8 Dataclasses

`dataclasses` (Python 3.7+) reduce boilerplate for data-holding classes.

```python
from dataclasses import dataclass, field
from typing import List

@dataclass(order=True, frozen=True)
class Point:
    x: float
    y: float

@dataclass
class Polygon:
    vertices: List[Point] = field(default_factory=list)

    def add_vertex(self, p: Point) -> None:
        self.vertices.append(p)

    def perimeter(self) -> float:
        import math
        pts = self.vertices
        return sum(
            math.hypot(pts[i].x - pts[i-1].x, pts[i].y - pts[i-1].y)
            for i in range(len(pts))
        )

p1, p2, p3 = Point(0, 0), Point(3, 0), Point(0, 4)
tri = Polygon([p1, p2, p3])
print(f"Perimeter: {tri.perimeter()}")  # 12.0
```

### 1.9 Design Patterns

#### Singleton

```python
class Singleton:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance
```

#### Observer

```python
from typing import Callable, List

class EventBus:
    def __init__(self):
        self._listeners: List[Callable] = []

    def subscribe(self, fn: Callable) -> None:
        self._listeners.append(fn)

    def publish(self, event: object) -> None:
        for fn in self._listeners:
            fn(event)
```

#### Factory

```python
class ShapeFactory:
    _registry = {}

    @classmethod
    def register(cls, name: str):
        def decorator(shape_cls):
            cls._registry[name] = shape_cls
            return shape_cls
        return decorator

    @classmethod
    def create(cls, name: str, **kwargs) -> Shape:
        return cls._registry[name](**kwargs)

@ShapeFactory.register("circle")
class Circle(Shape): ...
```

#### Strategy

```python
from typing import Protocol

class SortStrategy(Protocol):
    def sort(self, data: list) -> list: ...

class BubbleSort:
    def sort(self, data: list) -> list:
        data = data[:]
        for i in range(len(data)):
            for j in range(len(data) - i - 1):
                if data[j] > data[j+1]:
                    data[j], data[j+1] = data[j+1], data[j]
        return data

class Sorter:
    def __init__(self, strategy: SortStrategy):
        self._strategy = strategy

    def sort(self, data: list) -> list:
        return self._strategy.sort(data)
```

---

## 2. Basic Data Structures

### 2.1 Arrays and Lists

Python's `list` is a dynamic array (amortised O(1) append). NumPy provides true fixed-type arrays.

| Operation | Average | Worst |
|-----------|---------|-------|
| Access | O(1) | O(1) |
| Append | O(1)* | O(n) |
| Insert at index | O(n) | O(n) |
| Delete at index | O(n) | O(n) |
| Search | O(n) | O(n) |

*amortised

```python
import array

# typed array (compact memory)
arr = array.array('i', [1, 2, 3, 4, 5])

# numpy array (SIMD-accelerated)
import numpy as np
matrix = np.zeros((3, 3), dtype=np.float64)
```

### 2.2 Linked Lists

#### Singly Linked List

```
[head] -> [node1|next] -> [node2|next] -> [node3|None]
```

```python
from __future__ import annotations
from typing import Optional, Iterator, TypeVar, Generic

T = TypeVar("T")

class Node(Generic[T]):
    __slots__ = ("data", "next")

    def __init__(self, data: T) -> None:
        self.data = data
        self.next: Optional[Node[T]] = None

class SinglyLinkedList(Generic[T]):
    def __init__(self) -> None:
        self.head: Optional[Node[T]] = None
        self._size = 0

    def prepend(self, data: T) -> None:
        node = Node(data)
        node.next = self.head
        self.head = node
        self._size += 1

    def append(self, data: T) -> None:
        node = Node(data)
        if not self.head:
            self.head = node
        else:
            cur = self.head
            while cur.next:
                cur = cur.next
            cur.next = node
        self._size += 1

    def delete(self, data: T) -> bool:
        if not self.head:
            return False
        if self.head.data == data:
            self.head = self.head.next
            self._size -= 1
            return True
        cur = self.head
        while cur.next:
            if cur.next.data == data:
                cur.next = cur.next.next
                self._size -= 1
                return True
            cur = cur.next
        return False

    def __iter__(self) -> Iterator[T]:
        cur = self.head
        while cur:
            yield cur.data
            cur = cur.next

    def __len__(self) -> int:
        return self._size
```

#### Doubly Linked List

```python
class DNode(Generic[T]):
    __slots__ = ("data", "prev", "next")

    def __init__(self, data: T) -> None:
        self.data = data
        self.prev: Optional[DNode[T]] = None
        self.next: Optional[DNode[T]] = None

class DoublyLinkedList(Generic[T]):
    def __init__(self) -> None:
        self.head: Optional[DNode[T]] = None
        self.tail: Optional[DNode[T]] = None
        self._size = 0

    def append(self, data: T) -> None:
        node = DNode(data)
        if self.tail:
            self.tail.next = node
            node.prev = self.tail
        else:
            self.head = node
        self.tail = node
        self._size += 1
```

### 2.3 Stacks

LIFO (Last-In, First-Out). Python `list` or `collections.deque`.

```python
from collections import deque

class Stack(Generic[T]):
    def __init__(self) -> None:
        self._data: deque[T] = deque()

    def push(self, item: T) -> None:
        self._data.append(item)

    def pop(self) -> T:
        if not self._data:
            raise IndexError("pop from empty stack")
        return self._data.pop()

    def peek(self) -> T:
        return self._data[-1]

    def __len__(self) -> int:
        return len(self._data)

    def is_empty(self) -> bool:
        return not self._data
```

**Application — Balanced Parentheses:**

```python
def is_balanced(s: str) -> bool:
    pairs = {')': '(', ']': '[', '}': '{'}
    stack: list[str] = []
    for ch in s:
        if ch in '([{':
            stack.append(ch)
        elif ch in ')]}':
            if not stack or stack[-1] != pairs[ch]:
                return False
            stack.pop()
    return not stack
```

### 2.4 Queues and Deques

FIFO (First-In, First-Out). `collections.deque` gives O(1) append and popleft.

```python
from collections import deque
import heapq

class Queue(Generic[T]):
    def __init__(self) -> None:
        self._data: deque[T] = deque()

    def enqueue(self, item: T) -> None:
        self._data.append(item)

    def dequeue(self) -> T:
        if not self._data:
            raise IndexError("dequeue from empty queue")
        return self._data.popleft()

    def __len__(self) -> int:
        return len(self._data)

class PriorityQueue(Generic[T]):
    """Min-heap backed priority queue."""
    def __init__(self) -> None:
        self._heap: list = []
        self._counter = 0

    def push(self, priority: float, item: T) -> None:
        heapq.heappush(self._heap, (priority, self._counter, item))
        self._counter += 1

    def pop(self) -> T:
        _, _, item = heapq.heappop(self._heap)
        return item
```

### 2.5 Hash Tables / Dictionaries

Python's `dict` is a highly optimised open-addressing hash table (load factor ≈ 2/3).

| Operation | Average | Worst |
|-----------|---------|-------|
| Lookup | O(1) | O(n) |
| Insert | O(1) | O(n) |
| Delete | O(1) | O(n) |

```python
# Custom hash map with chaining
class HashMap(Generic[T]):
    def __init__(self, capacity: int = 16) -> None:
        self._capacity = capacity
        self._buckets: list[list] = [[] for _ in range(capacity)]
        self._size = 0

    def _hash(self, key) -> int:
        # Python's built-in hash() uses a randomised seed per process
        # (PYTHONHASHSEED).  Suitable for internal bucketing only —
        # do NOT use for security-sensitive hashing.
        return hash(key) % self._capacity

    def put(self, key, value: T) -> None:
        idx = self._hash(key)
        for pair in self._buckets[idx]:
            if pair[0] == key:
                pair[1] = value
                return
        self._buckets[idx].append([key, value])
        self._size += 1
        if self._size / self._capacity > 0.75:
            self._resize()

    def get(self, key) -> Optional[T]:
        idx = self._hash(key)
        for pair in self._buckets[idx]:
            if pair[0] == key:
                return pair[1]
        return None

    def _resize(self) -> None:
        old = self._buckets
        self._capacity *= 2
        self._buckets = [[] for _ in range(self._capacity)]
        self._size = 0
        for bucket in old:
            for key, val in bucket:
                self.put(key, val)
```

### 2.6 Sets

Python `set` is a hash table without values; supports O(1) average membership tests.

```python
a = {1, 2, 3}
b = {2, 3, 4}
print(a | b)   # union     {1, 2, 3, 4}
print(a & b)   # intersect {2, 3}
print(a - b)   # diff      {1}
print(a ^ b)   # sym diff  {1, 4}
```

---

## 3. Trees

### 3.1 Binary Trees

A tree where each node has at most two children.

```
        1
       / \
      2   3
     / \   \
    4   5   6
```

```python
from typing import Optional, List
from collections import deque

class BinaryTreeNode(Generic[T]):
    def __init__(self, val: T):
        self.val = val
        self.left: Optional[BinaryTreeNode[T]] = None
        self.right: Optional[BinaryTreeNode[T]] = None

def inorder(root: Optional[BinaryTreeNode]) -> List:
    return (inorder(root.left) + [root.val] + inorder(root.right)) if root else []

def preorder(root: Optional[BinaryTreeNode]) -> List:
    return ([root.val] + preorder(root.left) + preorder(root.right)) if root else []

def postorder(root: Optional[BinaryTreeNode]) -> List:
    return (postorder(root.left) + postorder(root.right) + [root.val]) if root else []

def level_order(root: Optional[BinaryTreeNode]) -> List[List]:
    if not root:
        return []
    result, queue = [], deque([root])
    while queue:
        level = []
        for _ in range(len(queue)):
            node = queue.popleft()
            level.append(node.val)
            if node.left:  queue.append(node.left)
            if node.right: queue.append(node.right)
        result.append(level)
    return result
```

### 3.2 Binary Search Trees (BST)

Invariant: `left.val < node.val < right.val`.

| Operation | Average | Worst (skewed) |
|-----------|---------|----------------|
| Search | O(log n) | O(n) |
| Insert | O(log n) | O(n) |
| Delete | O(log n) | O(n) |

```python
class BST(Generic[T]):
    def __init__(self):
        self.root: Optional[BinaryTreeNode[T]] = None

    def insert(self, val: T) -> None:
        def _insert(node, v):
            if not node:
                return BinaryTreeNode(v)
            if v < node.val:
                node.left = _insert(node.left, v)
            elif v > node.val:
                node.right = _insert(node.right, v)
            return node
        self.root = _insert(self.root, val)

    def search(self, val: T) -> bool:
        cur = self.root
        while cur:
            if val == cur.val:
                return True
            cur = cur.left if val < cur.val else cur.right
        return False

    def delete(self, val: T) -> None:
        def _min(node):
            while node.left:
                node = node.left
            return node

        def _delete(node, v):
            if not node:
                return node
            if v < node.val:
                node.left = _delete(node.left, v)
            elif v > node.val:
                node.right = _delete(node.right, v)
            else:
                if not node.left:
                    return node.right
                if not node.right:
                    return node.left
                succ = _min(node.right)
                node.val = succ.val
                node.right = _delete(node.right, succ.val)
            return node
        self.root = _delete(self.root, val)
```

### 3.3 AVL Trees

Self-balancing BST. Height difference between left and right subtrees ≤ 1. Guarantees O(log n) for all operations.

```python
class AVLNode:
    def __init__(self, key):
        self.key = key
        self.left = self.right = None
        self.height = 1

class AVLTree:
    def _height(self, n):
        return n.height if n else 0

    def _bf(self, n):
        return self._height(n.left) - self._height(n.right) if n else 0

    def _update(self, n):
        n.height = 1 + max(self._height(n.left), self._height(n.right))

    def _rotate_right(self, z):
        y, T3 = z.left, z.left.right
        y.right = z
        z.left = T3
        self._update(z)
        self._update(y)
        return y

    def _rotate_left(self, z):
        y, T2 = z.right, z.right.left
        y.left = z
        z.right = T2
        self._update(z)
        self._update(y)
        return y

    def _balance(self, node, key):
        self._update(node)
        bf = self._bf(node)
        if bf > 1:
            if key > node.left.key:
                node.left = self._rotate_left(node.left)
            return self._rotate_right(node)
        if bf < -1:
            if key < node.right.key:
                node.right = self._rotate_right(node.right)
            return self._rotate_left(node)
        return node

    def insert(self, root, key):
        if not root:
            return AVLNode(key)
        if key < root.key:
            root.left = self.insert(root.left, key)
        elif key > root.key:
            root.right = self.insert(root.right, key)
        else:
            return root
        return self._balance(root, key)
```

### 3.4 Red-Black Trees

A balanced BST with an extra colour bit per node. Used in Python's `sortedcontainers`, Java's `TreeMap`, C++ STL's `std::map`.

**Properties:**
1. Every node is red or black.
2. The root is black.
3. Every leaf (NIL) is black.
4. Red nodes have only black children.
5. All paths from a node to its descendant NIL leaves have the same number of black nodes.

```python
# Python's sortedcontainers.SortedList is backed by a list-of-lists achieving
# O(sqrt(n)) amortised for insert/delete and O(log n) for search.
from sortedcontainers import SortedList

sl = SortedList([5, 1, 3, 2, 4])
sl.add(0)
print(sl)          # SortedList([0, 1, 2, 3, 4, 5])
print(sl[2])       # 2  (O(log n))
sl.discard(3)
```

### 3.5 Heaps and Priority Queues

A **binary heap** is a complete binary tree satisfying the heap property. Python's `heapq` implements a min-heap.

| Operation | Time |
|-----------|------|
| Push | O(log n) |
| Pop min | O(log n) |
| Peek min | O(1) |
| Heapify | O(n) |

```python
import heapq

heap = []
heapq.heappush(heap, 3)
heapq.heappush(heap, 1)
heapq.heappush(heap, 4)
print(heapq.heappop(heap))   # 1

# heapify in O(n)
data = [5, 3, 8, 1, 2]
heapq.heapify(data)

# nlargest / nsmallest
print(heapq.nlargest(3, data))   # [8, 5, 3]
```

### 3.6 Tries (Prefix Trees)

Efficient for string prefix queries. O(m) per operation where m = string length.

```python
class TrieNode:
    __slots__ = ("children", "is_end")
    def __init__(self):
        self.children: dict[str, TrieNode] = {}
        self.is_end = False

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word: str) -> None:
        cur = self.root
        for ch in word:
            cur = cur.children.setdefault(ch, TrieNode())
        cur.is_end = True

    def search(self, word: str) -> bool:
        cur = self.root
        for ch in word:
            if ch not in cur.children:
                return False
            cur = cur.children[ch]
        return cur.is_end

    def starts_with(self, prefix: str) -> bool:
        cur = self.root
        for ch in prefix:
            if ch not in cur.children:
                return False
            cur = cur.children[ch]
        return True

    def autocomplete(self, prefix: str) -> list[str]:
        cur = self.root
        for ch in prefix:
            if ch not in cur.children:
                return []
            cur = cur.children[ch]
        results = []
        def dfs(node, path):
            if node.is_end:
                results.append(prefix + path)
            for c, child in node.children.items():
                dfs(child, path + c)
        dfs(cur, "")
        return results
```

### 3.7 Segment Trees

Supports range queries and point updates in O(log n).

```python
class SegmentTree:
    def __init__(self, data: list[int]) -> None:
        n = len(data)
        self._n = n
        self._tree = [0] * (4 * n)
        self._build(data, 0, 0, n - 1)

    def _build(self, data, node, start, end):
        if start == end:
            self._tree[node] = data[start]
        else:
            mid = (start + end) // 2
            self._build(data, 2*node+1, start, mid)
            self._build(data, 2*node+2, mid+1, end)
            self._tree[node] = self._tree[2*node+1] + self._tree[2*node+2]

    def query(self, l: int, r: int, node=0, start=None, end=None) -> int:
        if start is None: start, end = 0, self._n - 1
        if r < start or end < l:
            return 0
        if l <= start and end <= r:
            return self._tree[node]
        mid = (start + end) // 2
        return (self.query(l, r, 2*node+1, start, mid) +
                self.query(l, r, 2*node+2, mid+1, end))

    def update(self, idx: int, val: int, node=0, start=None, end=None):
        if start is None: start, end = 0, self._n - 1
        if start == end:
            self._tree[node] = val
        else:
            mid = (start + end) // 2
            if idx <= mid:
                self.update(idx, val, 2*node+1, start, mid)
            else:
                self.update(idx, val, 2*node+2, mid+1, end)
            self._tree[node] = self._tree[2*node+1] + self._tree[2*node+2]
```

### 3.8 Fenwick Trees (Binary Indexed Trees)

Compact structure for prefix-sum queries and point updates in O(log n) time and O(n) space.

```python
class FenwickTree:
    def __init__(self, n: int) -> None:
        self._n = n
        self._tree = [0] * (n + 1)

    def update(self, i: int, delta: int) -> None:
        while i <= self._n:
            self._tree[i] += delta
            i += i & (-i)

    def prefix_sum(self, i: int) -> int:
        total = 0
        while i > 0:
            total += self._tree[i]
            i -= i & (-i)
        return total

    def range_sum(self, l: int, r: int) -> int:
        return self.prefix_sum(r) - self.prefix_sum(l - 1)
```

### 3.9 B-Trees and B+ Trees

Used in databases and filesystems (e.g., PostgreSQL, ext4).

- **B-Tree**: Every node can hold up to `2t - 1` keys. All operations O(log n).
- **B+ Tree**: Data only in leaf nodes; leaves form a linked list for efficient range queries.

```python
# High-level concept (production implementations use C extensions)
class BTreeNode:
    def __init__(self, t: int, leaf: bool = False):
        self.t = t            # minimum degree
        self.leaf = leaf
        self.keys: list = []
        self.children: list["BTreeNode"] = []
```

---

## 4. Graphs

### 4.1 Graph Representations

```python
from collections import defaultdict
from typing import Dict, List, Tuple

# Adjacency list (sparse graphs)
graph: Dict[int, List[int]] = defaultdict(list)
graph[0].extend([1, 2])
graph[1].append(3)

# Adjacency matrix (dense graphs)
V = 5
adj_matrix = [[0] * V for _ in range(V)]
adj_matrix[0][1] = adj_matrix[1][0] = 1   # undirected

# Weighted edge list
edges: List[Tuple[int, int, float]] = [(0, 1, 2.5), (1, 2, 1.0)]
```

### 4.2 BFS and DFS

```python
from collections import deque
from typing import Optional, Set

def bfs(graph: Dict[int, List[int]], start: int) -> List[int]:
    visited: Set[int] = {start}
    queue = deque([start])
    order = []
    while queue:
        node = queue.popleft()
        order.append(node)
        for nb in graph[node]:
            if nb not in visited:
                visited.add(nb)
                queue.append(nb)
    return order

def dfs(graph: Dict[int, List[int]], start: int) -> List[int]:
    visited: Set[int] = set()
    order = []
    def _dfs(u):
        visited.add(u)
        order.append(u)
        for v in graph[u]:
            if v not in visited:
                _dfs(v)
    _dfs(start)
    return order
```

### 4.3 Shortest Path Algorithms

#### Dijkstra — O((V + E) log V)

```python
import heapq

def dijkstra(graph: Dict[int, List[Tuple[int, float]]], src: int) -> Dict[int, float]:
    dist = {src: 0.0}
    pq = [(0.0, src)]
    while pq:
        d, u = heapq.heappop(pq)
        if d > dist.get(u, float('inf')):
            continue
        for v, w in graph[u]:
            nd = d + w
            if nd < dist.get(v, float('inf')):
                dist[v] = nd
                heapq.heappush(pq, (nd, v))
    return dist
```

#### Bellman-Ford — O(VE), handles negative edges

```python
def bellman_ford(V: int, edges: List[Tuple[int,int,float]], src: int):
    dist = [float('inf')] * V
    dist[src] = 0
    for _ in range(V - 1):
        for u, v, w in edges:
            if dist[u] + w < dist[v]:
                dist[v] = dist[u] + w
    # detect negative cycle
    for u, v, w in edges:
        if dist[u] + w < dist[v]:
            raise ValueError("Negative cycle detected")
    return dist
```

#### Floyd-Warshall — O(V³), all-pairs

```python
def floyd_warshall(dist: List[List[float]]) -> List[List[float]]:
    V = len(dist)
    d = [row[:] for row in dist]
    for k in range(V):
        for i in range(V):
            for j in range(V):
                d[i][j] = min(d[i][j], d[i][k] + d[k][j])
    return d
```

### 4.4 Minimum Spanning Trees

#### Kruskal — O(E log E)

```python
def kruskal(V: int, edges: List[Tuple[float, int, int]]) -> List[Tuple[float, int, int]]:
    parent = list(range(V))
    rank = [0] * V

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def union(a, b):
        ra, rb = find(a), find(b)
        if ra == rb: return False
        if rank[ra] < rank[rb]: ra, rb = rb, ra
        parent[rb] = ra
        if rank[ra] == rank[rb]: rank[ra] += 1
        return True

    mst = []
    for w, u, v in sorted(edges):
        if union(u, v):
            mst.append((w, u, v))
    return mst
```

### 4.5 Topological Sort

```python
def topological_sort(graph: Dict[int, List[int]], V: int) -> List[int]:
    indegree = [0] * V
    for u in graph:
        for v in graph[u]:
            indegree[v] += 1
    queue = deque(u for u in range(V) if indegree[u] == 0)
    order = []
    while queue:
        u = queue.popleft()
        order.append(u)
        for v in graph[u]:
            indegree[v] -= 1
            if indegree[v] == 0:
                queue.append(v)
    if len(order) != V:
        raise ValueError("Graph has a cycle")
    return order
```

### 4.6 Strongly Connected Components

**Kosaraju's algorithm** — two DFS passes, O(V + E).

```python
def kosaraju(V: int, graph: Dict[int, List[int]]) -> List[List[int]]:
    visited = [False] * V
    order = []

    def dfs1(u):
        visited[u] = True
        for v in graph.get(u, []):
            if not visited[v]:
                dfs1(v)
        order.append(u)

    for u in range(V):
        if not visited[u]:
            dfs1(u)

    rev = defaultdict(list)
    for u in graph:
        for v in graph[u]:
            rev[v].append(u)

    visited = [False] * V
    sccs = []

    def dfs2(u, scc):
        visited[u] = True
        scc.append(u)
        for v in rev.get(u, []):
            if not visited[v]:
                dfs2(v, scc)

    for u in reversed(order):
        if not visited[u]:
            scc: List[int] = []
            dfs2(u, scc)
            sccs.append(scc)
    return sccs
```

---

## 5. Advanced and Modern Data Structures

### 5.1 Bloom Filters

A probabilistic data structure that tests set membership with **no false negatives** and a controlled false-positive rate. Space-efficient for large sets (e.g., databases, network routers, spell checkers).

```python
import math
import mmh3   # MurmurHash3
from bitarray import bitarray

class BloomFilter:
    def __init__(self, n: int, fp_rate: float = 0.01) -> None:
        m = -int(n * math.log(fp_rate) / (math.log(2) ** 2))
        k = int((m / n) * math.log(2))
        self._m = m
        self._k = k
        self._bits = bitarray(m)
        self._bits.setall(0)

    def add(self, item: str) -> None:
        for i in range(self._k):
            idx = mmh3.hash(item, i) % self._m
            self._bits[idx] = 1

    def __contains__(self, item: str) -> bool:
        return all(
            self._bits[mmh3.hash(item, i) % self._m]
            for i in range(self._k)
        )
```

### 5.2 Skip Lists

A probabilistic alternative to balanced BSTs. O(log n) average for all operations.

```python
import random

class SkipNode:
    def __init__(self, key, level):
        self.key = key
        self.forward = [None] * (level + 1)

class SkipList:
    MAX_LEVEL = 16
    P = 0.5

    def __init__(self):
        self.header = SkipNode(float('-inf'), self.MAX_LEVEL)
        self.level = 0

    def _random_level(self) -> int:
        lvl = 0
        while random.random() < self.P and lvl < self.MAX_LEVEL:
            lvl += 1
        return lvl

    def insert(self, key) -> None:
        update = [None] * (self.MAX_LEVEL + 1)
        cur = self.header
        for i in range(self.level, -1, -1):
            while cur.forward[i] and cur.forward[i].key < key:
                cur = cur.forward[i]
            update[i] = cur
        lvl = self._random_level()
        if lvl > self.level:
            for i in range(self.level + 1, lvl + 1):
                update[i] = self.header
            self.level = lvl
        node = SkipNode(key, lvl)
        for i in range(lvl + 1):
            node.forward[i] = update[i].forward[i]
            update[i].forward[i] = node

    def search(self, key) -> bool:
        cur = self.header
        for i in range(self.level, -1, -1):
            while cur.forward[i] and cur.forward[i].key < key:
                cur = cur.forward[i]
        cur = cur.forward[0]
        return cur is not None and cur.key == key
```

### 5.3 Disjoint Set Union (Union-Find)

Used in Kruskal's MST, connected components, and cycle detection. Near-O(1) amortised with path compression and union by rank.

```python
class DSU:
    def __init__(self, n: int) -> None:
        self.parent = list(range(n))
        self.rank = [0] * n
        self.components = n

    def find(self, x: int) -> int:
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])  # path compression
        return self.parent[x]

    def union(self, a: int, b: int) -> bool:
        ra, rb = self.find(a), self.find(b)
        if ra == rb:
            return False
        if self.rank[ra] < self.rank[rb]:
            ra, rb = rb, ra
        self.parent[rb] = ra
        if self.rank[ra] == self.rank[rb]:
            self.rank[ra] += 1
        self.components -= 1
        return True
```

### 5.4 LRU Cache

Implemented with an `OrderedDict` (doubly linked list + hash map), O(1) get/put.

```python
from collections import OrderedDict

class LRUCache(Generic[T]):
    def __init__(self, capacity: int) -> None:
        self._cap = capacity
        self._cache: OrderedDict = OrderedDict()

    def get(self, key) -> Optional[T]:
        if key not in self._cache:
            return None
        self._cache.move_to_end(key)
        return self._cache[key]

    def put(self, key, value: T) -> None:
        if key in self._cache:
            self._cache.move_to_end(key)
        self._cache[key] = value
        if len(self._cache) > self._cap:
            self._cache.popitem(last=False)

# Python 3.9+: functools.lru_cache / functools.cache
from functools import lru_cache

@lru_cache(maxsize=128)
def fib(n: int) -> int:
    return n if n < 2 else fib(n-1) + fib(n-2)
```

### 5.5 Suffix Arrays and Suffix Trees

Crucial for bioinformatics, text search engines (e.g., full-text indexing).

```python
def build_suffix_array(s: str) -> list[int]:
    """O(n log^2 n) construction."""
    n = len(s)
    sa = sorted(range(n), key=lambda i: s[i:])
    return sa

def lcp_array(s: str, sa: list[int]) -> list[int]:
    """Kasai's O(n) LCP algorithm."""
    n = len(s)
    rank = [0] * n
    for i, suffix_idx in enumerate(sa):
        rank[suffix_idx] = i
    lcp = [0] * n
    h = 0
    for i in range(n):
        if rank[i] > 0:
            j = sa[rank[i] - 1]
            while i + h < n and j + h < n and s[i+h] == s[j+h]:
                h += 1
            lcp[rank[i]] = h
            if h:
                h -= 1
    return lcp
```

### 5.6 Persistent Data Structures

A **persistent** data structure preserves old versions after modification (path-copying). Used in functional programming and version control.

```python
class PersistentStack(Generic[T]):
    """Immutable, persistent stack using structural sharing."""
    def __init__(self, head: Optional[T] = None, tail: Optional["PersistentStack[T]"] = None):
        self._head = head
        self._tail = tail

    @property
    def is_empty(self) -> bool:
        return self._head is None

    def push(self, val: T) -> "PersistentStack[T]":
        return PersistentStack(val, self)

    def pop(self) -> tuple[T, "PersistentStack[T]"]:
        if self.is_empty:
            raise IndexError("pop from empty stack")
        return self._head, self._tail

s0 = PersistentStack()
s1 = s0.push(1)
s2 = s1.push(2)
val, s3 = s2.pop()   # s1 still intact
```

### 5.7 Rope (String Data Structure)

A **rope** is a binary tree where leaves hold string fragments. Enables O(log n) concatenation and O(log n) split, beneficial for large text editors.

```python
class RopeNode:
    def __init__(self, s: str = ""):
        self.left: Optional[RopeNode] = None
        self.right: Optional[RopeNode] = None
        self.data: str = s
        self.weight: int = len(s)

    def is_leaf(self) -> bool:
        return self.left is None and self.right is None

class Rope:
    def __init__(self, s: str = ""):
        self.root = RopeNode(s)

    def __len__(self) -> int:
        return self.root.weight

    def to_string(self, node: Optional[RopeNode] = None) -> str:
        if node is None:
            node = self.root
        if node.is_leaf():
            return node.data
        return self.to_string(node.left) + self.to_string(node.right)
```

### 5.8 Van Emde Boas Trees

Supports insert, delete, successor and predecessor queries in O(log log U) time where U is the universe size. Used when U is bounded (e.g., 32-bit integers).

```python
class VEBTree:
    """Van Emde Boas tree for universe {0, ..., U-1}."""
    def __init__(self, U: int):
        self.U = U
        self.min = self.max = None
        if U > 2:
            sq = int(math.ceil(math.sqrt(U)))
            self.summary = VEBTree(sq)
            self.cluster = [VEBTree(sq) for _ in range(sq)]

    @staticmethod
    def _high(x, sq): return x // sq

    @staticmethod
    def _low(x, sq): return x % sq

    def insert(self, x: int) -> None:
        sq = int(math.ceil(math.sqrt(self.U)))
        if self.min is None:
            self.min = self.max = x
            return
        if x < self.min: x, self.min = self.min, x
        if self.U > 2:
            if self.cluster[self._high(x, sq)].min is None:
                self.summary.insert(self._high(x, sq))
            self.cluster[self._high(x, sq)].insert(self._low(x, sq))
        if x > self.max:
            self.max = x
```

---

## 6. Algorithms

### 6.1 Sorting Algorithms

| Algorithm | Best | Average | Worst | Space | Stable |
|-----------|------|---------|-------|-------|--------|
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) | ✓ |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | ✗ |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) | ✓ |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) | ✓ |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) | ✗ |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | O(1) | ✗ |
| Counting Sort | O(n+k) | O(n+k) | O(n+k) | O(k) | ✓ |
| Radix Sort | O(nk) | O(nk) | O(nk) | O(n+k) | ✓ |
| Tim Sort | O(n) | O(n log n) | O(n log n) | O(n) | ✓ |

```python
def merge_sort(arr: list) -> list:
    if len(arr) <= 1:
        return arr
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    return _merge(left, right)

def _merge(l, r):
    result, i, j = [], 0, 0
    while i < len(l) and j < len(r):
        if l[i] <= r[j]:
            result.append(l[i]); i += 1
        else:
            result.append(r[j]); j += 1
    result.extend(l[i:]); result.extend(r[j:])
    return result

def quick_sort(arr: list, lo: int = 0, hi: int = None) -> list:
    if hi is None:
        arr, hi = arr[:], len(arr) - 1
    if lo < hi:
        p = _partition(arr, lo, hi)
        quick_sort(arr, lo, p - 1)
        quick_sort(arr, p + 1, hi)
    return arr

def _partition(arr, lo, hi):
    pivot = arr[hi]
    i = lo - 1
    for j in range(lo, hi):
        if arr[j] <= pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    arr[i+1], arr[hi] = arr[hi], arr[i+1]
    return i + 1
```

### 6.2 Searching Algorithms

```python
def binary_search(arr: list, target) -> int:
    lo, hi = 0, len(arr) - 1
    while lo <= hi:
        mid = lo + (hi - lo) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            lo = mid + 1
        else:
            hi = mid - 1
    return -1

import bisect

def bisect_search(arr: list, target) -> int:
    idx = bisect.bisect_left(arr, target)
    return idx if idx < len(arr) and arr[idx] == target else -1
```

### 6.3 Divide and Conquer

```python
def max_subarray(arr: list[int]) -> int:
    """Kadane's / divide-and-conquer — O(n log n)."""
    def helper(lo, hi):
        if lo == hi:
            return arr[lo]
        mid = (lo + hi) // 2
        left_sum = right_sum = 0
        cur = 0
        for i in range(mid, lo - 1, -1):
            cur += arr[i]; left_sum = max(left_sum, cur)
        cur = 0
        for i in range(mid + 1, hi + 1):
            cur += arr[i]; right_sum = max(right_sum, cur)
        return max(helper(lo, mid), helper(mid+1, hi), left_sum + right_sum)
    return helper(0, len(arr) - 1)
```

### 6.4 Dynamic Programming

#### Memoisation vs Tabulation

```python
# Longest Common Subsequence — O(mn)
def lcs(a: str, b: str) -> int:
    m, n = len(a), len(b)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if a[i-1] == b[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
    return dp[m][n]

# 0/1 Knapsack — O(nW)
def knapsack(weights: list[int], values: list[int], W: int) -> int:
    n = len(weights)
    dp = [0] * (W + 1)
    for i in range(n):
        for w in range(W, weights[i] - 1, -1):
            dp[w] = max(dp[w], dp[w - weights[i]] + values[i])
    return dp[W]

# Coin Change — O(amount * coins)
def coin_change(coins: list[int], amount: int) -> int:
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0
    for coin in coins:
        for x in range(coin, amount + 1):
            dp[x] = min(dp[x], dp[x - coin] + 1)
    return dp[amount] if dp[amount] != float('inf') else -1
```

### 6.5 Greedy Algorithms

```python
# Activity Selection
def activity_selection(start: list[int], end: list[int]) -> list[int]:
    activities = sorted(zip(end, start, range(len(start))))
    selected = [activities[0][2]]
    last_end = activities[0][0]
    for e, s, idx in activities[1:]:
        if s >= last_end:
            selected.append(idx)
            last_end = e
    return selected

# Huffman Coding
import heapq
from collections import Counter

def huffman(text: str) -> dict[str, str]:
    freq = Counter(text)
    heap = [[w, [sym, ""]] for sym, w in freq.items()]
    heapq.heapify(heap)
    while len(heap) > 1:
        lo = heapq.heappop(heap)
        hi = heapq.heappop(heap)
        for pair in lo[1:]: pair[1] = '0' + pair[1]
        for pair in hi[1:]: pair[1] = '1' + pair[1]
        heapq.heappush(heap, [lo[0] + hi[0]] + lo[1:] + hi[1:])
    return dict(sorted(heapq.heappop(heap)[1:], key=lambda p: (len(p[-1]), p)))
```

### 6.6 Backtracking

```python
def n_queens(n: int) -> list[list[int]]:
    solutions = []
    board = []

    def is_safe(row, col):
        for r, c in enumerate(board):
            if c == col or abs(r - row) == abs(c - col):
                return False
        return True

    def backtrack(row):
        if row == n:
            solutions.append(board[:])
            return
        for col in range(n):
            if is_safe(row, col):
                board.append(col)
                backtrack(row + 1)
                board.pop()

    backtrack(0)
    return solutions
```

### 6.7 Bit Manipulation

```python
# Common bit tricks
x = 42
print(x & (x - 1))        # clear lowest set bit
print(x & (-x))            # isolate lowest set bit
print(bin(x).count('1'))   # popcount

def single_number(nums: list[int]) -> int:
    """XOR all — duplicates cancel out."""
    return __import__('functools').reduce(lambda a, b: a ^ b, nums)

def count_bits(n: int) -> list[int]:
    """Count set bits for 0..n — O(n)."""
    dp = [0] * (n + 1)
    for i in range(1, n + 1):
        dp[i] = dp[i >> 1] + (i & 1)
    return dp
```

### 6.8 String Algorithms

#### KMP (Knuth-Morris-Pratt) — O(n + m)

```python
def kmp_search(text: str, pattern: str) -> list[int]:
    def build_lps(p):
        lps = [0] * len(p)
        length, i = 0, 1
        while i < len(p):
            if p[i] == p[length]:
                length += 1; lps[i] = length; i += 1
            elif length:
                length = lps[length - 1]
            else:
                lps[i] = 0; i += 1
        return lps

    lps = build_lps(pattern)
    matches, i, j = [], 0, 0
    while i < len(text):
        if text[i] == pattern[j]:
            i += 1; j += 1
        if j == len(pattern):
            matches.append(i - j)
            j = lps[j - 1]
        elif i < len(text) and text[i] != pattern[j]:
            j = lps[j - 1] if j else (i := i + 1) and 0
    return matches
```

#### Rabin-Karp Rolling Hash — O(n + m) average

```python
def rabin_karp(text: str, pattern: str) -> list[int]:
    BASE, MOD = 256, 10**9 + 7
    m, n = len(pattern), len(text)
    if m > n: return []
    ph = th = h = 1
    for _ in range(m - 1): h = h * BASE % MOD
    for i in range(m):
        ph = (BASE * ph + ord(pattern[i])) % MOD
        th = (BASE * th + ord(text[i])) % MOD
    matches = []
    for i in range(n - m + 1):
        if ph == th and text[i:i+m] == pattern:
            matches.append(i)
        if i < n - m:
            th = (BASE * (th - ord(text[i]) * h) + ord(text[i+m])) % MOD
    return matches
```

---

## 7. Complexity Analysis

### Big-O Notation

| Notation | Name | Example |
|----------|------|---------|
| O(1) | Constant | Hash table lookup |
| O(log n) | Logarithmic | Binary search |
| O(n) | Linear | Linear scan |
| O(n log n) | Linearithmic | Merge sort |
| O(n²) | Quadratic | Bubble sort |
| O(2ⁿ) | Exponential | Brute-force subset enumeration |
| O(n!) | Factorial | Brute-force TSP |

### Master Theorem

For recurrences T(n) = aT(n/b) + f(n):

- If f(n) = O(n^(log_b(a) - ε)) → T(n) = Θ(n^log_b(a))
- If f(n) = Θ(n^log_b(a)) → T(n) = Θ(n^log_b(a) · log n)
- If f(n) = Ω(n^(log_b(a) + ε)) → T(n) = Θ(f(n))

```python
# Profiling with timeit
import timeit
import sys

def profile_sort(n=10000):
    data = list(range(n, 0, -1))
    t = timeit.timeit(lambda: sorted(data), number=100)
    return t / 100
```

---

## 8. Data Management and Governance

### 8.1 Data Pipelines in Python

```python
from typing import Iterator, Callable, TypeVar
S = TypeVar("S")

def pipeline(*funcs: Callable) -> Callable:
    """Compose a sequence of transformations."""
    def run(data):
        for fn in funcs:
            data = fn(data)
        return data
    return run

# Generator-based lazy pipeline
def read_csv(path: str) -> Iterator[dict]:
    import csv
    with open(path) as f:
        yield from csv.DictReader(f)

def filter_rows(predicate: Callable) -> Callable[[Iterator], Iterator]:
    def inner(rows):
        return (r for r in rows if predicate(r))
    return inner

def transform(fn: Callable) -> Callable[[Iterator], Iterator]:
    def inner(rows):
        return (fn(r) for r in rows)
    return inner
```

### 8.2 Data Quality and Validation

```python
from dataclasses import dataclass
from typing import Any
import re

class ValidationError(Exception): ...

@dataclass
class Schema:
    rules: dict[str, Callable[[Any], bool]]

    def validate(self, record: dict) -> dict:
        errors = {}
        for field, rule in self.rules.items():
            value = record.get(field)
            if not rule(value):
                errors[field] = f"Validation failed for '{field}': {value!r}"
        if errors:
            raise ValidationError(errors)
        return record

email_schema = Schema({
    "email": lambda v: isinstance(v, str) and re.fullmatch(r"[^@]+@[^@]+\.[^@]+", v) is not None,
    "age":   lambda v: isinstance(v, int) and 0 < v < 150,
})
```

### 8.3 Metadata Management

```python
from datetime import datetime, timezone
from uuid import uuid4

@dataclass
class DataAsset:
    name: str
    owner: str
    schema: dict
    tags: list[str]
    created_at: datetime = None
    id: str = None

    def __post_init__(self):
        if self.created_at is None:
            self.created_at = datetime.now(timezone.utc)
        if self.id is None:
            self.id = str(uuid4())

    def to_catalog_entry(self) -> dict:
        return {
            "id": self.id,
            "name": self.name,
            "owner": self.owner,
            "schema": self.schema,
            "tags": self.tags,
            "created_at": self.created_at.isoformat(),
        }
```

### 8.4 Data Versioning

```python
from copy import deepcopy
from typing import Any

class VersionedStore:
    """Append-only versioned key-value store."""
    def __init__(self):
        self._versions: list[dict] = [{}]

    def put(self, key: str, value: Any) -> int:
        snapshot = deepcopy(self._versions[-1])
        snapshot[key] = value
        self._versions.append(snapshot)
        return len(self._versions) - 1

    def get(self, key: str, version: int = -1) -> Any:
        return self._versions[version].get(key)

    def history(self, key: str) -> list[tuple[int, Any]]:
        return [(v, snap[key]) for v, snap in enumerate(self._versions) if key in snap]
```

### 8.5 Distributed Data Structures

```python
# Consistent Hashing Ring — used in distributed caches (Redis Cluster, Cassandra)
import hashlib
from bisect import bisect, insort

class ConsistentHashRing:
    def __init__(self, replicas: int = 150) -> None:
        self._replicas = replicas
        self._ring: list[int] = []
        self._nodes: dict[int, str] = {}

    def _hash(self, key: str) -> int:
        return int(hashlib.md5(key.encode()).hexdigest(), 16)

    def add_node(self, node: str) -> None:
        for i in range(self._replicas):
            h = self._hash(f"{node}:{i}")
            insort(self._ring, h)
            self._nodes[h] = node

    def remove_node(self, node: str) -> None:
        for i in range(self._replicas):
            h = self._hash(f"{node}:{i}")
            self._ring.remove(h)
            del self._nodes[h]

    def get_node(self, key: str) -> str:
        if not self._ring:
            raise ValueError("Empty ring")
        h = self._hash(key)
        idx = bisect(self._ring, h) % len(self._ring)
        return self._nodes[self._ring[idx]]
```

---

## 9. References

### Books

1. **Cormen, T. H., Leiserson, C. E., Rivest, R. L., & Stein, C.** (2022). *Introduction to Algorithms* (4th ed.). MIT Press.
2. **Sedgewick, R., & Wayne, K.** (2011). *Algorithms* (4th ed.). Addison-Wesley Professional.
3. **Knuth, D. E.** (1997). *The Art of Computer Programming, Vol. 1–3* (3rd ed.). Addison-Wesley.
4. **Goodrich, M. T., Tamassia, R., & Goldwasser, M. H.** (2013). *Data Structures and Algorithms in Python*. Wiley.
5. **Gamma, E., Helm, R., Johnson, R., & Vlissides, J.** (1994). *Design Patterns: Elements of Reusable Object-Oriented Software*. Addison-Wesley.
6. **Lutz, M.** (2013). *Learning Python* (5th ed.). O'Reilly Media.
7. **Ramalho, L.** (2022). *Fluent Python* (2nd ed.). O'Reilly Media.
8. **Skiena, S. S.** (2020). *The Algorithm Design Manual* (3rd ed.). Springer.
9. **Okasaki, C.** (1998). *Purely Functional Data Structures*. Cambridge University Press.
10. **van Rossum, G., & Drake, F. L.** (2009). *Python 3 Reference Manual*. CreateSpace.

### Research Papers

1. Bloom, B. H. (1970). Space/time trade-offs in hash coding with allowable errors. *Communications of the ACM*, 13(7), 422–426.
2. Pugh, W. (1990). Skip lists: A probabilistic alternative to balanced trees. *Communications of the ACM*, 33(6), 668–676.
3. Van Emde Boas, P. (1977). Preserving order in a forest in less than logarithmic time and linear space. *Information Processing Letters*, 6(3), 80–82.
4. Tarjan, R. E. (1975). Efficiency of a good but not linear set union algorithm. *Journal of the ACM*, 22(2), 215–225.
5. Kasai, T., Lee, G., Arimura, H., Arikawa, S., & Park, K. (2001). Linear-time longest-common-prefix computation in suffix arrays and its applications. *CPM 2001, LNCS 2089*, 181–192.
6. Aho, A. V., & Corasick, M. J. (1975). Efficient string matching: An aid to bibliographic search. *Communications of the ACM*, 18(6), 333–340.
7. Fredman, M. L., & Tarjan, R. E. (1987). Fibonacci heaps and their uses in improved network optimization algorithms. *Journal of the ACM*, 34(3), 596–615.
8. Karp, R. M., & Rabin, M. O. (1987). Efficient randomized pattern-matching algorithms. *IBM Journal of Research and Development*, 31(2), 249–260.
9. Dijkstra, E. W. (1959). A note on two problems in connexion with graphs. *Numerische Mathematik*, 1(1), 269–271.
10. Bellman, R. (1958). On a routing problem. *Quarterly of Applied Mathematics*, 16(1), 87–90.
11. Floyd, R. W. (1962). Algorithm 97: Shortest path. *Communications of the ACM*, 5(6), 345.
12. Kruskal, J. B. (1956). On the shortest spanning subtree of a graph and the traveling salesman problem. *Proceedings of the American Mathematical Society*, 7(1), 48–50.
13. Knuth, D. E., Morris, J. H., & Pratt, V. R. (1977). Fast pattern matching in strings. *SIAM Journal on Computing*, 6(2), 323–350.

---

> **See also:** [`README.tex`](./README.tex) — the companion LaTeX document with TikZ diagrams, formal proofs, and complete BibTeX references for all entries above.
