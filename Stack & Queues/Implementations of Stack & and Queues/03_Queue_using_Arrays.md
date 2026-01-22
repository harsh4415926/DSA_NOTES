# queue using arrays
````java
public class ArrayQueue {
    private int[] queueArray;
    private int front; // Index of the front element
    private int rear;  // Index of the rear element
    private int capacity;
    private int currentSize;

    // Constructor
    public ArrayQueue(int size) {
        this.capacity = size;
        this.queueArray = new int[this.capacity];
        this.front = 0;
        this.rear = -1;
        this.currentSize = 0;
    }

    // 1. ENQUEUE: Add element to the rear of the queue
    public void enqueue(int value) {
        if (isFull()) {
            System.out.println("Queue Overflow! Cannot enqueue " + value);
            return;
        }
        // Calculate new rear position (Circular increment)
        rear = (rear + 1) % capacity;
        queueArray[rear] = value;
        currentSize++;
        
        System.out.println("Enqueued " + value);
    }

    // 2. DEQUEUE: Remove element from the front of the queue
    public int dequeue() {
        if (isEmpty()) {
            System.out.println("Queue Underflow! Cannot dequeue");
            return -1;
        }
        int removedValue = queueArray[front];
        
        // Calculate new front position (Circular increment)
        front = (front + 1) % capacity;
        currentSize--;
        
        return removedValue;
    }

    // 3. PEEK: Get the front element without removing it
    public int peek() {
        if (isEmpty()) {
            System.out.println("Queue is empty");
            return -1;
        }
        return queueArray[front];
    }

    // 4. Utility: Check if queue is empty
    public boolean isEmpty() {
        return (currentSize == 0);
    }

    // 5. Utility: Check if queue is full
    public boolean isFull() {
        return (currentSize == capacity);
    }
    
    // Main method to test
    public static void main(String[] args) {
        ArrayQueue queue = new ArrayQueue(5);

        queue.enqueue(10);
        queue.enqueue(20);
        queue.enqueue(30);
        queue.enqueue(40);
        queue.enqueue(50);
        
        // This will show Overflow because size is 5
        queue.enqueue(60); 

        System.out.println("Front element: " + queue.peek());

        System.out.println("Dequeued: " + queue.dequeue());
        System.out.println("Dequeued: " + queue.dequeue());

        // Now we can add more elements because of Circular Logic
        queue.enqueue(60);
        queue.enqueue(70);
        
        System.out.println("Current Front: " + queue.peek());
    }
}

````