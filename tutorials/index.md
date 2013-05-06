---
layout: default

category: tutorial
title: Introduction and installation
index: 1
---

{% include pagelist.html %}

# Introduction
Canvace is a platform designed to help developers creating HTML5-based browser games.
It is composed of two parts: a development environment, where the game appearance and graphical components are set up, and a Javascript API.

The intended workflow when developing with Canvace is outlined here:
- the development environment is used to design the game graphics (maps, characters, ...);
- the stages (levels) composing the game are saved and exported in a standard representation;
- An HTML5 file, containing a canvas which is going to host the game, is created;
- The Javascript API is now employed to assist in the development of the game logic.

This tutorial aims at providing a straightforward but complete introduction to the most important features provided by Canvace. For better fulfilling this task, a demonstrative isometric game has been developed with Canvace: the chapters you're going to read will replay all the steps necessary for its creation, guiding you through the whole development process.

The complete source code for the game can be downloaded HERE.

## Installing Canvace
Canvace requires Node.js: you can download it [here](http://nodejs.org/download/). Then, Canvace is easily installed running

{% highlight bash %}
    $ npm install -g canvace
{% endhighlight %}

from the command line. Please note that, in order to perform this installation process on Unix systems, you'll require administrative rights!

After npm has finished, Canvace is ready to use: the development environment is started by executing `canvace`. On Windows, if `canvace` cannot be found or errors appear it may be necessary to run `canvace.cmd` instead. This is because the system automatically treats `canvace` as a JScript executable.

The most recent version of the client library is available at [this link](http://www.canvace.com/download). Download it in the path you're going to develop the game into, and then import canvace.js in your game files as an external Javascript script.

----------------------------

{% include next_prev.html %}