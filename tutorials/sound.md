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

The "soundResources" variable is a map between sound names and sound URLs in the directory. The second one is an optional parameter: if you don't need any
sound in your game, simply ignore it.

{% highlight javascript %}
    var soundResources = {
        "start-sound" : [ "init.mp3", "init.ogg" ]
    }
{% endhighlight %}

As you see, it is possible to associate more audio files to the same name: this is because not all browsers support the same audio formats. Here, for example, we load a MP3 and a OGG file for the same sound, and we assign them both to the same name, "start-sound". Internally, Canvace will discover which formats your browser is
able to reproduce, and select the right file, if one is present.

Once the loading process is complete, a single reproduction can be performed using the loader itself:

{% highlight javascript %}
    loader.playSound("start-sound");
{% endhighlight %}

This immediately reproduces the specified sound (in the game, that is played when the game starts) once: if you pass "true" as the second parameter, the sound will be
reproduced in loop.

For more complex sound effects, you can obtain the Canvace.Audio.SourceNode object associated with each of your audio resources:

{% highlight javascript %}
    var sound = loader.getSound("start-sound");
{% endhighlight %}
    
You can now start, pause, resume, and play the sound in a loop.

----------------------------
