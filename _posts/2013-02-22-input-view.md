---
layout: default
permalink: input-view.html
---

# Input events
A crucial component of any game is user interaction. In this chapter, we will present the classes provided by Canvace for reacting to mouse and keyboard events.

## Mouse events
In order to register handlers for mouse event, first a Canvace.Mouse object must be created:

    var mouse = new Canvace.Mouse(canvas);

the first and only parameter specifies the canvas object for which the object will capture the mouse events.
You can now implement and set some callback functions, and react to a variety of mouse events. An example is shown below:

    mouse.onDown( function(x, y) {
        var destination = stage.getView().getCell(x, y, layer);
        var tileID = map.getAt(destination.i, destination.j, destination.k);
        var tile = map.getTile(tileID);
        [...]
    });

The function is called whenever the user left-clicks inside the canvas. The x and y parameters identify the point clicked by the user, in canvas coordinates.
The function body shows how we can retrieve which tile has been clicked. First, the canvas coordinates are translated into (i, j, k) coordinates of the tile map with
the getCell method of the Stage.View class.
Stage.View contains useful methods for managing the viewport and projection of the stage on the canvas: read below for a more detailed explanation of its features.
Note that you seldom need to explicitly instantiate a Stage.View object, since the Stage.View associated with your stage is returned by getView(). The same applies
to the TileMap of your stage too, although not showed by the code snippet.

Next, the TileMap tells us which tile corresponds to the i, j and k coordinates.

The API documentation for the Canvace.Mouse class lists all the mouse events you can receive. If you wish to disconnect an event handler, save the return value of
the method you used to register it (e.g. onDown on the example above): it contains a function which unregisters the handler.

## Keyboard events
Likewise, the Canvace.Keyboard class provides you access to keyboard events. Key codes in Canvace can be specified using the DOM_VK_XXX codes, listed for example
[here](https://developer.mozilla.org/en-US/docs/DOM/KeyboardEvent "KeyBoardEvent by Mozilla").

In the game, we react to the pressing of the space bar by toggling on and off the debug effect.

    var keyboard = new Canvace.Keyboard(window);
    var debugStatus = false;

    keyboard.onKeyDown(DOM_VK_SPACE, function(code) {
        debugStatus = !debugStatus;
        debug.toggle(debugStatus);
    });
    
Note that, at line 1, Canvace.Keyboard is initialized with the global window object, and not just for the canvas. The "debug" variable contains the DebugEffect,
already added to the stage renderer, but disabled. Pressing the space key alternatively turns it on and off. The handler also receives the key code, which is
unused here.

There are three main keyboard events you can react to:
- onKeyDown: a key has being pressed.
- onKeyPress: a key has being pressed. The difference with onKeyDown is that, in case a key is being held down for a longer time, the onKeyDown event is fired only
once, whereas onKeyPress will be fired repeatedly.
- onKeyUp: a key has been released.

----------------------------