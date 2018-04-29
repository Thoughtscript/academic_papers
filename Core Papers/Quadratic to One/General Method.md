# A General Method to Reduce Quadratic Time Complexity to Constant Time

This short paper outlines a simple *polynomial time reduction* procedure to dramatically reduce time-complexity from quadratic time to constant time for a class of problems.

It is the hope of this author that such a mechanism can be extroplated and generalized outward.

# Time Complexity

The *complexity* of an algorithm is determined by the following: given some input *n*, the number of times *n* is processed, iterated over, or executed determines the complexity.

**Linear complexity** - A single loop through *n* gives a complexity of *n*. Two, non-nested, loops through *n* gives rise to *2n*. Such are scenarios of **linear complexity** whereby change in complexity is constant despite increases in input size or number of executions.

**Logorithmic complexity** - A loop through a *fully balanced binary search tree* gives rise to a **logorithmic complexity** scenario whereby the time complexity of the algorithm at first increases dramatically but then increases in a diminishing manner. Thus, the *growth in time complexity* **decreases** as the iterations cycle or the method continues.

Consider: 1, 2, 4, 8, 16, ... nodes that need to be visited.  

**Exponential or Quadratic complexity** - A loop through *n* with a second, nested, loop through *n* gives rise to *n^2*, etc. These are **exponentially** increasing *complexity* scenarios whereby the *growth in time complexity* **increases** over time.

### Formalized

Best formal definition:

(1)	Where *f*, *g* are functions from **N** to the positive reals, **R+**; and  
(2) Where *O(g)* is a *set* of functions from **N** to the positive reals, **R+**; and  
(3) Where *el* stands for the *elemental inclusion* operator:  

(a) *f* *el* *O(g)* means: asymptotically and ignoring constant factors, *f* does not grow more quickly than *g* over time or input size.  
(b) *f* *el* *O(g)* means: asymptotically and ignoring constant factors, *g* is the worst case time complexity for *f* over time or input size.   

See: http://www.uni-forst.gwdg.de/~wkurth/cb/html/cs1_v06.pdf

# Main Aim of This Paper

The main aim of this paper to specify a general procedure, working through examples, and identifying a class of suitable applcations of said procedure to reduce the time complexity from quadratic to one.

### Quadratic Time

```javascript
function print(str) {
  var el = document.getElementById("out");
  el.appendChild(document.createTextNode(str));
  el.appendChild(document.createElement("br"));
}
```

```javascript
function quadratic(inputArr) {
    var output_arr = [];
    for (var i = 0; i < inputArr.length; i++) {
      var temp = i == 0 ? inputArr[1] : inputArr[0];
      for (var j = 0; j < inputArr.length; j++)
        if (i != j) temp = inputArr[j] * temp;
      output_arr.push(temp);
    }
    return output_arr;
  }

  print(quadratic([0, 1, 2, 3]));
  print(quadratic([1, 1, 2, 5]));
```

### Constant Time

Here we pre-generate (AOT) the necessary data into static arrays. Thus, for each subsequent run through the algorithm is constant.

```javascript
function one(inputArr) {
    var a =
      inputArr[answerArray[0][0]] *
      inputArr[answerArray[0][1]] *
      inputArr[answerArray[0][2]];
    var b =
      inputArr[answerArray[1][0]] *
      inputArr[answerArray[1][1]] *
      inputArr[answerArray[1][2]];
    var c =
      inputArr[answerArray[2][0]] *
      inputArr[answerArray[2][1]] *
      inputArr[answerArray[2][2]];
    var d =
      inputArr[answerArray[3][0]] *
      inputArr[answerArray[3][1]] *
      inputArr[answerArray[3][2]];

    return [a, b, c, d];
  }

  print(one([0, 1, 2, 3]));
  print(one([1, 1, 2, 5]));
```

### Formal Properties of the Class



# Conclusion