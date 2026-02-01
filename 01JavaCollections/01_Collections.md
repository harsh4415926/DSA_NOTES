# Java Data Structures & Algorithms Cheat Sheet

A quick reference for common collections and their standard operations (CRUD: Create, Read, Update, Delete).

## 1. Strings (Text Processing)

### `String` (Immutable)
*Used when text does not change often.*

- **Length:** `str.length()`
- **Access:** `str.charAt(index)`
- **Substrings:**
    - `str.substring(beginIndex)`
    - `str.substring(beginIndex, endIndex)` (End index is exclusive)
- **Search:**
    - `str.indexOf("sub")` (Returns index or -1)
    - `str.lastIndexOf("sub")`
- **Comparison:**
    - `str.equals(otherStr)` (Value check)
    - `str.equalsIgnoreCase(otherStr)`
    - `str.compareTo(otherStr)` (Lexicographical)
- **Transformation:**
    - `str.toCharArray()` (Convert to `char[]`)
    - `str.trim()` (Remove whitespace)
    - `str.split("regex")` (Returns `String[]`)
    - `String.valueOf(data)` (Convert int/char to String)

### `StringBuilder` (Mutable)
*Used for frequent modifications (append/delete).*

- **Initialize:** `StringBuilder sb = new StringBuilder("init");`
- **Add:** `sb.append(value)`
- **Access:** `sb.charAt(index)`
- **Update:** `sb.setCharAt(index, char)`
- **Insert:** `sb.insert(index, value)`
- **Delete:**
    - `sb.deleteCharAt(index)`
    - `sb.delete(start, end)`
- **Reverse:** `sb.reverse()`
- **Convert:** `sb.toString()`

---

## 2. Linear Data Structures

### `ArrayList` (Dynamic Array)
*Good for random access (`get`), slow for removing from middle.*

- **Declaration:** `List<Integer> list = new ArrayList<>();`
- **Add:** `list.add(element)`
- **Insert:** `list.add(index, element)`
- **Access:** `list.get(index)`
- **Update:** `list.set(index, newValue)`
- **Remove:**
    - `list.remove(index)` (Removes by index)
    - `list.remove(Object o)` (Removes first occurrence of object)
- **Check:** `list.contains(element)`
- **Size:** `list.size()`
- **Sort:** `Collections.sort(list)` or `list.sort(Comparator)`
- **Array Conv:** `list.toArray()`

### `LinkedList` (Doubly Linked List)
*Good for removing/adding from ends, slow random access.*
*(Implements both List and Deque interfaces)*

- **Add:** `list.addFirst(e)`, `list.addLast(e)`
- **Remove:** `list.removeFirst()`, `list.removeLast()`
- **Access:** `list.getFirst()`, `list.getLast()`

---

## 3. Stacks & Queues

### `Stack` (LIFO - Last In, First Out)
*Note: Java docs recommend using `ArrayDeque` for stacks, but `Stack` class is still common in OA.*

- **Declaration:** `Stack<Integer> stack = new Stack<>();`
- **Add:** `stack.push(element)`
- **Remove:** `stack.pop()` (Returns & removes top)
- **Check Top:** `stack.peek()` (Returns top without removing)
- **Empty Check:** `stack.isEmpty()`

### `Queue` (FIFO - First In, First Out)
*Interface implemented by `LinkedList` or `PriorityQueue`.*

- **Declaration:** `Queue<Integer> q = new LinkedList<>();`
- **Add:** `q.offer(element)` (Safe) or `q.add(element)` (Throws exception)
- **Remove:** `q.poll()` (Returns null if empty) or `q.remove()`
- **Check Front:** `q.peek()` (Returns null if empty) or `q.element()`

### `Deque` (Double Ended Queue)
*The modern standard for Stacks and Queues.*

- **Declaration:** `Deque<Integer> dq = new ArrayDeque<>();`
- **As Stack:** `push()`, `pop()`, `peek()`
- **As Queue:** `offer()`, `poll()`, `peek()`
- **Both Ends:**
    - `addFirst()`, `addLast()`
    - `pollFirst()`, `pollLast()`
    - `peekFirst()`, `peekLast()`

