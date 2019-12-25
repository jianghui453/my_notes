# trie (前缀树|字典树)

Trie is an efficient information reTrieval data structure. Using Trie, search complexities can be brought to optimal limit (key length). If we store keys in binary search tree, a well balanced BST will need time proportional to M * log N, where M is maximum string length and N is number of keys in tree. Using Trie, we can search the key in O(M) time. However the penalty is on Trie storage requirements (Please refer Applications of Trie for more details)

![](../Images/DataStructures/trie/1.png)

Every node of Trie consists of multiple branches. Each branch represents a possible character of keys. We need to mark the last node of every key as end of word node. A Trie node field isEndOfWord is used to distinguish the node as end of word node. A simple structure to represent nodes of the English alphabet can be as following,
```
// Trie node
struct TrieNode
{
     struct TrieNode *children[ALPHABET_SIZE];

     // isEndOfWord is true if the node
     // represents end of a word
     bool isEndOfWord;
};
```
Inserting a key into Trie is a simple approach. Every character of the input key is inserted as an individual Trie node. Note that the children is an array of pointers (or references) to next level trie nodes. The key character acts as an index into the array children. If the input key is new or an extension of the existing key, we need to construct non-existing nodes of the key, and mark end of the word for the last node. If the input key is a prefix of the existing key in Trie, we simply mark the last node of the key as the end of a word. The key length determines Trie depth.



Searching for a key is similar to insert operation, however, we only compare the characters and move down. The search can terminate due to the end of a string or lack of key in the trie. In the former case, if the isEndofWord field of the last node is true, then the key exists in the trie. In the second case, the search terminates without examining all the characters of the key, since the key is not present in the trie.

The following picture explains construction of trie using keys given in the example below,
```
                       root
                    /   \    \
                    t   a     b
                    |   |     |
                    h   n     y
                    |   |  \  |
                    e   s  y  e
                 /  |   |
                 i  r   w
                 |  |   |
                 r  e   e
                        |
                        r
```
In the picture, every character is of type trie_node_t. For example, the root is of type trie_node_t, and it’s children a, b and t are filled, all other nodes of root will be NULL. Similarly, “a” at the next level is having only one child (“n”), all other children are NULL. The leaf nodes are in blue.

## implementation

```
type TrieNode struct {
	Val byte
	Children []*TrieNode
}

func NewTrieNode(v byte) *TrieNode {
	n := new(TrieNode)
	n.Val = v
	n.Children = make([]*TrieNode, 0)
	return n
}

type Trie struct {
    Root *TrieNode
}


/** Initialize your data structure here. */
func Constructor() Trie {
	var t Trie
	t.Root = NewTrieNode('0')
	t.Root.Children = append(t.Root.Children, NewTrieNode('0'))
	return t
}


/** Inserts a word into the trie. */
func (this *Trie) Insert(word string)  {
	l := len(word)
	node := this.Root
loop:
	for i := 0; i < l; i++ {
		for j := range node.Children {
			if node.Children[j].Val == word[i] {
				node = node.Children[j]
				continue loop
			}
		}

		newnode := NewTrieNode(word[i])
		node.Children = append(node.Children, newnode)
		node = newnode
	}

	for i := range node.Children {
		if node.Children[i].Val == '0' {
			return
		}
	}

	node.Children = append(node.Children, NewTrieNode('0'))
}


/** Returns if the word is in the trie. */
func (this *Trie) Search(word string) bool {
	r := this.traversal(word)
	if r == nil {
		return false
	}

	for i := range r.Children {
		if r.Children[i].Val == '0' {
			return true
		}
	}

	return false
}


/** Returns if there is any word in the trie that starts with the given prefix. */
func (this *Trie) StartsWith(prefix string) bool {
	r := this.traversal(prefix)

	return r != nil
}

func (this *Trie) traversal(str string) *TrieNode {
	node := this.Root
	l := len(str)
loop:
	for i := 0; i < l; i++ {
		for j := range node.Children {
			if str[i] == node.Children[j].Val {
				node = node.Children[j]
				continue loop
			}
		}

		return nil
	}

	return node
}
```

## reference

1. [geeksforgeeks: Trie | (Insert and Search)](https://www.geeksforgeeks.org/trie-insert-and-search/)