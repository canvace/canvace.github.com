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

		<a id="install-instructions" href="#">
			<img src="/media/icons/install-32.png" alt="">
			<span>Install Canvace</span>
		</a>
	</div>
</div>

<div id="feature-highlights" class="table">
	<div>
		<div>
			<h2>Time savings</h2>

			<p>Vestibulum interdum purus eget tellus convallis scelerisque. Quisque euismod auctor nulla, vitae tempus metus facilisis id. Praesent pellentesque vulputate venenatis. Mauris eu dapibus lectus. Proin posuere accumsan lorem, luctus scelerisque sem pellentesque eget.</p>

			<p>Vivamus id sapien ac nulla pharetra sollicitudin. Cras bibendum cursus enim id auctor. Etiam orci nulla, fringilla eu sagittis et, sodales vel leo. Curabitur sed eros non felis convallis sollicitudin fermentum nec risus. Suspendisse potenti. Nunc congue rhoncus dui at condimentum.</p>
		</div>

		<div>
			<h2>Cross-platform</h2>

			<p>Tired of all that porting stuff? Canvace-powered games are entirely based on HTML5, CSS and AJAX, meaning they are plugin-free and run smoothly on practically any browsers, including mobile ones. Focus on making stunning games and do not worry about rewriting them for each platform.</p>

			<p>Darblast - Canvace's development environment - is a web application and can be used either as a stand-alone application or over a network. You can work with your teammates on the same project at the same time</p>
		</div>
	</div>

	<div>
		<div>
			<h2>Performances</h2>

			<p>Etiam tortor sem, porttitor non fermentum ac, sagittis sed odio. Proin ultricies ultrices volutpat. Cras facilisis cursus tortor, at sagittis sapien tempus a. Integer hendrerit, libero quis egestas porttitor, sem est hendrerit diam, sit amet eleifend eros nisl in magna.</p>

			<p>Suspendisse fringilla, tortor vel dignissim ultrices, massa est placerat arcu, sit amet accumsan libero libero non urna. Donec eros ligula, eleifend et vehicula et, dictum at diam. Suspendisse viverra nulla sem. Integer neque metus, pharetra sit amet tincidunt ac, vestibulum sed purus.</p>
		</div>

		<div>
			<h2>2D and 2.5D</h2>

			<p>You can make almost any kind of 2D and 2.5D game. Use generic projections to choose between isometric, orthogonal or anything you want. Create fluid, massive, multilayered levels and maps. Import single tiles, entire tile sheets, or make non tiled-based games. Animate your characters and objects.</p>
		</div>
	</div>
</div>

<a id="speedrun" href="#">Design your first stage in minutes!</a>

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