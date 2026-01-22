# Queue using Linkedlist
````java
public class LinkedListQueue {

    // Node class representing each element
    private class Node {
        int data;
        Node next;

        Node(int data) {
            this.data = data;
            this.next = null;
        }
    }

    private Node front; // Points to the first element (removal side)
    private Node rear;  // Points to the last element (insertion side)

    public LinkedListQueue() {
        this.front = null;
        this.rear = null;
    }

    // 1. ENQUEUE: Add element to the end (Rear)
    public void enqueue(int value) {
        Node newNode = new Node(value);

        // Case 1: If the queue is empty, the new node is both front and rear
        if (isEmpty()) {
            front = newNode;
            rear = newNode;
        } else {
            // Case 2: Link the current rear to the new node and update rear
            rear.next = newNode;
            rear = newNode;
        }
        System.out.println("Enqueued: " + value);
    }

    // 2. DEQUEUE: Remove element from the start (Front)
    public int dequeue() {
        if (isEmpty()) {
            System.out.println("Queue Underflow! Cannot dequeue.");
            return -1;
        }

        int removedValue = front.data;
        front = front.next; // Move front pointer to the next node

        // Edge Case: If the queue is now empty, rear must also be null
        if (front == null) {
            rear = null;
        }

        return removedValue;
    }

    // 3. PEEK: Get the front element
    public int peek() {
        if (isEmpty()) {
            System.out.println("Queue is empty");
            return -1;
        }
        return front.data;
    }

    // 4. Utility: Check if queue is empty
    public boolean isEmpty() {
        return (front == null);
    }

    // Main method to test
    public static void main(String[] args) {
        LinkedListQueue queue = new LinkedListQueue();

        queue.enqueue(10);
        queue.enqueue(20);
        queue.enqueue(30);

        System.out.println("Front element: " + queue.peek());

        System.out.println("Dequeued: " + queue.dequeue());
        System.out.println("Dequeued: " + queue.dequeue());

        queue.enqueue(40);
        queue.enqueue(50);
        
        System.out.println("Dequeued: " + queue.dequeue());
        System.out.println("Current Front: " + queue.peek());
    }
}
````
