---
layout: default
permalink: pathfinding.html
---

# Pathfinding with Canvace
This chapter describes how you can use Canvace to move entities efficiently around your map. 
Keep in mind entities can move only on non-solid tiles: if your destination lies on a solid tile, no path will be found by Canvace.

Moving an entity requires two phases:
- the path, if one is present, is computed by Canvace using the A\* algorithm, and returned as a list of edges in the map graph;
- the single steps of the path are carried out in sequence.

Let's see how this is implemented:

    var character = stage.getInstance( { name: n } );
    ...
    var currentPos = this.character.getPosition();
    var map = stage.getTileMap();
    var astarNode = map.getGraphNode(Math.round(currentPos.i), Math.round(currentPos.j), layer, destination.i, destination.j);
    this.pathEdges = this.canvace["astar"].findPath(astarNode);

Line 1 gets the default instance of the interested entity: see the chapter about entities to see why this is necessary.

In order for the A\* to find a path, we need a representation of the tile map as a graph where each tile is a node, and an edge is present between two tiles
when they are adjacent in the map. The findPath method of Canvace.Astar requires a Canvace.Astar.Node object which contains all the needed graph information. The
getGraphNode() method takes the current position and the desired destination, and returns such object.
The layer needs to be passed only once, as the graph stored inside Canvace allows no connections between tiles on different layers.

When no path exists, findPath returns NULL. Otherwise, it returns a list of edges. Taking for example the initial node, the item 0 of the list is a string identifying
an edge among those coming out of the node. From this edge, the entity is able to reach the next node (or tile) of the path.

    this.interpolationStep = function(duration) {
        var edge = this.pathEdges[this.stage];
        var i,j = getNextPosition(this.character.getPosition(), edge); PLACEHOLDER
        
        animator.interpolatePosition(this.character, {
            i: i,
            j: j
        }, duration, {
            callback: function() {
                // called when interpolation is finished
            }
        });

        this.stage++;
        if (this.stage === this.pathEdges.length) {
            // no more edges, we reached the destination node.
        }
    };

The actual movement from one tile to the next is performed as an interpolated animation. Such animations may influence the entity's position, velocity and acceleration
vectors. After changing the vectors, the entity position must be updated: this is done automatically by Canvace only if the entity's physics is enabled. Otherwise,
you must manually call the update() method for the affected instance.

In the above snippet, each invokation to interpolationStep starts an animation which moves the entity instance character in the next tile of the path. The (i, j)
coordinates of the destination tiles are retrieved at line 3, given the current node and the edge to traverse. You can specify the duration in milliseconds
of the animation with the third parameter, effectively controlling the overall speed of your entity.
The interpolatePosition() method performs a linear interpolation between the two consecutive positions: the effect is that the entity will move from the current
position to the destination at constat speed. It is however possible to specify more complex patterns of movement, either using one of the transition functions
provided by Canvace, or defining your own.
Refer to the Canvace API for a complete list of interpolations and animation options.

Although this code snippet doesn't show it, additional code prevents an interpolation step from being started before the previous one has finished: overlapping more
animations on the same entity instance easily causes errors, and the resulting path may be different from the one computed by A\*.

----------------------------