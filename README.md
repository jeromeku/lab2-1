lab2
====

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

 * D3 overlods Javascript data structures
 * Example: `[1, 2, 3].map(function(d, i) {return [d, i];})`, with accessor function `d3.max([[1,2], [2, 9], [3, 4]], function(d) { return d[0];})`
 * 
 
### Selection and Selections

* D3 returns an array as a selection, using CSS to query.
* You can then apply operators on them (styles, attributes, properties)
* 

### Joining Data to selections

* This is probably the most important concept in D3

```javascript
    var data = [10, 20, 30 , 40, 50];

    d3.select("body").selectAll("p")
        .data(data)
        .append("p")
        .transition().delay(function(d, i) { return 500*i; })
        .text(function(d) { return d});
```

* Examples with existing elements

It creates enter() and exit() subselections


* Placeholders are empty elements created to contain future elements




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


