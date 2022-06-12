# sudoku
## Aim:
To develop a code to solve a given sudoku puzzle.

## Theory:
We create apython program to solve a given sudoku puzzle by using Elimination and Only Choice strategies.
Firstly, we form the puzzle itself, either by getting data in String form or by indexes and values, from the user.
Then we fill the empty boxes with all the possible values, 1-9. We iteratively apply Elimination and Only Choice Strategies to finally end up with a result.

## Design Steps
## Step 1:
We define the puzzle. We initialize those boxes with only one value, with the data provided by the user, either in String format or in the Index & value format.

## Step 2:
We identify the Row units, Column units, and the square units. And then we form a unit list using the prior data.

## Step 3:
Two Dictionaries with units and peers of each boxes is defined.

## Step 4:
We reduce the puzzle using the two strategies.

## Step 5:
Search function is defined to find the final solution to a given sudoku puzzle.
```
## Program:
## Developed By : VEERAMALAI S
## Register Number : 212220230056
```
```python3
%matplotlib inline
import matplotlib.pyplot as plt
import random
import math
import sys
import time
rows = 'ABCDEFGHI'
cols = '123456789'

def cross(a,b):
    return [i+j for i in a for j in b]
boxes=cross(rows,cols)
ru=[cross(r,cols) for r in rows]
cu=[cross(rows,c) for c in cols]
su=[cross(rs,cs) for rs in ('ABC','DEF','GHI') for cs in('123','456','789')]
ul=ru+cu+su
units = dict((s, [u for u in ul if s in u])  for s in boxes)
peers = dict((s, set(sum(units[s],[]))-set([s]))for s in boxes)
def grid_values(grid):
    assert len(grid) == 81, "Input grid must be a string length of 81 (9x9)"
    boxes = cross(rows,cols)
    return dict(zip(boxes,grid))
def display(values):
    width = 1+max(len(values[s]) for s in boxes)
    line = '+'.join(['-'*(width*3)]*3)
    for r in rows:
        print(''.join(values[r+c].center (width)+('|' if c in '36' else '') for c in cols))
        if r in 'CF': print(line)
    return
def grid_values_improved(grid):
    values = []
    all_digits = '123456789'
    for c in grid:
        if c == '.':
            values.append(all_digits)
        elif c in all_digits:
                values.append(c)
    assert len(values) == 81
    boxes = cross(rows,cols)
    return dict(zip(boxes,values))    
def elimination(values):
    solved_values = [box for box in values.keys() if len(values[box])==1]
    for box in solved_values:
        digit = values[box]
        for peer in peers[box]:
            values[peer] = values[peer].replace(digit,'')
    return values
def only_choice(values):
    for unit in ul:
        for digit in '123456789':
            dplaces = [box for box in unit if digit in values[box]]
            if len(dplaces) == 1:
                values[dplaces[0]] = digit
    return values    
def reduce_puzzle(values):
    stalled =False
    while not stalled:
        solved_values_before = len([box for box in values.keys() if len(values[box])==1])
        elimination(values)
        only_choice(values)
        solved_values_after = len([box for box in values.keys() if len(values[box])==1])
        stalled = solved_values_after == solved_values_before
        if len([box for box in values.keys() if len(values[box])==1])==0:
            return False
    return values    
def search(values):
    values_reduced = reduce_puzzle(values)
    if not values_reduced:
        return False
    else:
        values=values_reduced
    if len([box for box in values.keys() if len(values[box])==1])==81:
        return values   
    possibility_count_list = [(len(values[box]),box) for box in values.keys() if len(values[box])>1]    
    possibility_count_list.sort()
    for(_,t_box_min) in possibility_count_list:
        for i_digit in values[t_box_min]:
            new_values = values.copy()
            new_values[t_box_min]=i_digit
            new_values = search(new_values)
            if new_values:
                return new_values           
    return values
def search(values):
    values_reduced = reduce_puzzle(values)
    if not values_reduced:
        return False
    else:
        values=values_reduced
    if len([box for box in values.keys() if len(values[box])==1])==81:
        return values  
    possibility_count_list = [(len(values[box]),box) for box in values.keys() if len(values[box])>1]
    possibility_count_list.sort()
    for(_,t_box_min) in possibility_count_list:
        for i_digit in values[t_box_min]:
            new_values = values.copy()
            new_values[t_box_min]=i_digit
            new_values = search(new_values)
            if new_values:
                return new_values
            
    return values
    
    p='.38...24.6..9.2..1..4..8..64712..35............5....1..83...67...6..1529..2....8.'
start_time = time.time()
display(grid_values(p))
p1=grid_values_improved(p)
print("\n\n")
display(p1)
result = search(p1)
print("\n\n")
display(result)
time_taken=time.time() - start_time
print("\n\n{0} seconds".format(time_taken))
```
## Output:
![sudoku](https://user-images.githubusercontent.com/75235601/173197037-8dcca72a-5082-44d0-93b8-95ee9046cd53.jpg)
## Result:
Thus, a program to solve sudoku puzzle using constraint propagation is implemented successfully.