### `PriorityQueue` (Min-Heap / Max-Heap)
*Default is Min-Heap (smallest element at top).*

- **Min-Heap:** `PriorityQueue<Integer> pq = new PriorityQueue<>();`
- **Max-Heap:** `PriorityQueue<Integer> pq = new PriorityQueue<>(Collections.reverseOrder());`
- **Add:** `pq.offer(element)`
- **Remove Top:** `pq.poll()` (Smallest/Largest)
- **Check Top:** `pq.peek()`

---

## 4. Hashing (Maps & Sets)

### `HashMap` (Key-Value Pairs)
*Unordered, O(1) average time complexity.*

- **Declaration:** `Map<String, Integer> map = new HashMap<>();`
- **Add/Update:** `map.put(key, value)`
- **Access:** `map.get(key)` (Returns null if missing)
- **Safe Access:** `map.getOrDefault(key, defaultValue)`
- **Check Key:** `map.containsKey(key)`
- **Check Value:** `map.containsValue(value)`
- **Remove:** `map.remove(key)`
- **Iterate:**
    - `map.keySet()` (Returns Set of keys)
    - `map.values()` (Returns Collection of values)
    - `map.entrySet()` (Returns Set of Entry objects)
    - *Loop:* `for (Map.Entry<K,V> entry : map.entrySet()) { ... }`

### `HashSet` (Unique Elements)
*Unordered, O(1) average time complexity.*

- **Declaration:** `Set<Integer> set = new HashSet<>();`
- **Add:** `set.add(element)` (Returns false if already exists)
- **Remove:** `set.remove(element)`
- **Check:** `set.contains(element)`
- **Empty:** `set.isEmpty()`

---

## 5. Sorted Structures (Tree Based)

### `TreeMap` (Sorted Map)
*Keys are sorted (Natural order or Comparator). O(log n).*

- **Methods:** Same as HashMap (`put`, `get`, `remove`).
- **Navigable Methods:**
    - `firstKey()`, `lastKey()`
    - `floorKey(k)` (Largest key <= k)
    - `ceilingKey(k)` (Smallest key >= k)
    - `lowerKey(k)` (Strictly < k)
    - `higherKey(k)` (Strictly > k)

### `TreeSet` (Sorted Set)
*Elements are sorted. O(log n).*

- **Methods:** Same as HashSet.
- **Navigable Methods:** Same as TreeMap (`first()`, `last()`, `floor()`, `ceiling()`).

---

## 6. Helper Classes (Utilities)

### `Arrays` (For primitive arrays `int[]`)
- `Arrays.sort(arr)`
- `Arrays.fill(arr, value)`
- `Arrays.toString(arr)` (Printing)
- `Arrays.binarySearch(arr, key)` (Must be sorted)
- `Arrays.copyOf(arr, newLength)`
- `Arrays.equals(arr1, arr2)`

### `Collections` (For Collections `List`, `Set`)
- `Collections.sort(list)`
- `Collections.reverse(list)`
- `Collections.max(collection)`
- `Collections.min(collection)`
- `Collections.swap(list, i, j)`
- `Collections.frequency(collection, obj)`

---

## Quick Complexity Summary (Big O)

| Data Structure | Access | Search | Insert | Delete |
| :--- | :--- | :--- | :--- | :--- |
| **ArrayList** | O(1) | O(n) | O(1)* | O(n) |
| **LinkedList** | O(n) | O(n) | O(1) | O(1) |
| **Stack/Queue** | O(1) (peek) | O(n) | O(1) | O(1) |
| **HashMap/Set** | - | O(1) | O(1) | O(1) |
| **TreeMap/Set** | - | O(log n) | O(log n) | O(log n) |
| **PriorityQueue**| O(1) (peek) | O(n) | O(log n) | O(log n) |

*\* Amortized time for insertion at end of ArrayList.*