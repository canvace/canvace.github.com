---
layout: default
permalink: pathfinding.html
---

# Pathfinding with Canvace
This chapter describes how you can use Canvace to move entities efficiently around your map. 
Keep in mind entities can move only on non-solid tiles: if your destination lies on a solid tile, no path will be found by Canvace.

Moving an entity requires two phases:
- the path, if one is present, is computed by Canvace using the A* algorithm, and returned as a list of edges in the map graph;
- the single steps of the path are carried out in sequence.

  var character = this.canvace["stage"].getInstance( { name: n } );
  [...]
  var currentPos = this.character.getPosition();
  var map = this.canvace["stage"].getTileMap();
  var astarNode = map.getGraphNode(Math.round(currentPos.i), Math.round(currentPos.j), layer, destination.i, destination.j);
  this.pathEdges = this.canvace["astar"].findPath(astarNode);

An entity is an abstract description of a dynamic game component: in order to actually operate on it inside the game (e.g. moving it around) you need to
create an instance of that entity. The first instance is always created by default, and can be retrieved from the Canvace.Stage with the getInstance() method (line 1).
Instances in the stage are filtered by their properties: for this reason, in the development environment we added a custom property "name" to the entities, which we
assigned a unique value to.
Other instances can be created and placed on the map with the createInstance() method of Stage.Entity.

In order for the A* to find a path, we need a representation of the tile map as a graph where each tile is a node, while an edge is present between two tiles
when they are adjacent in the map. The findPath method of Canvace.Astar requires a special Canvace.Astar.Node which contains all the needed graph information. The
getGraphNode() method takes the current position and the desired destination, and returns such object. The layer needs to be passed only once, as
you cannot plan a path between tiles located at different layers.

When no path exists, findPath returns NULL. Otherwise, it returns a list of edges. Taking for example the initial node, the item 0 of the list is a string identifying
an edge among those coming out of the node. From this edge, the entity is able to reach the next node (or tile) of the path.

The actual movement from one tile to the next is an interpolated animation. Such animations may change the entity's position, velocity and acceleration vectors.
After changing the vectors, the entity position must be updated: this is done automatically by Canvace if the entity's physics is enabled.

----------------------------