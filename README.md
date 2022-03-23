# Newick Tree Parser

> JavaScript library for parsing the Newick Tree format.
 
## Usage:

```js
import { Newick } from 'newick-tree-parser'
const newick = new Newick('some data')
```

## Documentation:

### NewickTools.dfs
Returns nodes data
```js
import { NewickTools } from 'newick'
NewickTools.dfs(tree)
```
tree - string or parsed object
```js
var tree = NewickTools.parse('(A:0.1,B:0.2,(C:0.3,D:0.4)E:0.5)F');
var data = NewickTools.dfs(tree);
```
Returns object
```json
{
  A: 0.1,
  B: 0.2,
  C: 0.3,
  D: 0.4,
  E: 0.5
}
```
---
```js
NewickTools.dfs(tree, nodeCallback)
```
- tree - string or parsed object
- nodeCallback - function (node)
```js
var tree = NewickTools.parse('(A:0.1,B:0.2,(C:0.3,D:0.4)E:0.5)F');
var data = NewickTools.dfs(tree, function (e) {
  e.length *= 10;
  return e;
});
```
Returns object
```json
{
  A: 1,
  B: 2,
  C: 3,
  D: 4,
  E: 5
}
```

### NewickTools.equals
Check if two trees are equal
```js
import { NewickTools } from 'newick'

NewickTools.equals(tree1, tree2)
```
tree1, tree2 - Newick instanse
Returns boolean true or false
```js
var tree1 = NewickTools.parse('(A:0.1,B:0.2,(C:0.3,D:0.4)E:0.5)F');
var tree2 = NewickTools.parse('(A:0.1,B:0.2,(C:0.3,D:0.4)E:0.5)F');

NewickTools.equals(tree1, tree2); // true;

tree2 = NewickTools.parse('(A:0.11,B:0.22,(C:0.33,D:0.44)E:0.55)F');

NewickTools.equals(tree1, tree2); // false
```

### NewickTools.getRoot
Returns a root of the tree
```js
import { NewickTools } from 'newick'

NewickTools.getRoot(s)
```
s - string on parsed object
```js
var tree = NewickTools.parse('(A:0.1,B:0.2,(C:0.3,D:0.4)E:0.5)F');
var root = NewickTools.getRoot(tree);\
```
Returns string (root name)
```
'F'
```

### NewickTools.map
Iterates tree's nodes and applies callback for each node

Alias for NewickTools.dfs(tree, callback)
```js
import { NewickTools } from 'newick'

NewickTools.map(tree, callback)
```
tree - string or parsed object
callback - function (node)
```js
var tree = NewickTools.parse('(A:0.1,B:0.2,(C:0.3,D:0.4)E:0.5)F');
var mappedTree = NewickTools.map(tree, function (e) {
  e *= 10;
  e.name += ' mapped';
  return e;
});
```
Returns object
```js
{
    name: "F mapped",
    branchset: [
    {
        name: "A mapped",
        length: 1
    },
    {
        name: "B mapped",
        length: 2
    },
    {
        name: "E mapped",
        length: 5,
        branchset: [
        {
            name: "C mapped",
            length: 3
        },
        {
            name: "D mapped",
            length: 4
        }]
    }]
}
```

### NewickTools.normalize

Returns normalized tree in [0; 1]
```js
import { NewickTools } from 'newick'

NewickTools.normalize(tree)
```
tree - string or parsed object
```js
var tree = NewickTools.parse('(A:5,B:20)F;');
var normalizedTree = NewickTools.normalize(tree)
```
Returns object
```json
{
  name:"F",
  branchset: [
    {name:"A", length: 0.2},
    {name:"B", length: 0.8}
  ]
}
```

### NewickTools.parse
Parse Newick string into tree-object
```js
import { NewickTools } from 'newick'

NewickTools.parse(s)
```
s - string
```js
var tree = NewickTools.parse('(A:0.1,B:0.2,(C:0.3,D:0.4)E:0.5)F');
```
Returns object
```js
{
    name: "F",
    branchset: [
    {
        name: "A",
        length: 0.1
    },
    {
        name: "B",
        length: 0.2
    },
    {
        name: "E",
        length: 0.5,
        branchset: [
        {
            name: "C",
            length: 0.3
        },
        {
            name: "D",
            length: 0.4
        }]
    }]
}
```

### NewickTools.serialize
Serializes tree
```js
import { NewickTools } from 'newick'

NewickTools.serialize(tree)
```
tree - string or parsed object
```js
var tree = NewickTools.parse('(A:0.1,B:0.2,(C:0.3,D:0.4)E:0.5)F');
var serializedTree = NewickTools.serialize(tree)
```
Returns string
```js
'(A:0.1,B:0.2,(C:0.3,D:0.4)E:0.5)F;'
```
### private.cast
Casts tree or string to tree-object

private
```js
cast(s)
```
s - string or parsed object
```js
var tree = '(A:0.1,B:0.2,(C:0.3,D:0.4)E:0.5)F';
tree = cast(tree);
```
Returns object
```js
{
    name: "F",
    branchset: [
    {
        name: "A",
        length: 0.1
    },
    {
        name: "B",
        length: 0.2
    },
    {
        name: "E",
        length: 0.5,
        branchset: [
        {
            name: "C",
            length: 0.3
        },
        {
            name: "D",
            length: 0.4
        }]
    }]
}
```

### private.getRoot
Returns a root of the tree

private
```js
getRoot(s)
```
s - string or parsed object
```js
var tree = Newick.parse('(A:0.1,B:0.2,(C:0.3,D:0.4)E:0.5)F');
tree = getRoot(tree);
```
Returns string (root name)
```js
'F'
```