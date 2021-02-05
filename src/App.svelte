<script>

	// Import
	import { onMount } from 'svelte';

	// Export
	export let name;

	// Variables
	var myCanvas;
	var myImage;

	// On mount
	onMount(() => {

		// Trovo gli elementi
		myCanvas = document.getElementById("myCanvas");
		myImage = document.getElementById("myImage");

		// Disegno la canvas
		var ctx = myCanvas.getContext("2d");

		var img = new Image();
		img.onload = function() {
			ctx.drawImage(img, 0, 0);
		}
		img.src = "http://upload.wikimedia.org/wikipedia/commons/d/d2/Svg_example_square.svg";

	});

	// Converts canvas to image
	function display() {
		const dataURI = myCanvas.toDataURL();
		
		myImage.src = dataURI;
	}

	

</script>

<main>
	<h1>Clabhouse Ring!</h1>

	<h4>Canvas:</h4>
	<canvas id="myCanvas" crossorigin="anonymous" ></canvas>

	<h4>Image</h4>
	<img id="myImage" src="" alt="">

	<br>

	<button id="mainButton" on:click={display}>Display!</button>
	<button id="mainButton">Convert!</button>

</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;

		display: flex;
		flex-direction: column;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	canvas {
		border: 1px solid red;
	}

	img {
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>