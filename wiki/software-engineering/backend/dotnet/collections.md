---
tags: dotnet
---

# Collection Criteria

Decide which Collection type to use based on the most required action on a Collection and following Criteria:

| Criterion          | Collection Type                                              |
| ------------------ | ------------------------------------------------------------ |
| Fixed Size         | `T[]`                                                        |
| Dynamic Size       | `List<T>`, `LinkedList<T>`                                   |
| Indexed Access     | `T[]`, `List<T>`                                             |
| Key-Value Pairs    | `Dictionary<TKey, TValue>`                                   |
| Unique Elements    | `HashSet<T>`                                                 |
| Insertion Order    | `List<T>`, `LinkedList<T>`                                   |
| Sorted Order       | `SortedList<TKey, TValue>`, `SortedDictionary<TKey, TValue>` |
| Thread-Safe        | `ConcurrentDictionary<TKey, TValue>`, `ConcurrentBag<T>`     |
| Deferred Execution | `IEnumerable<T>`                                             |
| Abstraction        | `IList<T>`, `ICollection<T>`, `IEnumerable<T>`               |

# Comparison

| Collection Type                  | Advantages                                                                                                            | Disadvantages                                                            | When to Use                                                           | Memory Usage                                    | Complexity                                           |
| -------------------------------- | --------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ | --------------------------------------------------------------------- | ----------------------------------------------- | ---------------------------------------------------- |
| `T[]`                            | Fast access, low memory overhead                                                                                      | Fixed size                                                               | Known and fixed size in private methods                               | Low                                             | Access: O(1), Insert: O(n), Remove: O(n)             |
| `Dictionary<TKey, TValue>`       | Fast lookups by key                                                                                                   | More memory overhead than lists or arrays                                | Key-value associations, fast lookups                                  | Higher (due to hash table implementation)       | Access: O(1), Insert: O(1), Remove: O(1)             |
| `HashSet<T>`                     | Fast operations, unique elements                                                                                      | Unordered collection                                                     | Unique elements, fast operations                                      | Higher (due to hash table implementation)       | Access: O(1), Insert: O(1), Remove: O(1)             |
| `Queue<T>`                       | Fast enqueue and dequeue                                                                                              | No indexed access                                                        | FIFO processing                                                       | Moderate                                        | Enqueue: O(1), Dequeue: O(1)                         |
| `Stack<T>`                       | Fast push and pop                                                                                                     | No indexed access                                                        | LIFO processing                                                       | Moderate                                        | Push: O(1), Pop: O(1)                                |
| `LinkedList<T>`                  | Efficient insertions/deletions                                                                                        | Higher memory overhead, slower indexed access                            | Efficient insertions/deletions, no fast indexed access                | Higher (due to node pointers)                   | Access: O(n), Insert: O(1), Remove: O(1)             |
| `SortedList<TKey, TValue>`       | Maintains sorted order, fast lookups                                                                                  | Slower insertions/deletions                                              | Sorted collection, fast lookups                                       | Higher (due to maintaining order)               | Access: O(log n), Insert: O(n), Remove: O(n)         |
| `SortedDictionary<TKey, TValue>` | Maintains sorted order, fast lookups                                                                                  | Slower insertions/deletions                                              | Sorted dictionary, fast lookups                                       | Higher (due to maintaining order)               | Access: O(log n), Insert: O(log n), Remove: O(log n) |
| `ObservableCollection<T>`        | Notifications for changes                                                                                             | Higher overhead due to event handling                                    | Data binding in UI applications                                       | Higher (due to event handling)                  | Access: O(1), Insert: O(1)\*, Remove: O(n)           |
| `IEnumerable<T>`                 | High Abstraction (good for exposing data), Deferred execution, minimal memory overhead                                | Forward-only iteration (iteration one by one), no direct access          | Simple, read-only collection, LINQ queries                            | Minimal (especially for lazy evaluation)        | Access: O(n), Insert: N/A, Remove: N/A               |
| `List<T>`                        | Dynamic resizing, rich methods                                                                                        | Higher memory overhead than arrays                                       | Unknown or dynamic size                                               | Moderate (due to resizing overhead)             | Access: O(1), Insert: O(1)\*, Remove: O(n)           |
| `IList<T>`                       | Same as List, but provides Abstraction for flexible implementation (e.g LinkedList, ObservableCollection can be used) | Not inherently thread-safe (race-condition), higher overhead than arrays | When you need indexed access and collection manipulation capabilities | Depends on the implementation (e.g., `List<T>`) | Access: O(1), Insert: O(1)\*, Remove: O(n)           |

O(1) = Operation time constant, regardless of the size
O(1)\* = Average time is constant, but indivdual operations can vary
O(n) = Time increases linearly with the size

# IEnumerable

## Deferred Execution

-   The execution of the query is postponed until you actually iterate over the data.
-   This mproves performance and reduces memory usage by not executing until needed and only processing the required data.

Example:

```
// Define an array of numbers
int[] numbers = { 1, 2, 3, 4, 5 };

// Create a LINQ query with deferred execution
IEnumerable<int> query = numbers.Where(n => n > 2);

// The query is not executed yet, so no filtering happens here
Console.WriteLine("Query created.");

// Iterate over the query
foreach (int number in query)
{
    Console.WriteLine(number); // Execution happens here, filtering the numbers
}

```
