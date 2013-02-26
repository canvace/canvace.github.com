---
layout: default
---

# Sounds
In this chapter we explain how to load and play audio resources in Canvace.
All audio resources you use in the game must be first loaded from the "media" directory.

{% highlight javascript %}
    var loader = new Canvace.Loader("media", progress, function () { ... });
    loader.loadAssets(data, soundResources);
{% endhighlight %}

The "soundResources" variable is a map from sound names to one or more source descriptors. This is an optional parameter: if you don't need any
sound in your game, simply ignore it.

{% highlight javascript %}
    var soundResources = {
        "start-sound" : [ "init.mp3", "init.ogg" ]
    }
{% endhighlight %}

As you can see, it is possible to associate more audio files to the same sound name: this is because not all browsers support the same audio formats. Here, for example, we load a MP3 and a OGG file for the same sound, and we associate them both to the same name, "start-sound". Internally, Canvace will try to discover which formats your browser is able to reproduce, and selects the first supported resource, if one is present.

Once the loading process is complete, you can play a sound using the loader itself:

{% highlight javascript %}
    loader.playSound("start-sound");
{% endhighlight %}

This immediately reproduces the specified sound once (in the game, "start-sound" is played when the game starts): if you pass "true" as the second parameter, the sound will be reproduced in loop.

For more complex sound effects, you can obtain the Canvace.Audio.SourceNode object associated with each of your audio resources:

{% highlight javascript %}
    var sound = loader.getSound("start-sound");
{% endhighlight %}
    
You can now start, pause, resume, and play the sound in a loop.

----------------------------
