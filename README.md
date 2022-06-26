### EX.NO: 02
### DATE: 28-04-2022
# Breadth First Search
## AIM
To develop an algorithm to find the route from the source to the destination point using breadth-first search.

## THEORY
Breadth First Search (BFS) Algorithm. 
Breadth first search is a graph traversal algorithm that starts traversing the graph from root node and explores all the neighbouring nodes.
Then, it selects the nearest node and explore all the unexplored nodes.
The algorithm follows the same process for each of the nearest node until it finds the goal.
It follows FIFO.It travel from left to right.

## DESIGN STEPS
### STEP 1:
Identify a location in the google map:
### STEP 2:
Select a specific number of nodes with distance
### STEP 3:
Import the required packages and create some methods to define and understand breadthfirstsearch.
### STEP 4:
Include the nodes(locations) and values(distance) in dictionary as keys and values.
### STEP 5:
Pass the required location it will return the distance and destination.

## ROUTE MAP
#### Include your own map
<img src="https://user-images.githubusercontent.com/75234807/166142528-9743e76e-7271-4562-ae82-c8d0093b1b0a.png" width="65%" height="50%">

## PROGRAM
```python 
#DEVELOPED BY: M.LOKESH KRISHNAA 
#REGISTER NO : 212220230030
```
```python
%matplotlib inline
import matplotlib.pyplot as plt
import random
import math
import sys
from collections import defaultdict, deque, Counter
from itertools import combinations
class Problem(object):
def __init__(self, initial=None, goal=None, **kwds): 
        self.__dict__.update(initial=initial, goal=goal, **kwds) 
def actions(self, state):        
        raise NotImplementedError
def result(self, state, action): 
        raise NotImplementedError
def is_goal(self, state):        
        return state == self.goal
def action_cost(self, s, a, s1): 
        return 1
def __str__(self):
        return '{0}({1}, {2})'.format(
            type(self).__name__, self.initial, self.goal)
class Node:
def __init__(self, state, parent=None, action=None, path_cost=0):
        self.__dict__.update(state=state, parent=parent, action=action, path_cost=path_cost)

def __str__(self): 
        return '<{0}>'.format(self.state)        
failure = Node('failure', path_cost=math.inf) # Indicates an algorithm couldn't find a solution.
cutoff  = Node('cutoff',  path_cost=math.inf) # Indicates iterative deepening search was cut off.
def expand(problem, node):
    s = node.state
    for action in problem.actions(s):
        s1 = problem.result(s, action)
        cost = node.path_cost + problem.action_cost(s, action, s1)
        yield Node(s1, node, action, cost)
def path_actions(node):
    if node.parent is None:
        return []  
    return path_actions(node.parent) + [node.action]
def path_states(node):
    if node in (cutoff, failure, None): 
        return []
    return path_states(node.parent) + [node.state]
FIFOQueue = deque
def breadth_first_search(problem):
    node = Node(problem.initial)
    if problem.is_goal(problem.initial):
        return node
    # Remove the following comments to initialize the data structure
    frontier = FIFOQueue([node])
    reached = {problem.initial}
    while frontier:
        node = frontier.pop()
        for child in expand(problem, node):
            s = child.state
            if problem.is_goal(s):
                return child
            if s not in reached:
                reached.add(s)
                frontier.appendleft(child)
    return failure
class RouteProblem(Problem):
    def actions(self, state): 
        return self.map.neighbors[state]
    def action_cost(self, s, action, s1):
        return self.map.distances[s, s1]
    def h(self, node):
        locs = self.map.locations
        return straight_line_distance(locs[node.state], locs[self.goal])
 class Map:
   def __init__(self, links, locations=None, directed=False):
        if not hasattr(links, 'items'): # Distances are 1 by default
            links = {link: 1 for link in links}
        if not directed:
            for (v1, v2) in list(links):
                links[v2, v1] = links[v1, v2]
        self.distances = links
        self.neighbors = multimap(links)
        self.locations = locations or defaultdict(lambda: (0, 0))    
def multimap(pairs) -> dict:
    result = defaultdict(list)
    for key, val in pairs:
        result[key].append(val)
    return result
Home_nearby_locations = Map(
    {('Kundrathur', 'Home'): 6, ('Kundrathur', 'Pammal'): 6,
     ('Home', 'Porur'): 4, ('Pammal', 'Airport'):5,
     ('Porur', 'Vadapalani'): 7, ('Porur', 'Maduravoyal'): 4, ('Porur', 'Guindy'): 10, ('Airport', 'Guindy'): 9,
     ('Vadapalani', 'T.Nagar'): 4, ('Vadapalani', 'Koyambedu'): 4, ('Maduravoyal', 'Koyambedu'): 5, ('Maduravoyal', 'Ambattur'): 6, ('Guindy', 'Saidapet'): 2,
     ('T.Nagar', 'EA Mall'): 5, ('Koyambedu', 'Korattur'): 6, ('Ambattur', 'Madhavaram'): 13, ('Saidapet', 'T.Nagar'): 4,
     ('EA Mall', 'Broadway'): 5, ('Korattur', 'Mahavaram'): 10, ('Madhavaram', 'Manali'): 11,
     ('Broadway', 'Tondiarpet'): 5, ('Broadway', 'Perambur'): 9, ('Manali', 'Tiruvottiyur'): 7,
     ('Tondiarpet', 'Tiruvottiyur'): 6, ('Perambur', 'Madhavaram'): 5})
r0 = RouteProblem('Home', 'Tiruvottiyur', map=Home_nearby_locations)
goal_state_path_0=breadth_first_search(r0)
print("GoalStateWithPath:{0}".format(goal_state_path_0))
print("Total Distance={0} Kilometers".format(goal_state_path_0.path_cost))
print("Route:{0}".format(path_states(goal_state_path_0)))
```
## OUTPUT:
![bfs](https://user-images.githubusercontent.com/75234646/175819112-ef5ceba7-32a2-4291-8e3f-70c545fa3b18.PNG)
## SOLUTION JUSTIFICATION:
The Route solutions are found by Breadth First Search algorithm(following FIFO and routes travelling from left to right).
## RESULT:
Thus,an algorithm developed to find the route from the source to the destination point using breadth-first search.
