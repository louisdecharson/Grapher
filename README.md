# Grapher - plotting library

Grapher is a minimal wrapper around [D3.js](https://d3js.org) to do line plots, bar plots and [sparklines](https://www.edwardtufte.com/bboard/q-and-a-fetch-msg?msg_id=0001OR).

#### Table of Contents

-   [Installation](#installation)
-   [Example](#example)
-   [Documentation](#documentation)
-   [See also](#seealso) 
-   [Credits](#credits)

## Installation

```{html}
<script src="./d3.js"></script>
<script src="./grapher.js"></script>
```

## Example

```{html}
<body>

        <h2>My Graph</h2>
        <button onclick="switchBarLine()">Switch bar / lines</button>
        <div id="myPlot"></div>
        
        <script src="https://cdnjs.cloudflare.com/ajax/libs/d3/5.15.0/d3.min.js" integrity="sha256-m0QmIsBXcOMiETRmpT3qg2IQ/i0qazJA2miCHzOmS1Y=" crossorigin="anonymous"></script>
        <script src="./grapher.js"></script>
        <script>
         data = [{"value":3,"Country/Region":"France","date":"2020-01-21T23:00:00.000Z"},
                 {"value":1,"Country/Region":"France","date":"2020-01-22T23:00:00.000Z"},
                 {"value":4,"Country/Region":"France","date":"2020-01-23T23:00:00.000Z"},
                 {"value":7,"Country/Region":"France","date":"2020-01-24T23:00:00.000Z"},
                 {"value":-3,"Country/Region":"Italy","date":"2020-01-21T23:00:00.000Z"},
                 {"value":3,"Country/Region":"Italy","date":"2020-01-22T23:00:00.000Z"},
                 {"value":4,"Country/Region":"Italy","date":"2020-01-23T23:00:00.000Z"},
                 {"value":10,"Country/Region":"Italy","date":"2020-01-24T23:00:00.000Z"}]

         let options = {
             "data": data,
             "x": {
                 "name":"date",
                 "scale": "scaleTime",
                 "tickFormat": d3.timeFormat("%d/%m/%y"),
                 "parse": (d) => new Date(d)
             },
             "y": {
                 "name": "value",
                 "scale": "scaleLinear",
                 "tickFormat": d3.format('.3s')
             },
             "category": "Country/Region",
             "type": "lines",
             "style": {
                 "grid": false
             }
         }
         let myGraph = new Grapher("myPlot", options);
         myGraph.draw();

         function switchBarLine() {
             if (myGraph.options.type == "bar") {
                 myGraph.draw({"type":"lines"});
             } else {
                 myGraph.draw({"type":"bar"})
             }
         }
        </script>
    </body>
```

## Documentation

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

#### Table of Contents

-   [Grapher](#grapher)
    -   [Parameters](#parameters)
    -   [Examples](#examples)
    -   [margin](#margin)
        -   [Parameters](#parameters-1)
    -   [options](#options)
        -   [Parameters](#parameters-2)
    -   [draw](#draw)
        -   [Parameters](#parameters-3)
    -   [downloadData](#downloaddata)
    -   [findTimeFormat](#findtimeformat)
        -   [Parameters](#parameters-4)
    -   [unique](#unique)
        -   [Parameters](#parameters-5)
    -   [getOptimalPrecision](#getoptimalprecision)
        -   [Parameters](#parameters-6)
    -   [updateDict](#updatedict)
        -   [Parameters](#parameters-7)
    -   [getDimensionText](#getdimensiontext)
        -   [Parameters](#parameters-8)
    -   [to_csv](#to_csv)
        -   [Parameters](#parameters-9)
    -   [splitString](#splitstring)
        -   [Parameters](#parameters-10)

### Grapher

The `Grapher` object represents the graph.

You create a `Grapher` by specifying a `container` (a DOM element)
 that will contain the graph, and other options.

#### Parameters

-   `id` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** of the DOM element
-   `options` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Set of options for the graph.
    -   `options.data` **[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)>** Array of JSON containing the data (optional, default `[]`)
    -   `options.x` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Set of options for the x axis
        -   `options.x.name` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Name of the x-axis variable in options.data (optional, default `x`)
        -   `options.x.scale` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** d3-scale, see [d3-scale on Github](https://github.com/d3/d3-scale) (optional, default `"scaleLinear"`)
        -   `options.x.parse` **([function](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function) | null)** Function to parse the data (useful if x-axis is a date) (optional, default `null`)
        -   `options.x.tickFormat` **([function](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function) | null)** Function to format the variable when displayed as tick or in tooltip (optional, default `null`)
        -   `options.x.label` **([string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) | null)** Label for x-axis (optional, default `null`)
        -   `options.x.domain` **([Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)> | null)** Hardcode the x-axis bound. 
            Default is to use min and max of x values in options.data (optional, default `null`)
        -   `options.x.nice` **[Boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** Extends the domain so that it starts and ends on nice round values (applying d3.nice()), for further reference see [d3-scale on Github](https://github.com/d3/d3-scale#continuous_nice) (optional, default `true`)
    -   `options.y` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Set of options for the y axis
        -   `options.y.name` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** name of the y-axis variable in options.data (optional, default `x`)
        -   `options.y.scale` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** d3-scale, see [d3-scale on Github](https://github.com/d3/d3-scale) (optional, default `"scaleLinear"`)
        -   `options.y.parse` **([function](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function) | null)** function to parse the data (useful if y-axis is a date) (optional, default `null`)
        -   `options.y.tickFormat` **([function](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function) | null)** function to format the variable when displayed as tick or in tooltip (optional, default `null`)
        -   `options.y.label` **([string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) | null)** label for y-axis (optional, default `null`)
        -   `options.y.domain` **([Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)> | null)** Hardcode the y-axis bound. Default is to use min and max of y values in options.data (optional, default `null`)
        -   `options.y.nice` **[Boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** Extends the domain so that it starts and ends on nice round values (applying d3.nice()), for further reference see [d3-scale on Github](https://github.com/d3/d3-scale#continuous_nice) (optional, default `true`)
    -   `options.category` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** set of options for category
        -   `options.category.name` **([string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) | null)** name of the category variable in options.data (optional, default `null`)
        -   `options.category.parse` **[function](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function)** function to parse (or format) the category variable when displayed in tooltip (optional, default `(d=>d)`)
    -   `options.categories` **([Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)> | [Boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean))** Hardcode the list of elements of the 'category' variable to select only data's elements belonging to this list. When no list is specified, the list of elements is derived from the data and all 'category' values found in the data are considered. (optional, default `false`)
    -   `options.type` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** type of the graph. Possible types are "line", "bar", "dotted-line", "dot", "sparkline" (optional, default `"line"`)
    -   `options.style` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** list of options for styling the elements of the graph
        -   `options.style.colors` **[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)>** List of colors for the lines, bars, dots (not applicable for sparkline).
               Default is ["#1abb9b","#3497da","#9a59b5","#f0c30f","#e57e22","#e64c3c","#7f8b8c","#CC6666", "#9999CC", "#66CC99"]
        -   `options.style.barWidth` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Share of the x axis width that should be filled with bars. Setting to 0.8, lets 20% in space between bars. (optional, default `0.8`)
        -   `options.style.strokeWidth` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** stroke-width of the line. Default is 3px (optional, default `3`)
        -   `options.style.dotSize` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** dot-size of the dots. Default is 4px (optional, default `4`)
        -   `options.style.tooltipColor` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Text color in the tooltip (optional, default `"#181818"`)
        -   `options.style.tooltipBackgroundColor` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Background color of the tooltip (optional, default `"#ffffff"`)
        -   `options.style.tooltipOpacity` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Opacity of the tooltip. Default is 0.8 (80%) (optional, default `"0.8"`)
        -   `options.style.tooltipLineColor` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Color of the vertical line color (optional, default `"#000000"`)
    -   `options.grid` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Options for the grid
        -   `options.grid.x` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Options for grid on the x-axis
            -   `options.grid.x.show` **[Boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** if true, grid will be added (same frequency as ticks) (optional, default `false`)
            -   `options.grid.x.lines` **[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;{x: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String), label: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String), position: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String), y: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String), textAnchor: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String), class: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)}>** Array for vertical lines. For each line, you should at least specify the position on the x-axis (same format as x axis). You can add a label for which you can specify its position by choosing 'position' value among 'start', 'middle', 'end'. The label can also be more precisely positionned by specifying a 'y' and textAnchor value among 'start', 'middle', 'end'. You can also add a class with 'class'. (optional, default `[]`)
        -   `options.grid.y` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Options for grid on the y-axis
            -   `options.grid.y.show` **[Boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** if true, grid will be added (same frequency as ticks) (optional, default `true`)
            -   `options.grid.y.lines` **[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;{y: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String), label: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String), position: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String), x: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String), textAnchor: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String), class: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)}>** Array for horizontal lines. For each line, you should at least specify the position on the y-axis (same format as y axis). You can add a label for which you can specify its position by choosing 'position' value among 'start', 'middle', 'end'. The label can also be more precisely positionned by specifying a 'x' and textAnchor value among 'start', 'middle', 'end'. You can also add a class with 'class'. (optional, default `[]`)
    -   `options.legend` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Options for the legend
        -   `options.legend.show` **[Boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** show legend. If false, legend will not be displayed. (optional, default `true`)
        -   `options.legend.x` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** legend's x position in the svg (optional, default `15`)
        -   `options.legend.y` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** legend's y position in the svg. Default value equals the margin-top of the g element inside the svg.
        -   `options.legend.interstice` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** space in px between the legend items (optional, default `25`)
        -   `options.legend.backgroundColor` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** legend's background color (optional, default `"#ffffff"`)
        -   `options.legend.opacity` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** opacity of the legend (optional, default `"0.9"`)
    -   `options.download` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** options for downloading data associated with the graph
    -   `options.filename` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** filename for the csv linked to the data (optional, default `"data_<elementId>_<date>.csv"`)
    -   `options.sparkline` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** options for the sparkline object
        -   `options.sparkline.range` **([Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)> | null)** range for the background's 
              band displayed in the sparkline. If null, not the band is not displayed. (optional, default `null`)
        -   `options.sparkline.textLastPoint` **[Boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** display the last point's value as adjacent text. (optional, default `true`)
        -   `options.sparkline.strokeWidth` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** stroke's width of the sparkline (optional, default `1`)
        -   `options.sparkline.lineColor` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** line's color of the sparkline (optional, default `"#000000"`)
        -   `options.sparkline.circleColor` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** circle's color for the last point of the sparkline (optional, default `"#f00"`)
        -   `options.sparkline.textColor` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** text's color of the last point value displayed next to the sparkline (optional, default `"#f00"`)
        -   `options.sparkline.textFontSize` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** text's font size (optional, default `"85%"`)
        -   `options.sparkline.textFontWeight` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** text's font weight (optional, default `"600"`)
        -   `options.sparkline.rangeFillColor` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** range's band fill color (optional, default `"#ccc"`)
        -   `options.sparkline.rangeFillColor` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** range's band fill color (optional, default `"#ccc"`)
    -   `options.advanced` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Advanced options
        -   `options.advanced.additionalColumnsInData` **[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)>** Grapher will only keep x, y and category when parsing the data. Passing columns names here will preserve them in the data. (optional, default `[]`)
-   `width` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** width of the DOM element (if not already setup in HTML or CSS) (optional, default `null`)
-   `height` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** height of the DOM element (if not already setup in HTML or CSS) (optional, default `null`)

#### Examples

```javascript
let myGraphExample1 = new Grapher('myGraphExample1',
                                {"data": data,
                                 "x": {
                                     "name": "date",
                                      "scale": "scaleTime",
                                      "parse": (d) => d3.timeParse('%Y-%m-%d')(d),
                                      "tickFormat": d3.timeFormat("%d/%m/%y"),
                                      "label": "Date"
                                  },
                                  "y": {
                                      "name": "value",
                                      "tickFormat": d3.format('.3s'),
                                      "label": "Glucose"
                                  },
                                  "type": "dotted-line",
                                  });
```

#### margin

Get and sets the values of the inner margins

##### Parameters

-   `margin` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** 
    -   `margin.top` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** margin top (optional, default `10`)
    -   `margin.right` **([string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) \| [function](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function))** margin right depends of the width of the graph (optional, default `(x)=>x<400?20:30`)
    -   `margin.bottom` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** margin bottom
    -   `margin.left` **([string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) \| [function](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function))** margin left depends of the width of the graph (optional, default `(x)=>x<400?40:60`)

Returns **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** 

#### options

Gets and sets the option for `Grapher` object. 
Note that setter is called only by `typing myGraph.options = {something};` 
Typing myGraph.options.data = something; do work but setter is not called and data is not parsed again, etc.

##### Parameters

-   `options` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** @link Grapher

#### draw

Draw the Grapher object

##### Parameters

-   `options` **([Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object) | null)** you can pass an options Object to update the element's option. @link Grapher

#### downloadData

Download the data associated with the Grapher element

#### findTimeFormat

Return the timeformat in d3.timeParse function

##### Parameters

-   `str` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** string containing d3.timeParse(xxx)

Returns **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** time format argument of d3.timeParse function

#### unique

Return unique values of an array (including if there are dates)

##### Parameters

-   `arr` **[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)** array

Returns **[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)** of unique values of 'arr'

#### getOptimalPrecision

Get optimal decimal precision for number formatting

##### Parameters

-   `maxN` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** maximum number in the data

Returns **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** optimal format for d3.format

#### updateDict

Update a nested Object

##### Parameters

-   `dict` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Object to update with values in newDict
-   `newDict` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Object containing key:values to update dict with

#### getDimensionText

Retrieve the width and height of some text based on its font

##### Parameters

-   `text` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** text to be measured
-   `fontSize` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** fontSize of the text

Returns **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Object with width and height of the text as if it was on the DOM

#### to_csv

Transform a JSON object into a csv

##### Parameters

-   `j` **[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)>** Array of objects, each object should has the same number of keys (and each key is going to be a column). 
      Each element of the array is going to be a line.
-   `header` **[Boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** If true, first line of the csv fine will be a header composed of the first object keys (optional, default `true`)

Returns **[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** return a csv file as a string

#### splitString

Split a string in substrings of length of approx. n characters
and splitting on space only. The function will split the string in substring of minimum length 'n'
on spaces inside the string.

##### Parameters

-   `s` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** string to be split
-   `n` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** length of bits to split string 's'
-   `sep` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** separator to be used to for splitting the string. The character should not be inside the string. (optional, default `"|"`)

Returns **[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)** array of substrings

## See also

There are many libraries to do plots out-there, starting by [D3.js](https://d3js.org). You can also have a look to more high-level libraries like [C3.js](https://c3js.org), [plot.ly](http://plot.ly), [Chart.js](http://chartjs.org), [Vega](http://vega.github.io).

## Credits

Copyright: Louis de Charsonville, louisdecharson@posteo.net
