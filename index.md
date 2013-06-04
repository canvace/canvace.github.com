---
layout: default
jquery: true
no_disqus: true
---

<div id="home-banner">
	<img src="/media/home-banner.png" alt="" />

	<p>The HTML5 Game Platform</p>

	<div id="button-box">
		<a id="view-on-github" href="https://github.com/canvace">
			<img src="/media/icons/github-32.png" alt="">
			<span>View on GitHub</span>
		</a>

		<a id="install-canvace" href="#">
			<img src="/media/icons/install-32.png" alt="">
			<span>Install Canvace</span>
		</a>
	</div>
</div>

<div id="install-instructions">
	<pre><code><span class="shell-comment"># Windows systems</span>
$ npm install -g canvace

<span class="shell-comment"># Mac OS X and other Unix systems</span>
$ sudo npm install -g canvace</code></pre>
</div>

<script type="text/javascript">
$(function () {
	$("#install-canvace").on("click", function (event) {
		event.preventDefault();
		event.stopPropagation();

		$("#install-instructions").slideToggle("slow");
	});
});
</script>

<div id="feature-highlights" class="table">
	<div>
		<div>
			<h2>Save time</h2>

			<p>Canvace's visual environment allows you to <a id="speedrun" href="#">design complex game levels in minutes</a>, while Canvace's JavaScript Game Engine features most of the things your HTML5 games will ever need, including: scene management, efficient rendering, motion physics, collision detection, asynchronous asset loading with progress feedback, frame-by-frame animations, interpolated animations with configurable transition functions, sound support, mobile device detection, input management including touch device support, debug facilities, and a lot more.</p>

			<p>Just worry about the gameplay.</p>
		</div>

		<div>
			<h2>Cross-platform</h2>

			<p>Tired of all that porting stuff? Canvace-powered games are entirely based on HTML5, CSS and AJAX, meaning they are plugin-free and run smoothly on practically any browsers, including mobile ones. Focus on making stunning games and do not worry about rewriting them for each platform.</p>

			<p>Do your team members use three different operating systems for development? Canvace's visual environment is based on Node.js and HTML5 and runs on Windows, Linux or Mac OS X seamlessly.</p>
		</div>
	</div>

	<div>
		<div>
			<h2>Performances</h2>

			<p>Do not worry to reach 60 FPS even on lesser platforms, Canvace will do the trick. Canvace's Game Engine is highly optimized to allow you to efficiently render vast multi-layered isometric or orthogonal environments.</p>

			<p>Just design the game levels and pass them to the JavaScript library.</p>
		</div>

		<div>
			<h2>2D and 2.5D</h2>

			<p>You can make almost any kind of 2D and 2.5D game. Use generic projections to choose between isometric, orthogonal or anything you want. Create fluid, massive, multilayered levels and maps. Import single tiles, entire tile sheets, or make non tiled-based games. Animate your characters and objects.</p>
		</div>
	</div>
</div>

Don't settle for less than this. [Get started now](tutorials/index.html).

<div class="overlay container hidden">
	<div class="overlay content">
		<iframe id="ladybug-speedrun" width="640" height="390"
			src="http://www.youtube.com/embed/Q-haBMqdnQ4?enablejsapi=1">&nbsp;</iframe>
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
		event.stopPropagation();

		$(".overlay.container").toggleClass("hidden");
		$(".close-button").focus();

		player.seekTo(0, true);
		player.playVideo();
	}));

	(function (hideOverlay) {
		$(".close-button, .overlay.container").click(hideOverlay);
		$(window).on("keyup", function (event) {
			if (27 == event.keyCode) {
				hideOverlay(event);
			}
		});
	}(function (event) {
		if (!$(".overlay.container").hasClass("hidden")) {
			event.preventDefault();
			event.stopPropagation();

			player.stopVideo();

			$(".overlay.container").toggleClass("hidden");
			$(".close-button").blur();
		}
	}));
}
</script>