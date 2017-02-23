## First Interview

*Reminder: Grade your partner throughout the interview*
#### Personal Pitch
- Tell me about your background

#### Behavioral
- Tell me about two improvements you've made in the last 6 months.
- Tell me about a time you failed and how you handled it.

#### Technical Knowledge
Explain the Virtual DOM, and give a pragmatic overview of how React renders it to the DOM.

##### Answer:
The Virtual DOM is an interesting concept; it’s a complex idea that boils down into a much simpler algorithm.

In React, if we create simple ES6 class and print it out, we have a function (as all functions can be used as a constructor in JavaScript):

```javascript
const app = () => {
    let React = react,
        {Component} = React,
        DOM = reactDom

    class Comments extends Component {
        constructor(props){ super(props) }
        render(){ return <div>test</div> }
    }

    console.log(Comments)
}

require('react', 'react-dom').then(app)
```
The console.log(Comments) gives us something that looks like this (after compiled by Babel from ES6 to ES5):

```javascript
function Comments(props) {
    _classCallCheck(this, Comments);

    return _possibleConstructorReturn(this, Object.getPrototypeOf(Comments).call(this, props));
}
```

When we write something to draw a React Component to the screen, we might have something like the following:

DOM.render(<Comments />, document.body)
The JSX gets transpiled into ES5 by Babel as well:

DOM.render(React.createElement(Comments, null), document.body);
We can see that <Comments /> is transpiled directly into React.createElement(Comments, null). This is where we can see what a Virtual DOM object actually is: a plain JavaScript Object that represents the tag to be rendered onto the screen.

Let’s inspect the output of React.createElement():
```javascript
console.log(<div/>)
// or
console.log(React.createElement('div', null))
```
This gives us:
```javascript
{"type":"div","key":null,"ref":null,"props":{},"_owner":null,"_store":{}}
```

See how the type is a string? DOM.render({...}) gets this object above and looks at the type, and decides whether or not to reuse an existing <div> element on the DOM or create a new <div> and append it.

The Virtual DOM is not a simple Object – it is a recursive structure. For example, if we add two elements beneath the <div/>:
```javascript
console.log(<div><span/><button/></div>)
// or
console.log(React.createElement(
    'div',
    null,
    React.createElement('span', null),
    React.createElement('button', null)
))
```
What we get is a nested Object-tree:
```javascript
{
    "type":"div",
    "key":null,
    "ref":null,
    "props":{
        "children": [
            {"type":"span","key":null,"ref":null,"props":{}},
            {"type":"button","key":null,"ref":null,"props":{}}
        ]
    }
}
```
This is why, in a React Component’s code, we can access the child and ancestor elements via this.props.children. What React will do is walk down a very deep tree of nested Objects (depending on your UI complexity), each sitting in their parent element’s children.

One thing to note is that the type so far has just been a string. When a React Element is made from a custom Component (like Comments above), the type is a function:
```javascript
console.log(<Comments />)
// or
console.log(React.createElement(Comments, null))
```
gives us:
```javascript
{
    "key":null,
    "ref":null,
    "props":{},
    “type”: function Component() { ... }
}
```
You can play around with a web version of this code at [Matthew Keas’ github](https://goo.gl/HZZMjv).

Source:
https://www.toptal.com/react/interview-questions

More resources:
[Difference Between Virtual DOM and DOM](http://reactkungfu.com/2015/10/the-difference-between-virtual-dom-and-dom/)
#### Technical Whiteboarding
Given a text file and a word, find the positions that the word occurs in the file. We'll be asked to find the positions of many words in the same file.

Example (don't give unless they ask clarifying questions):  
example.txt:  
hi  
hello  
hey  
hi  

solution_function(example.txt, "hi") => [0, 3]

Solution:

First take a quick read through:
 http://www.ardendertat.com/2011/12/20/programming-interview-questions-23-find-word-positions-in-text/


Ruby version of optimal solution
```ruby
class Node
  attr_accessor :positions
  def initialize(letter)
    @letter = letter
    @isTerminal = False
    @children = {}
    @positions = []
  end
end

class Trie
  def initialize
    @root = new Node("")
  end

  def contains?(word)
    current = @root
    word.each_char do |letter|
      return false unless current.children.include?(letter)
      current = current.children[letter]
    end
    current.isTerminal
  end


  def [](word)
    current = @root
    word.each_char do |letter|
      return false unless current.children.include?(letter)
      current = current.children[letter]
    end
    current.isTerminal = true
    current.positions
  end
end

# take in the text
def getWords(text)
  # method that parses the text file and returns
  # an array of the words all turned into lowercase
  # something along the lines of text.split(" ")

  # we're using this method for simplicity's sake, but it's not optimal space complexity.
  # it could be more optimal to read directly into the file and iterate over the file directly instead
end

# create_index takes the words and builds up a trie
def create_index(text)
  trie = new Trie
  words = getWords(text)
  words.each_with_index do |word, i|
    trie[word].positions << i  
  end
  trie
end

# method that takes in the trie index and specific word and returns an array of the indices
def queryIndex(index, word)
  if index.contains?(word)
    return index[word]
  else
    return []
  end
end

```


Additional Resources on Tries:
- [HackerRank Video](https://www.youtube.com/watch?v=zIjfhVPRZCg)
- [Wikipedia](https://en.wikipedia.org/wiki/Trie)
- [GeeksforGeeks](http://www.geeksforgeeks.org/trie-insert-and-search/)
