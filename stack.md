# Stack

Stacks are basically LIFO(First in first out) structure. Basically reverse of queues.

`a <- b <- c <- d`

so if you want to add a new element to this queue you basically append it to the start. What we do is `d.prev` become our new element and it is added in the stack and will also set the head to the new element

`a <- b <- c <- d <- new`

removing a element means the head now points the second element and first is removed

`a <- b <- c <- d`

All the operations on stack that is pushing and popping is a constant time.

### Implementation

```dotnetcli
interface Node<T> {
  value: T;
  prev?: Node<T>;
}

export default class Stack<T> {
  public length: number;
  public head?: Node<T>;

  constructor() {
    this.length = 0;
    this.head = undefined;
  }

  push(item: T): void {
    const node = { value: item, prev: undefined };
    if (!this.head) {
      this.head = node;
    }
    const prevNode = this.head;
    this.head = node;
    this.head.prev = prevNode;
    this.length += 1;
    return;
  }

  pop(): T | undefined {
    if (!this.head) return undefined;
    this.head = this.head.prev;
    this.length -= 1;
    return;
  }

  peek(): T | undefined {
    return this.head?.value;
  }
}

const test_node = new Stack();

test_node.push(1);
test_node.push(2);
test_node.push(3);
test_node.pop();
test_node.peek();
console.log(test_node.head);
console.log(test_node.length);
```
