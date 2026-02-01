# LRU Cache (leetcode)
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 146 - LRU Cache
 * ----------------------------------------------------------------------
 * Design a cache that evicts the Least Recently Used (LRU) item when full.
 * * STEPS:
 * 1. Use HashMap for O(1) access + Doubly Linked List (DLL) for O(1) ordering.
 * 2. Get/Put -> Move accessed node to Head (MRU). Capacity full -> Remove Tail (LRU).
 * ----------------------------------------------------------------------
 */

class LRUCache {

    private class Node {
        int key, value;
        Node prev, next;

        Node(int key, int value) {
            this.key = key;
            this.value = value;
        }
    }

    private HashMap<Integer, Node> map;
    private int capacity;
    private Node head, tail;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        map = new HashMap<>();

        // Initialize dummy head and tail to simplify edge cases
        head = new Node(-1, -1);
        tail = new Node(-1, -1);

        head.next = tail;
        tail.prev = head;
    }

    // Unlink a node from the DLL
    private void remove(Node node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    // Insert node right after head (Most Recently Used position)
    private void insertAtHead(Node node) {
        node.next = head.next;
        head.next.prev = node;
        head.next = node;
        node.prev = head;
    }

    public int get(int key) {
        if (!map.containsKey(key)) {
            return -1;
        }

        // Accessing a key makes it the Most Recently Used
        Node node = map.get(key);
        remove(node);      // Remove from current position
        insertAtHead(node); // Move to front
        return node.value;
    }

    public void put(int key, int value) {
        // Case 1: Key already exists -> Update value and move to head
        if (map.containsKey(key)) {
            Node node = map.get(key);
            node.value = value;
            remove(node);
            insertAtHead(node);
            return;
        }

        // Case 2: Capacity reached -> Evict LRU (node before tail)
        if (map.size() == capacity) {
            Node lru = tail.prev;   
            remove(lru);            // Remove from DLL
            map.remove(lru.key);    // Remove from Map
        }

        // Case 3: Insert new node
        Node node = new Node(key, value);
        insertAtHead(node);
        map.put(key, node);
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(1)
 * - Get and Put operations involve only HashMap lookups and pointer 
 * updates, which are constant time.
 * * Space Complexity: O(capacity)
 * - Stores at most 'capacity' nodes in Map and DLL.
 * ----------------------------------------------------------------------
 */
````