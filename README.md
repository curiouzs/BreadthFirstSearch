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
#### Example map
![ alt text for screen readers](./images/map1.jpg "Map around my house")

## PROGRAM
```python #DEVELOPED BY: M.LOKESH KRISHNAA 
#REGISTER NO : 212220230030
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
   saveetha_nearby_locations = Map(
    {('Poonamallee', 'SeenerKupam'): 3,
    ('Poonamallee', 'Karanchavadi'): 3,
    ('SeenerKupam', 'paruthipattu'): 3,  
    ('paruthipattu','Avadi'): 5,
    ('Avadi','Ambattur'): 2,
    ('Ambattur', 'Ambattur Estate'): 1,
    ('Ambattur Estate', 'Anna nagar west'): 2, 
    ('Ambattur Estate', 'Maduravoyal'):  5, 
    ('Maduravoyal', 'Koyambedu'): 5, 
    ('Koyambedu', 'CMBT'): 1,
    ('Koyambedu', 'Chetpet'): 6,
    ('CMBT', 'Vadapalani'): 5,  
    ('Kattupakkam', 'Porur link road'): 4, 
    ('Porur link road', 'Porur'): 1,
    ('Porur', 'Vadapalani'): 3,
    ('Porur link road', 'Maduravoil'): 2, 
    ('Porur', 'Vadapalani'): 4,
    ('Vadapalani', 'T-Nagar'): 4,
    ('Vadapalani', 'Guindy'): 6,
    ('Vadapalani', 'Nungabakkam'): 4,
    ('Nungabakkam','Chetpet'): 5,
    ('T-Nagar', 'Guindy'): 7, 
    ('Porur', 'Gundy'): 10, 
    ('Gundy', 'ChennaiAirport'): 5})

r0 = RouteProblem('chennai', 'arakkonam', map=chennai_to_arakkonam)
goal_state_path=breadth_first_search(r0)
print("GoalStateWithPath:{0}".format(goal_state_path))
path_states(goal_state_path) 
print("Total Distance={0} Kilometers".format(goal_state_path.path_cost))

r1 = RouteProblem('chennai', 'thiruvallur', map=chennai_to_arakkonam)
goal_state_path=breadth_first_search(r1)
print("GoalStateWithPath:{0}".format(goal_state_path))
path_states(goal_state_path) 
print("Total Distance={0} Kilometers".format(goal_state_path.path_cost))

r2 = RouteProblem('poonamalle', 'arakkonam', map=chennai_to_arakkonam)
goal_state_path=breadth_first_search(r2)
print("GoalStateWithPath:{0}".format(goal_state_path))
path_states(goal_state_path) 
print("Total Distance={0} Kilometers".format(goal_state_path.path_cost))

r3 = RouteProblem('arakkonam', 'ambattur', map=chennai_to_arakkonam)
goal_state_path=breadth_first_search(r3)
print("GoalStateWithPath:{0}".format(goal_state_path))
path_states(goal_state_path) 
print("Total Distance={0} Kilometers".format(goal_state_path.path_cost))

r4 = RouteProblem('perambakkam', 'avadi', map=chennai_to_arakkonam)
goal_state_path=breadth_first_search(r4)
print("GoalStateWithPath:{0}".format(goal_state_path))
path_states(goal_state_path) 
print("Total Distance={0} Kilometers".format(goal_state_path.path_cost))
```
## OUTPUT:
## SOLUTION JUSTIFICATION:
The Route solutions are found by Breadth First Search algorithm(following FIFO and routes travelling from left to right).
## RESULT:
Thus,an algorithm developed to find the route from the source to the destination point using breadth-first search.
