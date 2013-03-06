---
layout: default

category: tutorial
title: State machines
index: 7
---

{% include pagelist.html %}

# State machines
The `Canvace.StateMachine` class provides an easy-to-use definition of a deterministic finite automata, or DFA.
In the game, a simple state machine is used to control the reactions to collisions of the main character. We start with a mortal character: whenever it collides with an enemy, a life is lost. The game is lost when you run out of lives. However, during the game, the character can become immortal by stepping on the tile which contains the donut: in this new state, it changes appearance, and it cannot be hurt by enemies anymore.

{% highlight javascript %}
    entity.collisions = new Canvace.StateMachine({
        mortal: {
            enemyCollides: function(enemy) { ... },

            becomeImmortal: function(replacement) {
                ...
                return "immortal";
            }
        },

        immortal: {
            enemyCollides: function(enemy) { ... }
        }
    }, "mortal");
{% endhighlight %}

The `collisions` state machine has two states, `mortal` and `immortal`, with `mortal` set as the initial state in the second parameter. Each state is an object: its properties are functions which specify the available transitions while being in that state. To perform a transition, simply invoke the correspondent function on the DFA.

{% highlight javascript %}
    entity.collisions.enemyCollides(enemyInstance);
{% endhighlight %}

Notice that the same transition name can be defined in multiple states: it is thus possible to execute different instructions for the same "event", depending on the current state, as Canvace automatically invokes the correct transition. For instance the reaction to a collision with an enemy, performed by `enemyCollides()`, changes accordingly to whether our character is mortal or immortal.

Normally transitions change the current state: this is done by returning the name of the new state as a string. For example, when `becomeImmortal()` ends, the state changes from `mortal` to `immortal`. It is also possible to define transitions that don't modify the current state: this happens with `enemyCollides()`, which doesn't return anything.

----------------------------
