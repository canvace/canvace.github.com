---
layout: default
---

# Entities
Entities are the dynamic components of your game. Typically all the characters in the game are represented as entities.
After creating an entity in the development environment, we can retrieve it from the stage it was created into with the getEntity() function, which returns an object
of type Canvace.Stage.Entity.

## Entities vs. instances
Entities are just an abstract description of the component. You may see that the Entity class only stores a quite generic set of information: for example,
you may get the coordinates and span of the bounding box assigned to the entity. Bounding boxes are used by Canvace to easily detect collisions among entities with a great
variety of different shapes and sizes.
As you can see, entities are filtered in the stage by their properties: for this reason, in the development environment we added a custom property "name"
to all the entities, with a unique value assigned to.

In order to operate on the entity inside the game (e.g moving it around) you need to create tangible instances of that entity. A first instance always exists by
default: it can be obtained from the Canvace.Stage object, calling the getInstance() method. Other instances can be created and placed on the map with the
createInstance() method of Stage.Entity.

All instances (even when the physics isn't enabled for the correspondent entity) may evolve, during the progress of the game, according to the physics rules
implemented by Canvace. More specifically, each entity stores the following physics-related properties:
- position
- velocity
- uniform velocity
- acceleration
These properties are vectors in a 3-dimension space. By modifying these vectors it is possible to move the instance around the map: at each rendering loop, the
entity's physics state is changed according to the vectors, and then the actual position of the instance is updated. You don't need to control this process manually:
the Pathfinding chapter of this tutorial explains how you can use interpolated animations to let Canvace do the job for you.

## Collisions
Canvace is equipped with features for testing and reacting to collisions. An entity may collide with other entities or with tiles.

{% highlight javascript %}
    var protInst = protagonist.character;
    for (var i = 0; i < enemies.length; ++i) {
        var enemy1 = enemies[i].character;
        ...

        if (protInst.collidesWithInstance(enemy1)) {
            protagonist.loseLife();
        }
    }
{% endhighlight %}
    
Here, for example, we decrement the "life count" of the main character of the game when it collides with one of the enemies. The collidesWithInstance()
method, after testing if a collision is taking place, performs additional operations aimed at restoring a configuration without collisions. If you wish to perform
collision testing and recovery separately, first call testCollision(), and then collision().
Collisions with tiles work in a similar way: collidesWithTiles() only needs the Canvace.TileMap.Tile object retrieved from the stage's tilemap.

(Explain rectangleCollision too?)
(replacing/removing entities?).

----------------------------