# Trie

A trie, also called a digital tree and sometimes a radix tree or prefix tree (as prefixes can search them), is a kind of search tree—an ordered tree data structure that is used for pattern matching.
The trie has the following features

Each node but the root is labeled with a character.
Nodes are alphabetically ordered from left to right
The path from child to root yields the string
Each node has 26 pointer to other trie node from a to z.

![image](https://upload.wikimedia.org/wikipedia/commons/thumb/b/be/Trie_example.svg/400px-Trie_example.svg.png)

```typescript
class TrieNode {
  public children: { [id: string]: TrieNode } = {};

  public content: String;
  public isWord: boolean;
  constructor(i = '') {
    this.content = i;
  }
}
class Trie {
  public root: TrieNode;
  constructor() {
    this.root = new TrieNode();
  }

  public insert(word) {
    let current = this.root;
    for (let i = 0; i < word.length; i++) {
      if (current[word[i]] == null) {
        current.children[word[i]] = new TrieNode(word[i]);
      }
      current = current.children[word[i]];
    }
    current.isWord = true;
  }

  public find(word): boolean {
    let current = this.root;
    for (let i = 0; i < word.length; i++) {
      const ch = word.charAt(i);
      const node = current.children[ch];
      if (node == null) {
        return false;
      }
      current = node;
    }
    return current.isWord;
  }

  countWords() {
    if (!this.root) {
      return console.log('No root node found');
    }
    var queue = [this.root];
    var counter = 0;
    while (queue.length) {
      var node = queue.shift();
      if (node.isWord) {
        counter++;
      }
      for (var child in node.children) {
        if (node.children.hasOwnProperty(child)) {
          queue.push(node.children[child]);
        }
      }
    }
    return counter;
  }
```

```typescript
 print() {
    if (!this.root) {
      return console.log('No root node found');
    }
    var newline = new TrieNode('|');
    var queue = [this.root, newline];
    var string = '';
    while (queue.length) {
      var node = queue.shift();
      string += node.content.toString() + ' ';
      if (node === newline && queue.length) {
        queue.push(newline);
      }
      for (var child in node.children) {
        if (node.children.hasOwnProperty(child)) {
          queue.push(node.children[child]);
        }
      }
    }
    console.log(string.slice(0, -2).trim());
  }
}
```

#### How to use

```typescript
let trie = new Trie();
trie;
trie.insert("Programming");
trie.insert("is");
trie.insert("a");
trie.insert("abc");
trie.insert("way");
trie.insert("of");
trie.insert("life");
trie.print();

console.log(trie.find("of"));
```
