---
layout: default
---

# Input events
A crucial component of any game is user interaction. In this chapter, we will present the classes provided by Canvace for reacting to mouse and keyboard events.

## Mouse events
In order to register handlers for mouse events, first a `Canvace.Mouse` object must be created:

{% highlight javascript %}
    var mouse = new Canvace.Mouse(canvas);
{% endhighlight %}

the first and only parameter specifies the canvas object for which the object will capture the mouse events.
You can now implement and set some callback functions, and react to a variety of mouse events. An example is shown below:

{% highlight javascript %}
    mouse.onDown( function(x, y, button) {
        var destination = stage.getView().getCell(x, y, layer);
        var tileID = map.getAt(destination.i, destination.j, destination.k);
        var tile = map.getTile(tileID);
        [...]
    });
{% endhighlight %}

The function is called whenever the user left-clicks inside the canvas. The x and y parameters identify the point clicked by the user, in canvas coordinates, while
button is an identifier for the particular mouse button being pressed (0 = left button, 1 = middle, if present, 2 = right button).
The function body shows how we can retrieve which tile has been clicked. First, the canvas coordinates are translated into (i, j, k) coordinates of the tile map with the `getCell()` method of the `Stage.View` class. Read below for a more detailed explanation of `Stage.View`'s features.

Next, the tile map tells us which tile corresponds to the i, j and k coordinates. Although not showed in the snippet, the `TileMap` associated to a stage is obtained by the `Canvace.Stage` class itself, calling `getTileMap()`.

The API documentation for the `Canvace.Mouse` class lists all the mouse events you can receive. If you wish to disconnect an event handler, save the return value of
the method you used to register it (e.g. `onDown` on the example above): it contains a function which unregisters the handler.

## Keyboard events
Likewise, the `Canvace.Keyboard` class provides you access to keyboard events. Key codes in Canvace can be specified using the DOM_VK_XXX codes, listed
[here](https://developer.mozilla.org/en-US/docs/DOM/KeyboardEvent "KeyBoardEvent"): they are automatically normalized across browsers by Canvace.

In the game, we react to the pressing of the space bar by toggling on and off the debug effect.

{% highlight javascript %}
    var keyboard = new Canvace.Keyboard(window);
    var debugStatus = false;

    keyboard.onKeyDown(KeyEvent.DOM_VK_SPACE, function(code) {
        debugStatus = !debugStatus;
        debug.toggle(debugStatus);
    });
{% endhighlight %}
    
Note that, at line 1, `Canvace.Keyboard` is initialized with the global window object, and not just for the canvas. The `debug` variable contains the `DebugEffect`,
already added to the stage renderer, but disabled. Pressing the space key alternatively turns it on and off. The handler also receives the key code, which is
unused here.

There are three main keyboard events you can react to:
- `onKeyDown`: a key has being pressed.
- `onKeyPress`: a key has being pressed. The only difference with `onKeyDown` is found when a key is being held down for a longer time: the `onKeyDown` event is fired only once, whereas `onKeyPress` will be fired repeatedly.
- `onKeyUp`: a key has been released.

# Stage view
The `Stage.View` class helps in managing the rendering viewport and the projection of the stage on the canvas. This class is seldom instantiated directly: the view
associated to your current stage is retrieved with `stage.getView()`.

## Dragging the view
If your stage is bigger than the canvas it is rendered in, it is possible to drag around the view, and show different portions of the level.

{% highlight javascript %}
    mouse.onDrag(function (x0, y0, x, y, button) {
        if (button === 2) {
            stage.getView().drag(x - x0, y - y0);
        }
    });
{% endhighlight %}

Here we make the view follow the mouse pointer as it is being dragged around. The callback for `onDrag` takes five parameters: the position where the drag started and the one where the drag ends, both in canvas coordinates, and an identifier of the mouse button. The `drag()` method of `Stage.View` displaces the origin of the view, initially placed at the top-left corner of the canvas, by the given x and y offsets. In order to move the view to an absolute position (again in canvas coordinates) use `dragTo()`.

## Projecting and unprojecting
`Stage.View` also provides methods for transforming from canvas coordinates (x, y, z) into map coordinates (i, j, k), and vice versa.

- `project`: goes from map coordinates to canvas ones, using the defined projection matrix.
- `projectElement`: quite similar to `project()`, it makes this conversion for a given game element (tile or entity), thus adding its offsets to the computed coordinates. It doesn't take into account the changes on the view's origin position due to dragging;
- `unproject`: performs the inverse transformation at the specified layer: takes x, y and k, and returns i, j and k. Unlike the `getCell()` method mentioned above, which works only for tiles, it works for any point of the canvas.

----------------------------