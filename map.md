# Hashmaps

Arrays are pretty similar to hash maps already. Arrays let you quickly look up the value for a given "key" . . . except the keys are called "indices," and we don't get to pick them—they're always sequential integers (0, 1, 2, 3, etc).

Think of a hash map as a "hack" on top of an array to let us use flexible keys instead of being stuck with sequential integer "indices."

![image](https://www.interviewcake.com/images/svgs/cs_for_hackers__hash_tables_lies_key_labeled.svg?bust=210)

To look up the value for a given key, we just run the key through our hashing function to get the index to go to in our underlying array to grab the value.

How does that hashing method work? There are a few different approaches, and they can get pretty complicated. But here's a simple proof of concept:
Grab the number value for each character and add those up.

![image](https://www.interviewcake.com/images/svgs/cs_for_hackers__hash_tables_lies_chars.svg?bust=210)

The result is 429. But what if we only have 30 slots in our array? We'll use a common trick for forcing a number into a specific range: the modulus operator (%). Modding our sum by 30 ensures we get a whole number that's less than 30 (and at least 0):

```dotnetcli
429 % 30 = 9
```

## Hash collisions

What if two keys hash to the same index in our array? In our example above, look at "lies" and "foes":

![image](https://www.interviewcake.com/images/svgs/cs_for_hackers__hash_tables_lies_and_foes_addition.svg?bust=210)

They both sum up to 429! So of course they'll have the same answer when we mod by 30:

This is called a hash collision. There are a few different strategies for dealing with them.

Here's a common one: instead of storing the actual values in our array, let's have each array slot hold a pointer to a linked list holding the values for all the keys that hash to that index:

![image](https://www.interviewcake.com/images/svgs/cs_for_hackers__hash_tables_hash_collision_key_val.svg?bust=210)

### Lets build a LRU(Least Recently Used) (Caching mechanism)

So we will basically use two data structure here to create LRU

1. Hashmap (to store key: value: LinkedListNode(which will be a linked list node))
1. LinkedList

```typescript
class LRUCache {
  cache: Record<number, MyNode | undefined>;
  size: number;
  capacity: number;
  head: MyNode | null;
  tail: MyNode | null;

  constructor(capacity: number) {
    this.capacity = capacity;
    this.cache = {};
    this.head = null;
    this.tail = null;
    this.size = 0;
  }

  reorder(node: MyNode) {
    if (this.size <= 1) return;

    if (this.head === node) return;

    if (this.tail === node) {
      node.prior.next = null;
      this.tail = node.prior;
    } else {
      node.prior.next = node.next;
      node.next.prior = node.prior;
    }

    node.next = this.head;
    this.head.prior = node;

    this.head = node;
  }

  add(key: number, value: number): void {
    const node = new MyNode();

    node.key = key;
    node.value = value;

    if (this.size > 0) {
      node.next = this.head;
      node.prior = null;

      this.head.prior = node;

      this.head = node;
    } else {
      node.prior = null;
      node.next = null;

      this.head = node;
      this.tail = node;
    }

    this.cache[key] = node;
    this.size += 1;

    return;
  }

  remove(target: MyNode) {
    if (this.size <= 1) {
      this.head = null;
      this.tail = null;
    } else {
      if (target === this.head) {
        target.next.prior = null;
        this.head = target.next;
      } else if (target === this.tail) {
        target.prior.next = null;
        this.tail = target.prior;
      } else {
        target.prior.next = target.next;
        target.next.prior = target.prior;
      }
    }

    this.cache[target.key] = undefined;
    this.size -= 1;
  }

  get(key: number): number {
    const target = this.cache[key];

    if (target) {
      this.reorder(target);
      return target.value;
    } else {
      return -1;
    }
  }

  put(key: number, value: number): void {
    const target = this.cache[key];
    if (target) {
      target.value = value;
      this.reorder(target);
      return;
    }

    if (this.size >= this.capacity) {
      this.remove(this.tail);
    }

    this.add(key, value);
  }
}

class MyNode {
  prior: MyNode | null;
  next: MyNode | null;
  value: number;
  key: number;
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * var obj = new LRUCache(capacity)
 * var param_1 = obj.get(key)
 * obj.put(key,value)
 */
```
