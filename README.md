lab2
====

As a reminder, you have the [gallery](https://github.com/mbostock/d3/wiki/Gallery), [tutorials](https://github.com/mbostock/d3/wiki/Tutorials), the [API](https://github.com/mbostock/d3/wiki/API-Reference) and the [source code](https://github.com/mbostock/d3) to find more happiness with D3.

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

* As you can see, it is an "empty" document with no CSS code or external file for style.
* You can load D3 from a local script.

However, even if empty, this document allows us to do a lot of things:

* Open the Chrome developper console.
* Look at the D3 object and explore its attribute and functions.
* Examples: 
   * Which D3 version is? `d3.version`.
   * Execute functions such as `d3.ascending(1, 2)` and `d3.max([1, 3, 4])`.
   * Look at the [source code](https://github.com/mbostock/d3/blob/master/d3.js) for a more readable code.
   * Explore other fields coming with [JavaScript objects](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/prototype).
 * Once loaded, D3 overloads some Javascript data structures
 * Example: `[1, 2, 3].map(function(d, i) {return [d, i];})`, with accessor function `d3.max([[1,2], [2, 9], [3, 4]], function(d) { return d[0];})` (e.g. if you have object or arrays)
 * Many, many helpers function: `d3.interpolateString(1, 2)`, etc..
 
### Selection and Selections

Selections are the most important concepts in D3.

* D3 returns DOM nodes selection (in [transerveral order](http://en.wikipedia.org/wiki/Tree_traversal)) as an array, using CSS to query.
* Example: `d3.select("body")`
* To access the node: `d3.select("body")[0][0]` equivalent to `document.body`
* OR reference to a node `d3.select(document.links)`
* You can then apply operators on them (styles, attributes, properties)
* Ex: '(“element”), (“.class”), (“#id”), (“parent child”)'
* Example of CSS selections: d3.select(“body”).append("div").style("width", 500).attr("height", 200).style(“color”, “red”)

### Joining Data to selections

* The most difficult concept with D3:

```
d3.select("body").selectAll("p").data([1,2,3])
```

* `d3.select("body").selectAll("p")` is an empty array.
* `d3.select("body").selectAll("p").data([1,2,3])` is an array of empty elements.
* Proof `d3.select("body").selectAll("p").data([1,2,3])[0].length`
* Remember the `[1, 2, 3].map()` example above, it actually does the same thing.
* We now attach nodes to the empty array elements
* What `.data()` does is that it binds/joins data and returns the update
  * `exit()`
  * `enter()`

Back to the update:
* We are going to bind data and call the update function and add elements.
* `d3.select("body").selectAll("p").data([1,2,3]).append("p").text(function(d) {return d;})`

* It creates placeholders are empty elements created to contain future elements
  * They are visible in the DOM inspector

Let's bind data to existing elements. Here are a series of paragraphs:

```html
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
```

```javascript
    var data = [10, 20, 30 , 40, 50];

    d3.select("body").selectAll("p")
        .data(data)
        .transition().delay(function(d, i) { return 500*i; })
        .text(function(d) { return d});
```

Where are the data?
* Warning: this is done for the purpose of teaching!
* `d3.select("p").datum()` returns the data of a single element
* `d3.select("p").data()` returns an array of elements
* `d3.select("p").node().__data__`
* `d3.select("p").property("__data__")`
* Let's try to change `d3.selectAll("p").node().__data__ = 5`
* You need to update `d3.select("body").selectAll("p").text(function(d) {return d;})`
* But you should stick to the `.data()` function, and accessor function to retrieve data

Adding a transition on the update

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
        .text(function(d) { return d});
```

## Divs

* The code below binds data to existing DOM object
*

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


A little flashback now:

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
    .text(function(d) { return d.name;})
```

## Table

```html
<table border=1>
<tbody>
	<tr>
		<td>(1, 1)</td><td>(1, 2)</td>
	</tr><tr>
		<td>(2, 1)</td><td>(2, 2)</td>
	<tr>
</tbody>
```

Let's do some SVG, collection of elements you can manipulate.
Now map the data to existing elements and update them

## More Exercices

* Generate datasets 
  * 'var alphabet = "abcdefghijklmnopqrstuvwxyz".split("");'
  * `d3.range(20)`
  * Math.random()

* Update the previous examples and add scales to make them fit on the screen


