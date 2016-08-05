LINKED LISTS

- linked lists are foundational to many data structures (ie trees and graphs)

*What is a linked list?*

linear data structure of nodes (there is an order)
-- each node has two values -- the value of the node itself and the ref to its next node
each element has a reference that points to the next element
(a pointer is an address in memory)
each element has a value (number, string, object, whatever)
the first element is called the head
the last element is called the tail and its next element is some special value meaning "no element"

(note -- in lisp languages, tail is everything but the head)

C specific things about pointers
* -- takes an address in mem and returns the thing
& -- takes the thing and returns its address in mem

'on the heap' -- means dynamically created

* Array (in lower level languages) -- upon creation give the size. It creates a static address in mem for the first ele in the array and then the mem addy of the other els can be implied

* Linked list -- doesn't matter where all the nodes are in memory

no random access to linked lists
no easy indexing
can be less efficient memory-wise

# Doubly Linked List
- Every element has a reference to the previous element as well as the next

# Circularly-linked list
- The last element's "next" points to the first element

# Dummy head node
- variation on a circularly-linked list, has a "no value" node as the head


#INTERVIEW QUESTIONS

* Implement a simple linked list
* Write a separate, more complex, linked list implementation
* Return value of the nth node
* 
