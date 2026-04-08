````java
class Node {
    int data;
    Node next;

    Node(int data) {
        this.data = data;
        this.next = null;
    }
}

public class LinkedList {

    Node head;

    // ---------------- INSERTION ----------------

    // 1. Insert at Head
    public void insertAtHead(int data) {
        Node newNode = new Node(data);
        newNode.next = head;
        head = newNode;
    }

    // 2. Insert at Last
    public void insertAtLast(int data) {
        Node newNode = new Node(data);
        if (head == null) {
            head = newNode;
            return;
        }

        Node temp = head;
        while (temp.next != null) {
            temp = temp.next;
        }
        temp.next = newNode;
    }

    // 3. Insert at Position (0-based index)
    public void insertAtPosition(int data, int pos) {
        if (pos == 0) {
            insertAtHead(data);
            return;
        }

        Node newNode = new Node(data);
        Node temp = head;

        for (int i = 0; i < pos - 1 && temp != null; i++) {
            temp = temp.next;
        }

        if (temp == null) return;

        newNode.next = temp.next;
        temp.next = newNode;
    }

    // 4. Insert after a Value
    public void insertAfterValue(int data, int target) {
        Node temp = head;

        while (temp != null && temp.data != target) {
            temp = temp.next;
        }

        if (temp == null) return;

        Node newNode = new Node(data);
        newNode.next = temp.next;
        temp.next = newNode;
    }

    // ---------------- DELETION ----------------

    // 5. Delete Head
    public void deleteHead() {
        if (head == null) return;
        head = head.next;
    }

    // 6. Delete Last
    public void deleteLast() {
        if (head == null || head.next == null) {
            head = null;
            return;
        }

        Node temp = head;
        while (temp.next.next != null) {
            temp = temp.next;
        }
        temp.next = null;
    }

    // 7. Delete at Position (0-based index)
    public void deleteAtPosition(int pos) {
        if (head == null) return;

        if (pos == 0) {
            deleteHead();
            return;
        }

        Node temp = head;

        for (int i = 0; i < pos - 1 && temp.next != null; i++) {
            temp = temp.next;
        }

        if (temp.next == null) return;

        temp.next = temp.next.next;
    }

    // 8. Delete by Value
    public void deleteByValue(int target) {
        if (head == null) return;

        if (head.data == target) {
            head = head.next;
            return;
        }

        Node temp = head;

        while (temp.next != null && temp.next.data != target) {
            temp = temp.next;
        }

        if (temp.next == null) return;

        temp.next = temp.next.next;
    }

    // ---------------- UTILITY ----------------

    public void printList() {
        Node temp = head;
        while (temp != null) {
            System.out.print(temp.data + " -> ");
            temp = temp.next;
        }
        System.out.println("null");
    }

    // ---------------- MAIN ----------------

    public static void main(String[] args) {
        LinkedList list = new LinkedList();

        list.insertAtHead(3);
        list.insertAtHead(1);
        list.insertAtLast(5);
        list.insertAtPosition(2, 1);
        list.insertAfterValue(4, 3);

        list.printList();

        list.deleteHead();
        list.deleteLast();
        list.deleteAtPosition(1);
        list.deleteByValue(3);

        list.printList();
    }
}
````