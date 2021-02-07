<script>

	// Import
	import { onMount } from 'svelte';

	// Export

	// Variables
	var myCanvas;
	var newProfileImage;
    var newImage = new Image();
	var curHeight;
	var curWidth;
	var uploaded = false;
	var downloadable = false;
	var lineWidth = 35;
	var lineColor = "#FF0000";
    newImage.onload = function () {
        curHeight = this.height;
        curWidth = this.width;
    };
	var fileName = "Clubhouse standing profile pic.png";
	var clubhousePath = new Path2D("M0.664496 160.396C0.489543 151.131 0.404886 145.938 0.664389 136.684C1.93296 91.4475 17.7845 57.4635 42.6806 34.7859C67.5822 12.1034 101.579 0.690202 139.208 0.689938C142.832 0.689912 145.89 0.636968 148.777 0.586972C154.093 0.494924 158.831 0.412874 165.469 0.689504C206.089 2.38216 238.384 13.5898 261.053 35.5175C283.72 57.4431 296.832 90.1541 298.947 134.998C298.966 135.402 298.985 135.799 299.003 136.191C299.516 147.032 299.822 153.49 298.948 164.605C295.985 202.28 286.888 233.729 266.221 256.817C245.563 279.896 213.272 294.699 163.752 298.931C153.551 299.802 147.272 299.519 137.908 299.097C136.709 299.042 135.458 298.986 134.142 298.929C94.8087 297.238 61.8738 284.978 38.5625 261.986C15.2544 238.998 1.51095 205.225 0.664496 160.396Z");
	var canvas;
	var ctx;
	

	// On mount get elements by ID
	onMount(() => {
		console.log("Mounted");
		myCanvas = document.getElementById("myCanvas");
		newProfileImage = document.getElementById("newProfileImage");
		canvas = document.getElementById('myCanvas');
		ctx = canvas.getContext('2d');
	});

	// On upload I do something
	function loadFile(event) {
		console.log("File loaded!");
		uploaded = true;
		newImage.src = URL.createObjectURL(event.target.files[0]);

		drawCanvas(canvas, ctx, newImage)
	};

	// Draw canvas
	function drawCanvas(canvas, ctx, newImage) {

		ctx.beginPath()
		
		// context.scale(0.5,0.5)
		ctx.lineWidth = lineWidth;
		ctx.clip(clubhousePath);
		ctx.strokeStyle = lineColor;
		ctx.stroke(clubhousePath);
		
		setTimeout(() => {
			console.log("X " + curWidth);
			console.log("Y " + curHeight);

			var proporzione = curWidth/300;
			var altezzaDaUsare = curHeight/proporzione
			var larghezzaDaUsare = 300
			var offsetY = -(altezzaDaUsare-300)/2
			var offsetX = 0

			console.log("La proporzione è " + proporzione);
			
			ctx.drawImage(newImage, offsetX, offsetY, larghezzaDaUsare, altezzaDaUsare)

			// Disegno bordino bianco
			ctx.lineWidth = lineWidth + 10;
			ctx.strokeStyle = "#FFFFFF"
			ctx.stroke(clubhousePath);

			// Disegno bordino bianco
			ctx.lineWidth = lineWidth;
			ctx.strokeStyle = lineColor;
			ctx.stroke(clubhousePath);
		}, 1000);

		downloadable = true;
	}

	// Converts canvas to image
	function downloadAvatar() {

		console.log("Download new Avatar!");
		
		// ctx.stroke(clubhousePath);

		const dataURI = myCanvas.toDataURL();
		newProfileImage.src = dataURI;

		// For IE/Edge (PNG)
		if (window.navigator.msSaveBlob) {
			window.navigator.msSaveBlob(myCanvas.msToBlob(), fileName)
		} else {
			const a = document.createElement("a")
			a.href = myCanvas.toDataURL()
			document.body.appendChild(a)
			a.download = fileName
			a.click()
			document.body.removeChild(a)
		}
	}


</script>

<main>
	
	<h4>Stand out!</h4>
	<p>Rooms are crowded. Never go unnoticed, thanks to <a href="https://en.wikipedia.org/wiki/Gestalt_psychology">Gestalt psychology</a></p>

	<div class="darkContainer" style="padding:12px 24px">
		Do you want this space? — Let's talk!
	</div>

	<div class="card">

		<center>

			<div id="canvasSpace">

				{#if !uploaded}
					<img src="images/anteprima.png" alt="">
				{/if}
	
				<canvas id="myCanvas" height="300" width="300"></canvas>

			</div>


			<br>
			
			
			<div class="upload-btn-wrapper">
				<button class="btn">🚀 Pimp your avatar!</button>
				<input  type="file"  accept="image/*" name="myfile" id="file"  on:change={loadFile} />
			</div>
			<br>

			{#if downloadable}
				<button id="download" class="btn" on:click={downloadAvatar} >😍 Download</button>
			{/if}

		</center>

	</div>


	<div class="darkContainer">
		<h4>How does it work</h4>
		<p>Simple, yet effective — it put a contrast–ring that make you the most notable person in the room!</p>
		<p>Here's an example:</p>
		<img id="ryan" src="images/example.png" alt="">
	</div>


	<!-- Stuff for the script -->
	<img id="output" class="hide" width="200" alt="" />
	<img id="exampleImage" class="hide" src="images/ryan_hoover.jpg" width="200" alt="" />
	<img id="newProfileImage" class="hide" src="" alt="">
	

</main>

<style>

	main {
		/* Size */
		max-width: 600px;
		width: 100%;
		margin: 0 auto;
		padding: 16px;

		/* For size */
		/* min-height: 100vh; */

		overflow: hidden;
	}

	.darkContainer, .card {
		display: flex;
		flex-direction: column;
		justify-content: center;

		margin-bottom: 32px;
	}

	.darkContainer {
		/* Dark closure */
		background: #E6E3D6;
		border-radius: 24px;

		padding: 32px;
	}

	#canvasSpace {
		height: 300px;
		width: 300px;
		position: relative;
	}

	canvas {
		/* border: 1px solid red; */
		margin-bottom: 40px;
		background-color: transparent;
		background-image: url("images/anteprima.png");

		position: absolute;
		top: 0;
		left: 0;
	}

	#ryan {
		height: 120px;
		width: 320px;
	}

	.hide {
		display: none;
	}

	/* .margin0auto {
		margin: 0 auto;
	} */

	.card {
		/* Card */
		background: #FFFFFF;
		/* Elevation */
		box-shadow: 0px 2px 2px #D0CCBF;
		border-radius: 24px;

		width: 100%;
		padding: 32px;
	}

	

	/* INPUT VERDE */

	.upload-btn-wrapper {
		position: relative;
		overflow: hidden;
		display: inline-block;

		background: #56AB68;
		box-shadow: 0px 16px 60px rgba(86, 171, 104, 0.5);
		border-radius: 1000px;
		cursor: pointer;
	}

	.btn {
		padding: 16px 24px;

		/* Primary green */
		background: #56AB68;
		border-radius: 1000px;
		font-weight: 600;
		color: #FFFFFF;
		border: none;
		font-size: 24px;
	}

	/* .btn.disabled {
		background-color: grey;
	} */

	.upload-btn-wrapper input[type=file] {
		font-size: 100px;
		position: absolute;
		left: 0;
		top: 0;
		opacity: 0;
	}

	/* @media (min-width: 640px) {
		main {
			max-width: none;
		}
	} */
</style>