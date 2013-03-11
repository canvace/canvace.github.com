---
layout: default

category: tutorial
title: Rendering a stage in the canvas
index: 3
---

{% include pagelist.html %}

# Displaying a stage in the canvas
In this chapter, we will see how Canvace displays a game stage into a canvas.

## Organization of a Canvace game
Generally speaking, you may organize a Canvace game like this:
- stages directory, for the JSON files representing the stages;
- media directory, containing all the pictures and sound files used in the game;
- index.html: definition of the canvas (and other HTML elements external to the game if needed);
- one or more Javascript files for the game logic.

## Stage loading and rendering
Initially, the stage information exported by the development environment needs to be loaded into Canvace. This is done asynchronously, using a `Canvace.Loader`. The parameters for the constructor specify where the pictures can be found, a progress callback (which in this case simply updates a progress bar), an error callback, and finally a callback for completion. The loading process starts with the call to `loadStage()`, which takes the CSS selector for the canvas and the JSON file of the first stage.

Note: the code below is just a snippet from the complete source, which tries to highlight the most important parts of this procedure.

{% highlight javascript %}
    var loader = new Canvace.Loader({

        basePath: "media", /* where the pictures are found */

        progress: (function (bar) {
            return function (percentage) {
                bar.value = percentage.toFixed();
            }
        }( document.getElementById("progress")) ),

        error: errorReport,

        complete: function (loader, stage) {
            [...]
            var renderLoop = new Canvace.RenderLoop(stage, null, loader, animator, eventLoop.loop);
            renderLoop.run();
        }
    });

    loader.loadStage("#tutorial", "stage/Stage1.json");
{% endhighlight %}

Notice that the `complete` callback gets passed a `Canvace.Stage` instance representing the stage it just loaded. This object will be useful later for retrieving different kinds of information related to the stage, e.g. the entities or the tile map.

As soon as the game information has been stored inside Canvace, it's time to render the stage into the canvas. For this purpose, line 15 initializes the render loop: here we can control the periodical rendering of the stage inside the canvas. The parameters `animator` and `eventLoop.loop` define what happens to the scene between two consecutive renderings. In particular, the `eventLoop.loop` function runs the game logic: it moves the characters, manages collisions and state transitions. The `animator` is a Canvace class for animations which is described in more details in the pathfinding chapter.

At line 16, the `run()` method starts the rendering, and the stage is finally displayed.

![Game stage](images/game-stage.png)

The render loop can be later stopped calling `stop()`, and nothing will be displayed anymore: in the demonstrative game, you can see `stop()` is called at the end,
when the game has been either won or lost:

{% highlight javascript %}
    function lost() {
        renderLoop.stop();
        var losingScreen = document.getElementById("losing-screen");
        losingScreen.style.display = "block";
    }
{% endhighlight %}
    
Line 3 and 4 overwrite the canvas, now empty, with a "You lost!" message. At winning, a similar function is invoked.
Once the render loop has being stopped, it cannot be restarted anymore: if you wish to do so, call `suspend()` instead, and then `run()` to resume it.

----------------------------

{% include next_prev.html %}