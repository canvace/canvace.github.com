---
layout: default

category: tutorial
title: Stage effects
index: 7
---

{% include pagelist.html %}

# Canvace effects
It is possible to "animate" the stage by enabling some special effects. Effects must be first instantiated and configured, and then added to the stage renderer.

{% highlight javascript %}
    var renderer = renderLoop.getRenderer();
    var effect = new Canvace.RumbleEffect(100);
    renderer.addEffect(effect);
{% endhighlight %}
    
This is an example usage. The `Canvace.StageRenderer` object is obtained from the `RenderLoop`, in turn already initialized and started at the beginning of the game. Note that as soon as the effect is added to the renderer, it is enabled at the next rendering iteration. Also, the same effect can be added more than once to the renderer.
Canvace provides two effects.

### Rumble effect
This effects shakes the viewport, simulating rumbling. In the `Canvace.RumbleEffect` constructor you can set up the duration of the shaking, expressed in frames, and some additional options controlling the direction and the amplitude of the shaking effect.

### Debug effect
This effect is useful for debugging the game during the development. It may be configured to superimpose a variety of information to the game stage. Some examples are:
- entities' bounding boxes (only valid for entities with physics enabled);
- entities' velocity, uniform velocity, and acceleration vectors, again only if the physics is enabled;
- solid map: highlights the solid tiles.

This is how, for example, the debug effect is instantiated in the tutorial game:

{% highlight javascript %}
    var debug = new Canvace.DebugEffect(stage, {
        drawBoundingBoxes: true,
        drawVelocity: true
    });
{% endhighlight %}

The debug effect doesn't have a fixed duration: once added to the renderer, if not explicitly stopped it is displayed for the entire duration of the stage.
However, it can be enabled and disabled using the `toggle()` method. In the game, the debug effect is alternatively toggled on and off when pressing the space bar.

----------------------------

{% include next_prev.html %}