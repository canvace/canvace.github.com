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

<div class="overlay container hidden">
	<div class="overlay content">
		<iframe id="ladybug-speedrun" type="text/html" width="640" height="390"
  src="http://www.youtube.com/embed/Q-haBMqdnQ4?enablejsapi=1"
  frameborder="0">&nbsp;</iframe>
		<a class="close-button" href="#">Close</a>
	</div>
</div>

<script type="text/javascript">
(function () {
	var tag = document.createElement("script");
	tag.src = "https://www.youtube.com/iframe_api";

	var firstScriptTag = document.getElementsByTagName("script")[0];
	firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);
}());

var player;

function onYouTubeIframeAPIReady() {
	player = new YT.Player("ladybug-speedrun", {
		events: {
			onReady: onYouTubePlayerReady
		}
	});
}

function onYouTubePlayerReady() {
	(function (showOverlay) {
		$("#speedrun").click(showOverlay);
	}(function (event) {
		event.preventDefault();

		$(".overlay.container").toggleClass("hidden");
		$(".close-button").focus();

		player.seekTo(0, true);
		player.playVideo();
	}));

	(function (hideOverlay) {
		$(".close-button").click(hideOverlay);
		$(".overlay.container").on("keyup", function (event) {
			if (27 == event.keyCode) {
				hideOverlay(event);
			}
		});
	}(function (event) {
		event.preventDefault();

		player.stopVideo();

		$(".overlay.container").toggleClass("hidden");
		$(".close-button").blur();
	}));
}
</script>