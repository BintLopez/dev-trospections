Board (static 16 x 16)
Walls and targets (changes each game)
Goal (changes each turn)
Robot positions (changes each move)

Tree data structure
each node has exactly one parent
need to know how to search through the tree -- many algos for this

Depth-first search
* follow one branch all the way to the end and then move across the tree
* typically done recursively
* naive depth-first search algos don't have cycle detection

Graph is like a tree

Breadth-first search
* go down the levels of a tree
* global visited list

Array#rotate -- http://ruby-doc.org/core-2.2.0/Array.html#method-i-rotate

comparing sorted arrays faster than sorting sets
sorted set has really bad performance in ruby

Search Algorithms recommended reading
Basil and Fabian: Three Ways Through

Best-First Search
* try to figure out which node of the tree is the next best one to explore

A* algo
* each state is scored
* lowest score is best
* socre = distance so far + estimated distance left
* must not overestimate

'monotonic'

use a profiler -- oftentimes your intuition about what would be performant is wrong