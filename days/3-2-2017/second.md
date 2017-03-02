## Second Interview

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
Explain AJAX.

##### Answer:
https://www.w3schools.com/xml/ajax_intro.asp

#### Technical Whiteboarding
Given a binary matrix, find out the maximum sizesquare sub-matrix with all 1s.

##### Answer:


For full explanation of solution, read here:
http://www.geeksforgeeks.org/maximum-size-sub-matrix-with-all-1s-in-a-binary-matrix/

```ruby
  def print_max_subsquare(binary_matrix)
    sum_matrix = [[]]

    # set the first column of the sum_matrix equal to
    # the first column of the binary_matrix
    binary_matrix.each do |row|
      sum_matrix[row][0] = binary_matrix[row][0]
    end

    # set the first row of the sum_matrix equal to the
    # first row of the binary_matrix
    binary_matrix[0].each do |col|
      sum_matrix[0][col] = binary_matrix[0][col]
    end

    # construct the other entries of the sum_matrix
    binary_matrix.each do |row|
      binary_matrix[0].each do |col|
        if binary_matrix[row][col] == 1
          sum_matrix = min(sum_matrix[row][col - 1], sum_matrix[row - 1][col], sum_matrix[row - 1][col - 1]) + 1
        else
          sum_matrix[row][col] = 0
        end
      end
    end
  end

  #IN PROGRESS
```
