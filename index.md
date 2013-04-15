---
layout: default
jquery: true
no_disqus: true
---

Canvace
=======

The ultimate HTML5 game development platform
--------------------------------------------

<a id="speedrun" href="#">Design your first stage in minutes!</a>

Current Canvace features:

*	Generic 3x3 projection matrix  
	&rarr;	That includes both orthogonal and isometric games
*	2D and 2.5D support
*	Visual development
*	Team development
*	Asynchronous asset loading with progress feedback
*	Scene graph management
*	Frame-by-frame animations
*	Interpolated animations with configurable transition functions
*	Fast, fully functional motion physics with simple AABB collisions  
	&rarr;	Otherwise it works fine with box2d.js
*	Sound support
*	Mobile detection
*	Input APIs, including touch devices
*	`requestAnimationFrame`-based render loop implementation
*	Debug facilities
*	Free and Open Source

Don't settle for less than this. [Get started now](tutorials/index.html).

<div class="overlay container">
	<div class="overlay content">
		<iframe width="560" height="315" src="http://www.youtube.com/embed/Q-haBMqdnQ4" frameborder="0" allowfullscreen="allowfullscreen">&nbsp;</iframe>
		<a class="close-button" href="#">Close</a>
	</div>
</div>

<script type="text/javascript">
$(function () {
	var overlay = $(".overlay.container").detach();

	$("#speedrun").click(function toggleOverlay(event) {
		event.preventDefault();

		if (overlay) {
			overlay.appendTo("body");
			overlay = null;

			$(".close-button").on("click", toggleOverlay);
			$(".overlay.container").on("keyup", function (event) {
				if (27 == event.keyCode) {
					toggleOverlay(event);
				}
			});

			$(".close-button").focus();
		} else {
			$(".close-button").off("click");
			$(".overlay.container").off("keyup");

			overlay = $(".overlay.container").detach();
		}
	});
});
</script>