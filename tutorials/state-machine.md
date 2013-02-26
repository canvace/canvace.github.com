---
layout: default
---

# State machines
The Canvace.StateMachine class provides an easy-to-use definition of a deterministic finite automata, or DFA.
In the game, a simple state machine is used to control the reactions to collisions of the main character. We start with a mortal character: whenever it collides with an enemy, a life is lost. The game is stopped when it runs out of lives. The character can become immortal by stepping on the tile which contains a donut: in this new state, it changes appearance, and it cannot be hurt by enemies anymore.

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

Therefore, this state machine has two states, "mortal" and "immortal", with "mortal" passed as the initial state in the second parameter. Each state is an object: its properties are the functions defining the available transitions while being in that state. To perform a transition, simply invoke the correspondent function:

{% highlight javascript %}
    entity.collisions.enemyCollides(enemyInstance);
{% endhighlight %}

Notice that the same transition name can be used for multiple states: it is possible to execute different instructions for the same "event", depending on the current state, as Canvace automatically invokes the right transition. For instance the reaction to a collision with an enemy, performed by enemyCollides(), changes accordingly to whether our character is mortal or immortal.

Normally transitions change the current state: this is done by returning the name of the new state as a string. For example, when becomeImmortal() ends, the state changes from "mortal" to "immortal". It is also possible to define transitions that don't modify the current state: this happens with enemyCollides(), which doesn't return anything.

----------------------------
