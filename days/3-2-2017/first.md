## First Interview

*Reminder: Grade your partner throughout the interview*
#### Personal Pitch
Why do you want to work as a software engineer here?

Some things to think about here:
In essence, this is a personal pitch question, but for something framed this way, you especially want to focus less on the first two parts of:
1. Why you're interested tech.
2. How you came to become a software engineer.

And instead focus much more on the second two parts of:
3. Your technical skills/accomplishments.
4. How you can contribute to their team/what specifically makes you want to work there.
#### Behavioral
1. Tell me about a time you failed and how you handled it.
2. What role do you usually play in a team?

#### Technical Knowledge
Tell me about the MVC architecture.


##### Answer:
https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller

#### Technical Whiteboarding
Imagine a robot sitting on the upper left corner of a grid with `r` rows and `c` columns. The robot can only move in two directions, right and down, but certain cells are "off limits" such that the robot cannot step on them. Design an algorithm to find a path for the robot from the top left to the bottom right.

Clarifications (wait for interviewee to ask clarifying questions before providing):
1. Assume that the grid is made up of a nested array -where each element is either true or false. If it's false, that means it is an "off limit" cell.
2. Input is the maze, which is a nested array, output is the path, which is an array of subarrays that indicates a path to travel.

##### Answer (from CTCI Problem 8.2):

**Naive solution:**
If we picture this grid, the only way to move to the bottom right spot (r,c) of the grid is by moving to one of the adjacent spots: (r - 1, c) or (r, c - 1). So, we need to find a path to either (r - 1, c) or (r, c - 1).

To find a path to those adjacent spots, we need to move to one of its adjacent cellls. So, we need to find a spot adjacent to (r - 1, c), which are coordinates (r - 2, c) and (r - 1, c - 1), or a spot adjacent to (r, c - 1), which are spots (r - 1, c - 1) and (r, c - 2).

So then, to find a path from the origin, we just work backwards like this. Starting from the last cell (r, c), we try to find a path to each of its adjacent cells. The recursive code below implements this algorithm.
```ruby
def get_path(maze) # maze is a nested array
  return nil if maze == nil || maze.lenth == 0

  path = []

  if get_path_helper(maze, maze.length - 1, maze[0].length - 1, path)
    return path
  end

  return nil # if there is no working path then return nil
end

def get_path_helper(maze, row, col, path)
  # if out of bounds or "off limits" return false.
  return false if col < 0 || row < 0 || !maze[row][col]

  is_at_origin = row == 0 && col == 0

  if is_at_origin || get_path_helper(maze, row, col - 1, path) || get_path_helper(maze, row - 1, col,path)
    point = [row, col]
    path.push(point)
    return true
  end

  false
end
```
Time complexity of the above solution is O(2^(r+c)), since each path has r + c steps and there are two choices we can make at each step.

**Better solution that uses dynamic programming:**
Often, we can optimize exponential algorithms by finding duplicate work. What are we repeating?

If we walk through the algorithm, we'll see that we are visiting squares multiple times. In fact, we visit each square many, many times. After all, we have `rc` squares but we're doing O(2^(r+c)) work. If we were ony visiting each square once, we would probably have an algorithm that was O(rc)(unless we were somehow doing a lot of work during each visit).

How does our current algorithm work? To find a path to (r, c), we look for a path to an adjacent coordinate: (r - 1, c) or (r, c - 1). Of course, if one of those squares is off limits, we ignore it. Then we look at their adjacent coordinates: (r - 2, c), (r - 1 , c - 1), (r - 1, c - 1), and (r, c - 2). The spot (r - 1, c - 1) appears twice, which means that we're duplicating effort. Ideally, we should remember that we already visited (r - 1, c - 1) so that we don't waste time.

```ruby
def get_path(maze)
  return nil if maze == nil || maze.length == 0

  path = []

  failed_points = Hash.new

  if get_path_helper(maze, maze.length - 1, maze[0].length - 2, path, failed_points)
    return path
  end

  return nil

end

def get_path_helper(maze, row, col, path, failed_points)
  # If out of bounds or not available, return false
  return false if col < 0 || row < 0 || !maze[row][col]

  point = [row, col]

  # If we've already visited this cell, return false
  if failed_points[point]
    return false
  end

  is_at_origin = row == 0 || col == 0

  if is_at_origin || get_path_helper(maze, row, col - 1, path, failed_points) || get_path_helper(maze, row - 1, col, path, failed_points)
    path.push(point)
    return true
  end

  failed_points[point] = true # cache the result in our hash
  false
end
```
Time complexity: The algorithm will now take O(rc) time because we hit each cell just once.
