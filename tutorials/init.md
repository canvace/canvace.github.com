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
Initially, the stage information exported by the development environment needs to be loaded into Canvace. This is done asynchronously in two separate steps.

First, the JSON file Stage1.json is loaded using Ajax (line 1). The anonymous function, called when the loading has completed, gets the whole data as an object. Then, the "media" directory, containing all the pictures used for the game, is loaded by instantiating a `Canvace.Loader` object and calling the `loadAssets()` method. This process may require some time; hence, a progress callback is defined in the `progress` property: it updates the status of a progress bar. We also set a function to be called at completion, and another that reports errors. The completion callback is the one that initializes all the Canvace objects and starts the actual game logic.

Note: the code below is just a snippet from the complete source, which tries to highlight the most important parts of these functions.

{% highlight javascript %}
    Canvace.Ajax.getJSON("Stage1.json", function(data) {
        var loader = new Canvace.Loader({

            basePath: "media", /* where the pictures are found */

            progress: (function (bar) {
                return function (percentage) {
                    bar.value = percentage.toFixed();
                }
            }( document.getElementById("progress")) ),

            error: errorReport,

            complete: function () {
                var stage = new Canvace.Stage(data, "#tutorial");
                [...]
                var renderLoop = new Canvace.RenderLoop(stage, null, loader, animator, eventLoop.loop);
                renderLoop.run();
            }
        });

        loader.loadAssets(data);
    }, errorReport);
{% endhighlight %}

As soon as the game information has been stored inside Canvace, it's time to render the stage into the canvas. Line 15 retrieves the canvas object from the DOM.
Line 16 associates a `Canvace.Stage` object with the game data and the canvas, identified by its CSS selector. This class is useful to retrieve all the game components for this stage. At last, the render loop is initialized: here we can control the periodical rendering of the stage inside the canvas. The parameters `animator` and `eventLoop.loop` define what happens to the scene between two consecutive renderings. In particular, the `eventLoop.loop` function runs the game logic: it moves the characters, manages collisions and state transitions. The `animator` is a Canvace class for animations which is described in more details in the pathfinding chapter.

At line 20, the `run()` method starts the rendering, and the stage is finally displayed.

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