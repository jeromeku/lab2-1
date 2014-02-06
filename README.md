lab2
====

As a reminder, you have the [gallery](https://github.com/mbostock/d3/wiki/Gallery), [tutorials](https://github.com/mbostock/d3/wiki/Tutorials), the [API](https://github.com/mbostock/d3/wiki/API-Reference) and the [source code](https://github.com/mbostock/d3).

## Empty template

Below is the `HTML` boilerplate for the rest of the examples during this lab.

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset=utf-8 />
  <title>CS 171 Lab 2</title>
  <script src="http://d3js.org/d3.v3.min.js" type="text/javascript" charset="utf-8"></script>
</head>
<body>
  <script type="text/javascript">
  
  
  </script>
</body>
</html>
```

Is example is meant to show what can already be done with an empty document:

 * Open the Chrome developper console
 * Look at the D3 object and explore its attribute and functions.
 * Example: functions, such as the first ones `d3.ascending(1, 2)` and `d3.max([1, 3, 4])`
 * D3 overloads Javascript data structures
 * Example: `[1, 2, 3].map(function(d, i) {return [d, i];})`, with accessor function `d3.max([[1,2], [2, 9], [3, 4]], function(d) { return d[0];})` (e.g. if you have object or arrays)
 * Many, many helpers function: `d3.interpolateString(1, 2)`, etc..
 
### Selection and Selections

Selections are the most important concepts in D3.

* D3 returns DOM nodes selection (in [transerveral order](http://en.wikipedia.org/wiki/Tree_traversal)) as an array, using CSS to query.
* Example: `d3.select("body")`
* To access the node: `d3.select("body")[0][0]` equivalent to `document.body`
* OR reference to a node `d3.select(document.links)`
* You can then apply operators on them (styles, attributes, properties)
* Example of CSS selections: ...

### Joining Data to selections

* **The most difficult D3 concept:** 

```d3.select("body").selectAll("p").data([1,2,3])```

* `d3.select("body").selectAll("p")` is an empty array
* `d3.select("body").selectAll("p").data([1,2,3])` is an array of empty elements!
* Proof `d3.select("body").selectAll("p").data([1,2,3])[0].length`
* Wat? We'll get back to it later.
* It makes more sens to bind to `d3.select("body").selectAll("p").data([1,2,3])[0].length` 
* Remember the `[1, 2, 3].map()` example above, it does the same thing!
* It creates placeholders are empty elements created to contain future elements
  * They are visible in the DOM inspector
It creates enter() and exit() subselections


```javascript
    var data = [10, 20, 30 , 40, 50];

    d3.select("body").selectAll("p")
        .data(data)
        .append("p")
        .transition().delay(function(d, i) { return 500*i; })
        .text(function(d) { return d});
```

* Where are the data??
* `d3.select("td").datum()` returns the data

* Examples with existing elements







```javascript
    var data = [10, 20, 30 , 40, 50];

    d3.select("body").selectAll("p")
        .data(data)
        .enter()
        .append("p")
        .transition().delay(function(d, i) { return 500*i; })
        .text(function(d) { return d});
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

## Visualizing Data!!

```html
<style type="text/css">
div{
    background-color: #dd9;
    margin: 5px;
    font-size: 25px;
    color: #007;
    padding: 10px;
    
}
</style>
</head>
<body>
  <div>Alex</div>
  <div>Hendrik</div>
  <div>David</div>
  <div>Romain</div>

  <script type="text/javascript">
      d3.selectAll("div")
          .data([100,300,200,500])
          .transition()
          .duration(2000)
          .style("width", function(d) { return  d + "px"; });
  </script>
```



## What else can be done?

* Scales
*


