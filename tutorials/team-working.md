---
layout: default

category: tutorial
title: Teamworking
index: 10
---

# Teamworking with Darblast

## Giving access to external clients
The Canvace game editor runs on a Node.js server which is able to work across a network. As such, it enables you to work cooperatively, as a team, on the same game project.

By default, nobody can access the game editor and data on your server. Each developer must be authorized by creating a username in Canvace. Users are managed from the Canvace's command line.

    $ canvace setuser <username> <password>

This creates a new user with the associated name and password, or changes the password if the username already exists.

    $ canvace removeuser <username>
    $ canvace clearusers
    
The former command removes an user, while the latter deletes all existing users.

You are also given the ability to configure the port number on which the Canvace HTTP server will listen:

    $ canvace config port <port>
    
The default port number is 80. Remember that if you are running Canvace on Windows, you may need to replace `canvace` with `canvace.cmd` in all the above commands.

## Editing the game
Each game project contains:

- images
- tiles
- entities
- stages

All the stages in a project share images, tiles and entities; they also share the projection matrix. We refer to these as "project resources". In addition to this, a game stage contains some "stage resources" which aren't shared with the other stages in the project: namely, the map and the instances of both tiles and entities.

There is a substantial difference between project and stage resources. If you work on project resources, all changes are immediately visible to all the clients working on the project. This includes, but is not limited to, adding an image, changing the frame associated to a tile, adding labels, etc.... This happens right after the component blur has taken place: an AJAX request causes the broadcast of the changes to all the clients involved in the development session.

On the other hand, changes on stage resources must be explicitly saved in the editor before they become visible by other clients. Until saving, the new changes are simply cached locally. This avoids having to broadcast a huge number of requests during all the development process, since game maps are usually updated at a quick rate.
