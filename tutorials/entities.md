---
layout: default
---

# Entities
Entities are the dynamic components of your game. Typically all the characters in the game are represented as entities.
After creating an entity in the development environment, we can retrieve it from the stage it was created into with the `getEntity()` function, which returns an object of type `Canvace.Stage.Entity`.

## Entities vs. instances
Entities are just an abstract description of the component. You may see that the `Canvace.Stage.Entity` class only stores a quite generic set of information: for example, you may get the coordinates and span of the bounding box assigned to the entity. Bounding boxes are used by Canvace to easily detect collisions among entities with a great variety of different shapes and sizes.
As you can see, entities are filtered in the stage by their properties: for this reason, in the development environment we added a custom property "name"
to all the entities, with a unique value assigned to.

In order to work on the entity inside the game (e.g moving it around) you need to create tangible instances of that entity. A first instance always exists by
default: it can be obtained from the `Canvace.Stage` object, calling the `getInstance()` method. Other instances can be created and placed on the map with the
`createInstance()` method of `Stage.Entity`.

All instances (even when the physics isn't enabled for the correspondent entity) may evolve, during the progress of the game, according to the physics rules
implemented by Canvace. More specifically, each entity stores the following physics-related properties:
- position
- velocity
- uniform velocity
- acceleration
These properties are vectors in a 3-dimensional space. By modifying these vectors it is possible to move the instance around the map: at each rendering loop, the
entity's physics state is changed according to the vectors, and then the actual position of the instance is updated. You don't need to control this process manually: the pathfinding chapter of this tutorial explains how you can use interpolated animations to let Canvace do the job for you.

## Collisions
Canvace is equipped with features for testing and reacting to collisions. An entity may collide with other entities or with tiles.

{% highlight javascript %}
    if (enemy1.collidesWithInstance(enemy2)) {
        enemy1.collision(enemy2);
    }
{% endhighlight %}
    
Here, for example, after we moved all the characters by one step in their current paths, we check whether two instances of enemies, enemy1 and enemy2, are  colliding. The `collidesWithInstance()` method performs this test, while `collision()` tries to restore non-colliding configurations for both. Usually this means the instances briefly move away from each other until a safety condition is met.
Collisions with tiles work in a similar way: `collidesWithTiles()` needs the `Canvace.TileMap.Tile` object retrieved from the stage's tilemap.

## Replacing and removing entities
Once an entity instance has been instantiated, it is possible to change its external appearance by replacing it with an instance of another entity who had a different frame associated to it. This second entity, of course, must have been created and exported from the editor as usual.

In the game, for example, we replace the instance of the main character when it collides with a donut (represented by another instance). First, we retrieve the second entity from the stage using the custom property "name" as filter:

{% highlight javascript %}
    var immortalMan = stage.getEntity({ name: "Stealth" });
{% endhighlight %}

As soon as we detect the collision, the following line performs the replacement:

{% highlight javascript %}
    protagonist.instance = protagonist.instance.replaceWith(immortalMan);
{% endhighlight %}

Note that once the replacement took place, the new instance is immediately created and returned, while the old instance isn't valid anymore, and should be discarded (here, we simply overwrite it with the new one). The new instance also shares all the physics-related information of the old one, i.e. the position, velocity and acceleration vectors. Thus, if the old instance was following a path, the new one will continue to do so.
It's also possible to completely remove an entity's instance from the stage. After the character has been replaced as above, the donut is removed from the map:

{% highlight javascript %}
    donut.remove();
{% endhighlight %}

After that call, the instance won't be rendered anymore.

----------------------------