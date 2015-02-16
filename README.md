# snap.svg.zpd
[![Gitter](https://badges.gitter.im/Join Chat.svg)](https://gitter.im/huei90/snap.svg.zpd?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

A zoom/pan/drag plugin for Snap.svg

This is an an adaptation of Andrea Leofreddi's [SVGPan library](https://code.google.com/p/svgpan/), version 1.2.2, for use as a [Snap.svg](http://snapsvg.io/) plugin.

This usually use on present view only. Not for Storing or Modifying the paper.

[DEMO](http://huei90.github.io/snap.svg.zpd) [DEMO2](http://huei90.github.io/snap.svg.zpd/demo2.html)
[DEMO3](http://huei90.github.io/snap.svg.zpd/reservation_seat.html)
### In the Wild

Drop me an issue/PR for the showcase

### Install

    $ bower install snap.svg.zpd --save

    $ npm install snap.svg.zpd --save

### Usage


Include `snap.svg.zpd.js` after `snap.svg.js`

```html
<script src="snap.svg.js"></script>
<script src="snap.svg.zpd.js"></script>
```

Writing the script

```js
var paper = Snap();
var bigCircle = paper.circle(150, 150, 100);
paper.zpd();

// with options and callback
paper.zpd(options, function (err, paper) {
    console.log(paper);
});

// with callback
paper.zpd(function (err, paper) {
    console.log(paper);
});
```

### options

#### pan

    true or false: enable or disable panning (default true)
    
#### panHandler

    true or false: sets the background as the only element that can pan the entire zpd object

#### zoom

    true or false: enable or disable zooming (default true)

#### drag

    true or false: enable or disable dragging (default false)

#### zoomScale

    number: Zoom sensitivity (default 0.2)

#### zoomThreshold

    array: min and max zoom level threshold [min, max] (default null)

#### dragItems

    object: object of snap element ids.  keys are id's of the object

```js
    var c1 = paper.circle(100,  100, 3);
    var c2 = paper.circle(200,  100, 3);
    var c2 = paper.circle(300,  100, 3);
    var myDragItems = {};
    myDragItems[c1.id] = c1.id;
    myDragItems[c2.id] = c2.id;
    myDragItems[c3.id] = c3.id;

    paper.zpd({drag:true, dragItems: myDragItems});
```    

### More

#### paper.zpd('destroy')

```js
paper.zpd('destroy');
```
    Destroy all the zpd elements, events and nodes

#### paper.zpd('save')

```js
paper.zpd('save');
// => return SVGMatrix {a:0.6787972450256348,b:0,c:0,d:0.6787972450256348,e:159.63783264160156,f:12.84811782836914}

paper.zpd('save', function (err, data) {
    console.log(data);
    // => return SVGMatrix {a:0.6787972450256348,b:0,c:0,d:0.6787972450256348,e:159.63783264160156,f:12.84811782836914}
});
```
    return current <g> transform attribute (matrix) - only in pan,zoom, not for drag now

#### paper.zpd({ load: SVGMatrix {}})

```js
paper.zpd({ load: {a:0.6787972450256348,b:0,c:0,d:0.6787972450256348,e:159.63783264160156,f:12.84811782836914}});
```
    set the initial <g> transform matrix

#### paper.zpd('origin')

```js
paper.zpd('origin');
```
    back to the origin location

#### paper.zpd('domElement')

```js
paper.zpd('domElement').appendChild(paper.circle(10,10,10).node);    
```

    get the dom Element of zpd to make it easy to add new elements inside the zpd <g> node

#### zoomTo

```js
paper.zoomTo(1.5, 3000, mina.bounce, function (err, paper) {
    console.log(paper);
});
```
    zoom (must > 0), interval (ms optional), mina (optional), callback (optional)

#### panTo

```js
paper.panTo('-1'); // go left -1 x location
paper.panTo('+0', '-1'); // go up -1 y location
paper.panTo(100,100); // go to location (x, y) (100, 100)
paper.panTo(100, 100, 3000, mina.bounce, function (err, paper) {
    console.log(paper);
});
```
    x, y (can be number or string with + -), interval (ms optional), mina (optional), callback (optional)

#### rotate

```js
paper.rotate(15);
paper.panTo(a, x, y, mina.bounce, function (err, paper) {
    console.log(paper);
});
```
    a (rotate degree) x, y (original point), interval (ms optional), mina (optional), callback (optional)
    
### Experimental: Edit
to add an element to the transformation matrix.
select the group that contains the matrix
```js
canvas = Snap.select('#snapsvg-zpd-'+paper.id);
```
create an element and add
```js
canvas.add(element);
```

#### understanding the matrix
Basic concepts of [matrix](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/transform)

to get the current matrix
```js
paper.zpd('save');

return

SVGMatrix {
    a: zoom,
    b: 0,
    c: 0,
    d: zoom,
    e: offset X,
    f: offset Y
}
```
if you need to map one point, for example event.click (x, y)
```js
matrix X = (original X + offsetX) / zoom
matrix Y = (original Y + offsetY) / zoom
```

### Contributor List

[Huei Tan](https://github.com/huei90) <br/>
[Ramon Saccilotto](https://github.com/tikiatua)
