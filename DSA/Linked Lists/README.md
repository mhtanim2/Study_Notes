# Linked List in Data Structures

A **Linked List** is a linear data structure where elements (nodes) are stored in non-contiguous memory locations. Each node contains data and a reference (pointer) to the next node in the sequence.

---

## Difference Between Array, List, and Linked List

| Feature         | Array / ArrayList           | List<T> (C# Generic List) | Linked List         |
|-----------------|----------------------------|---------------------------|---------------------|
| Memory          | Contiguous                 | Contiguous                | Non-contiguous      |
| Access Time     | O(1) (by index)            | O(1) (by index)           | O(n) (by position)  |
| Insert/Delete   | Slow (except at end)       | Slow (except at end)      | Fast (at ends/middle)|
| Dynamic Size    | ArrayList/List<T>: Yes     | Yes                       | Yes                 |
| Memory Overhead | Low                        | Low                       | Higher (pointers)   |

- **Array/ArrayList/List<T>**: Fast random access, but slow insertions/deletions except at the end.
- **Linked List**: Slower access, but efficient insertions and deletions anywhere.
> Complexity
![LL vs AL](./resource/LL%20ArrayL%20complexity.png)

---



## Linked List
A linked list is a `linear data structure` that stores a collection of elements called **`nodes`**, where each node **contains** `data and a pointer` to the `next node` in the sequence.

### Node
##### What
A node is the fundamental `building block` of a linked list, combination of **`value`** and **`reference`**.
##### Why
Give us the structure of building blocks for the mechanism
```csharp
public class Node{
    public int data;
    public Node? next;
    
    public Node(int d){
        data = d;
    }
}
```

---

## When to Use Linked List

- When you need frequent insertions and deletions in the middle of the collection.
- When memory allocation for large contiguous blocks is difficult.
- When you do not need fast random access.

---

## Types of Linked Lists

- **Singly Linked List**: Each node points to the next node.
- **Doubly Linked List**: Each node points to both next and previous nodes.
- **Circular Linked List**: Last node points back to the first node.

---

Linked lists are fundamental for understanding dynamic data structures and are widely used in scenarios where efficient insertions and