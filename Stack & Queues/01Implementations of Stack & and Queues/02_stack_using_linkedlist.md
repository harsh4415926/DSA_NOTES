# Stack using linkedlist
````java

public class LinkedListStack {

    // Node class to represent each element in the stack
    private class Node {
        int data;
        Node next;

        Node(int data) {
            this.data = data;
            this.next = null;
        }
    }

    private Node top; // The pointer to the top of the stack

    // Constructor
    public LinkedListStack() {
        this.top = null;
    }

    // 1. PUSH: Add element to the top (Head of the list)
    public void push(int value) {
        Node newNode = new Node(value);
        
        // Link the new node to the current top
        newNode.next = top;
        
        // Update top to point to the new node
        top = newNode;
        System.out.println("Pushed " + value + " to stack");
    }

    // 2. POP: Remove element from the top (Head of the list)
    public int pop() {
        if (isEmpty()) {
            System.out.println("Stack Underflow! The stack is empty.");
            return -1; // Indicate error
        }
        
        int poppedValue = top.data;
        top = top.next; // Move top pointer to the next node
        return poppedValue;
    }

    // 3. PEEK: View the top element
    public int peek() {
        if (isEmpty()) {
            System.out.println("Stack is empty");
            return -1;
        }
        return top.data;
    }

    // 4. Utility: Check if stack is empty
    public boolean isEmpty() {
        return (top == null);
    }

    // Main method for testing
    public static void main(String[] args) {
        LinkedListStack stack = new LinkedListStack();

        stack.push(10);
        stack.push(20);
        stack.push(30);

        System.out.println("Top element is: " + stack.peek());

        System.out.println("Popped element: " + stack.pop());
        System.out.println("Popped element: " + stack.pop());

        if (stack.isEmpty()) {
            System.out.println("Stack is empty");
        } else {
            System.out.println("Stack is not empty");
        }
    }
}
````