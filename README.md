Lab2
====

As a reminder, you have the [gallery](https://github.com/mbostock/d3/wiki/Gallery), [tutorials](https://github.com/mbostock/d3/wiki/Tutorials), the [API](https://github.com/mbostock/d3/wiki/API-Reference), the [research article](http://vis.stanford.edu/files/2011-D3-InfoVis.pdf) and the [source code](https://github.com/mbostock/d3) to find even more happiness with D3.

## Basic HTML document

Below is the `HTML` boilerplate for the rest of the examples during this lab.

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset=utf-8 />
  <title>CS 171 Lab 2</title>
  <style type="text/css">
  
  </style>
  <script src="http://d3js.org/d3.v3.min.js" type="text/javascript" charset="utf-8"></script>
</head>
<body>
  <script type="text/javascript">

  </script>
</body>
</html>
```

* As you can see, it is a quasi-empty document with no CSS code or external file for style.
* D3 is loaded from a remote server, but you can load D3 from a local file system.

Surprisingly, even if empty, this document allows us to do a lot of fun things. To get started, load the document and open the Chrome developer console.
* Look at the D3 object (type `d3` in the console) and explore its attribute and functions.
* Examples: 
  * Which D3 version is? `d3.version`.
  * Look at the code structure
  * Look at the [source code](https://github.com/mbostock/d3/blob/master/d3.js) for a more readable code.
  * Execute functions such as `d3.ascending(1, 2)`, `d3.max([1, 3, 4])`, `d3.interpolate(1, 10)`.
  * Notice default functions coming with [JavaScript objects](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/prototype).

### Selection and Selections

Selections are the most important concepts in D3.

* D3 returns DOM nodes selection (in [transerveral order](http://en.wikipedia.org/wiki/Tree_traversal)) as an "array" (see this [article](http://bost.ocks.org/mike/selection/#subclass) for more details), using CSS on an object as query.
* Query using CSS `d3.select("body")`
* To access the node: `d3.select("body")[0][0]` equivalent to `document.body`
* Query using a reference to a node `d3.select(document.links)`
* You can then apply operators on them (styles, attributes, properties)
* Ex: '("element"), (".class"), ("#id"), ("parent child")'
* Add paragraphs and change their properties `d3.select("p").style("font-size", 40+"px")`
* Example of CSS selections: `d3.select("body").append("div").style("width", 500).attr("height", 200).style("color", “red")`
* SelectAll selects all elements: `d3.selectAll("p").style("color", "blue")`
* Selections work pretty much like arrays `[10, 20, 30].map(function(d, i) { return [d, i];})`, with accessor function `d3.max([[1,2], [2, 9], [3, 4]], function(d) { return d[0];})` (e.g. if you have object or arrays)
* Selections can be filtered with .filter().

### Joining Data to selections

* Data binding:

```
d3.select("body").selectAll("p").data([1,2,3])
```

* `d3.select("body").selectAll("p")` is an empty array.
* `d3.select("body").selectAll("p").data([1,2,3])` is an array of empty elements.
* Proof `d3.select("body").selectAll("p").data([1,2,3])[0].length`.
* Similar to `[1, 2, 3].map()`.
* We now attach nodes to the empty array elements with `data()`.
* What `.data()` does is that it binds/joins data and returns the update.
  * `exit()` returns a selection of non-binded elements
  * `enter()` returns a selection of newly binded elements
* Advanced reading: corresponding [D3 source](https://github.com/mbostock/d3/blob/master/d3.js#L760)

Back to the update:
* We are going to bind data and call the update function and add elements.
* `d3.select("body").selectAll("p").data([1,2,3]).enter().append("p").text(function(d) {return d;})`.
* It created placeholders of empty elements, then added the `p` elements.
* They are visible in the DOM inspector.

Where are the data?
* (Warning: this is done for the purpose of understanding how it works..)
* `d3.select("p").datum()` returns the data for a single element.
* `d3.select("p").data()` returns an array of elements.
* `d3.select("p").node().__data__`.
* `d3.select("p").property("__data__")`.
* Let's try to change `d3.selectAll("p").node().__data__ = 5` but not recommended.
* Better way `d3.select("body").selectAll("p").text(function(d) { return d;})` by sticking to the `.data()` function, and accessor function to retrieve data.

## Let's add some code (finally!)

Here are a series of paragraphs:

```html
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
```

Let's bind data to existing elements, with a transition:

```javascript
    var data = [1, 2, 3, 4, 5];

    d3.select("body").selectAll("p")
        .data(data)
        .transition().delay(function(d, i) { return 500*i; })
        .text(function(d) { return d});
```

## Paragraphs

* The code below creates paragraphs and fill them with the data as text.
* Elements are progressively updates according to their position in the DOM

```javascript
    var data = [10, 20, 30 , 40, 50];

    d3.select("body").selectAll("p")
        .data(data)
        .enter()
        .append("p")
        .transition().delay(function(d, i) { return 500*i; })
        .text(function(d) { return d; });
```

## Divs

* The code below binds data to existing DOM objects:

```css
  <style type="text/css">
    div {
        margin: 10px;
        font-size: 20px;
        color: #000;
        padding: 10px; 
        background-color:gold;
        font-family: sans-serif;
        border-style:solid;
        border-width:1px;
    }
  </style>
```

```html
  <div style="width: 0px;">Homer</div>
  <div style="width: 0px;">Marge</div>
  <div style="width: 0px;">Bart</div>
  <div style="width: 0px;">Lisa</div>
  <div style="width: 0px;">Maggie</div>
```

```javascript
  var dataset = [36, 34, 10, 8, 1]

  d3.selectAll("div")
    .data(dataset)
    .transition().duration(function(d, i) { return 1000*i;})
    .style("width", function(d) { return (10*d) + "px"; })
```

Let's do some conditionnal styling:

```javascript
  .style("background-color", function(d) {
    if (d.age > 15) { 
      return "red";
    }
  })
```

A little flashback now: what if we only have Homer and Marge?

```javascript
  var dataset = [36, 34];
```

* Only existing `<div>` are updated.
* You can play with the ones which are not binded using `d3.selectAll("div").data(dataset).exit()`.

```javascript
  var dataset = [{"name": "Homer", "age": 36}, {"name": "Marge", "age": 34}]

  d3.selectAll("div")
    .data(dataset)
    .transition().duration(function(d, i) { return 1000*i;})
    .style("width", function(d) { return (10*d.age) + "px"; })
    .text(function(d) { return d.name;})
```

* What happens if we have new family members?

```javascript
 var dataset = [{"name": "Homer", "age": 36}, {"name": "Marge", "age": 34}, {"name": "Bart", "age": 10}, {"name": "Lisa", "age": 10}, {"name": "Maggie", "age": 1}];

  d3.select("body")
    .selectAll("div")
    .data(dataset)
    .enter()
    .append("div")
    .style("width", "10px")
    .text(function(d) { return d.name;})

  d3.selectAll("div")
    .data(dataset)
    .transition().duration(function(d, i) { return 1000*i;})
    .style("width", function(d) { return (10*d.age) + "px"; })
```

## SVG

An SVG bar chart ([source](http://www.recursion.org/d3-for-mere-mortals/)).

```javascript
  var data = [{year: 2006, books: 54},
            {year: 2007, books: 43},
            {year: 2008, books: 41},
            {year: 2009, books: 44},
            {year: 2010, books: 35}];

  var barWidth = 40;
  var width = (barWidth + 10) * data.length;
  var height = 200;

  var x = d3.scale.linear().domain([0, data.length]).range([0, width]);
  var y = d3.scale.linear().domain([0, d3.max(data, function(d) { return d.books; })]).
    rangeRound([0, height]);

  // add the canvas to the DOM
  var barDemo = d3.select("body").
    append("svg:svg").
    attr("width", width).
    attr("height", height);

  barDemo.selectAll("rect").
    data(data).
    enter().
    append("svg:rect").
    attr("x", function(d, i) { return x(i); }).
    attr("y", function(d) { return height - y(d.books); }).
    attr("height", function(d) { return y(d.books); }).
    attr("width", barWidth).
    attr("fill", "#2d578b");

  barDemo.selectAll("text").
    data(data).
    enter().
    append("svg:text").
    attr("x", function(d, i) { return x(i) + barWidth; }).
    attr("y", function(d) { return height - y(d.books); }).
    attr("dx", -barWidth/2).
    attr("dy", "1.2em").
    attr("text-anchor", "middle").
    text(function(d) { return d.books;}).
    attr("fill", "white");
```

## Table

Look at this example (from [D3 wiki](https://github.com/mbostock/d3/wiki/Selections)).

```javascript
var matrix = [
  [11975,  5871, 8916, 2868],
  [ 1951, 10048, 2060, 6171],
  [ 8010, 16145, 8090, 8045],
  [ 1013,   990,  940, 6907]
];

var tr = d3.select("body").append("table").selectAll("tr")
    .data(matrix)
  .enter().append("tr");

var td = tr.selectAll("td")
    .data(function(d) { return d; })
  .enter().append("td")
    .text(function(d) { return d; });
```


## More Exercices

* Generate datasets using those function and make the previous example scalable!
  * 'var alphabet = "abcdefghijklmnopqrstuvwxyz".split("");'
  * `d3.range(20)`
  * Math.random()

* Bar chart example (from [D3 Examples](http://bl.ocks.org/mbostock/3885304))
* D3 [list of examples](https://github.com/mbostock/d3/wiki/Gallery)

