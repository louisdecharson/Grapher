# Grapher

Grapher is a minimal wrapper around [D3.js](https://d3js.org) to do line plots, bar plots and [sparklines](https://www.edwardtufte.com/bboard/q-and-a-fetch-msg?msg_id=0001OR).

#### Table of Contents

- [Installation](#installation)
- [Example](#example)
- [Documentation](#documentation)
- [See also](#seealso) 
- [Credits](#credits)

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
    -   [findTimeFormat](#findtimeformat)
        -   [Parameters](#parameters-4)
    -   [unique](#unique)
        -   [Parameters](#parameters-5)
    -   [downloadData](#downloaddata)

### Grapher

The `Grapher` object represents the graph.

You cerate a `Grapher` by specifying a `container` (a DOM element)
 that will contain the graph, and other options.

#### Parameters

-   `id` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** of the DOM element
-   `options` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Set of options for the graph.
    -   `options.data` **[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)>** Array of JSON containing the data (optional, default `[]`)
    -   `options.x` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Set of options for the x axis
        -   `options.x.name` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Name of the x-axis variable in options.data (optional, default `x`)
        -   `options.x.scale` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** d3-scale (<https://github.com/d3/d3-scale>) (optional, default `"scaleLinear"`)
        -   `options.x.parse` **([function](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function) | null)** Function to parse the data (useful if x-axis is a date) (optional, default `null`)
        -   `options.x.tickFormat` **([function](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function) | null)** Function to format the variable when displayed as tick or in tooltip (optional, default `null`)
        -   `options.x.label` **([string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) | null)** Label for x-axis (optional, default `null`)
        -   `options.x.domain` **([Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)> | null)** Hardcode the x-axis bound. 
            Default is to use min and max of x values in options.data (optional, default `null`)
    -   `options.y` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Set of options for the y axis
        -   `options.y.name` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** name of the y-axis variable in options.data (optional, default `x`)
        -   `options.y.scale` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** d3-scale, see [d3-scale on Github](https://github.com/d3/d3-scale) (optional, default `"scaleLinear"`)
        -   `options.y.parse` **([function](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function) | null)** function to parse the data (useful if y-axis is a date) (optional, default `null`)
        -   `options.y.tickFormat` **([function](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function) | null)** function to format the variable when displayed as tick or in tooltip (optional, default `null`)
        -   `options.y.label` **([string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) | null)** label for y-axis (optional, default `null`)
        -   `options.y.domain` **([Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)> | null)** Hardcode the y-axis bound. Default is to use min and max of y values in options.data (optional, default `null`)
    -   `options.category` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** set of options for category
        -   `options.category.name` **([string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) | null)** name of the category variable in options.data (optional, default `null`)
        -   `options.category.parse` **[function](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function)** function to parse (or format) the category variable when displayed in tooltip (optional, default `(d=>d)`)
    -   `options.categories` **[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)>** hardcode the list of elements of the 'category' variable. 
          Default is to take all unique values in options.data (optional, default `[]`)
    -   `options.type` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** type of the graph. Possible types are "line", "bar", "dotted-line", "dot", "sparkline" (optional, default `"line"`)
    -   `options.style` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** list of options for styling the elements of the graph
        -   `options.style.colors` **[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)>?** List of colors for the lines, bars, dots (not applicable for sparkline).
               Default is ["#1abb9b","#3497da","#9a59b5","#f0c30f","#e57e22","#e64c3c","#7f8b8c","#CC6666", "#9999CC", "#66CC99"]
        -   `options.style.barWidth` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Share of the x axis width that should be filled with bars. Setting to 0.8, lets 20% in space between bars. (optional, default `0.8`)
        -   `options.style.strokeWidth` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** stroke-width of the line. Default is 3px (optional, default `3`)
        -   `options.style.dotSize` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** dot-size of the dots. Default is 4px (optional, default `4`)
        -   `options.style.grid` **[Boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** Whether to display the y-axis grid (optional, default `true`)
        -   `options.style.tooltipColor` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Text color in the tooltip (optional, default `"#181818"`)
        -   `options.style.tooltipBackgroundColor` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Background color of the tooltip (optional, default `"#ffffff"`)
        -   `options.style.tooltipOpacity` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Opacity of the tooltip. Default is 0.8 (80%) (optional, default `"0.8"`)
    -   `options.tooltipLineColor` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Color of the vertical line color (optional, default `"#000000"`)
    -   `options.legend` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Options for the legend
        -   `options.legend.hide` **[Boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** hide legend (optional, default `false`)
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

##### Parameters

-   `options` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** @link Grapher

#### draw

Draw the Grapher object

##### Parameters

-   `options` **([Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object) | null)** you can pass an options Object to update the element's option. @link Grapher

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

#### downloadData

Download the data associated with the Grapher element

## See also

There are many libraries to do plots out-there, starting by [D3.js](https://d3js.org). You can also have a look to more high-level libraries like [C3.js](https://c3js.org), [plot.ly](http://plot.ly), [Chart.js](http://chartjs.org), [Vega](http://vega.github.io).

## Credits

Copyright: Louis de Charsonville, louisdecharson@posteo.net
