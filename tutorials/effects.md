---
layout: default
---

# Canvace effects
It is possible to "animate" the stage by enabling some special effects. Effects must be first instantiated and configured, and then added to the stage renderer.

    var renderer = renderLoop.getRenderer();
    var effect = new Canvace.RumbleEffect(100);
    renderer.addEffect(effect);
    
This is an example usage. The Canvace.StageRenderer object is obtained from the RenderLoop, initialized at the beginning of the game. Note that as soon as the effect is
added to the renderer, it is immediately visible at the next rendering iteration. Furthermore, the same effect can be added more than once to the renderer.
Canvace provides two effects.

### Rumble effect
This effects shakes the viewport, simulating rumbling. In the Canvace.RumbleEffect constructor you can set up the duration of the shaking, expressed in frames, and some
additional options controlling the direction and the amplitude of the shaking effect.

### Debug effect
This effect is useful for debugging the game during the development. It may be configured to superimpose a variety of information to the game stage. Among those:
- entities' bounding boxes (only valid for entities with physics enabled);
- entities' velocity, uniform velocity, and acceleration vectors, again only when the physics is enabled;
- solid map (not sure how to explain).

This is how, for example, the debug effect is created in the tutorial game:

    var debug = new Canvace.DebugEffect(stage, {
        drawBoundingBoxes: true,
        drawVelocity: true
    });

The debug effect doesn't have a fixed duration: once added to the renderer, if not explicitly stopped it is displayed for the entire duration of the stage.
However, it can be enabled and disabled using the toggle() method.

----------------------------