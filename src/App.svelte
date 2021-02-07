<script>

	// Import
	import { onMount } from 'svelte';

	// Export

	// Variables
	var newProfileImage;
    var newImage = new Image();

	// Rings names
	var ringsNames = ["Orange", "Blue", "Green"]

	// Define rings
	var rings = {
		Orange: {
			image: new Image(),
		},
		Blue: {
			image: new Image(),
		},
		Green: {
			image: new Image(),
		},
	}
	// "red": new Image(),
	rings.Orange.image.src = "images/ring/Orange.svg"
	rings.Blue.image.src = "images/ring/Blue.svg"
	rings.Green.image.src = "images/ring/Green.svg"

	// Ring selection
	let ringSelection = "Orange";

	// Define canvas
	var canvas;
	var ctx;

	// Define stuff for image load
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
	var clubhousePathClip = new Path2D("M1.6634 160.327C1.48961 151.123 1.40552 145.965 1.66329 136.773C2.92339 91.8387 18.6689 58.0822 43.3983 35.5565C68.1332 13.0258 101.902 1.68893 139.28 1.68867C142.88 1.68865 145.917 1.63605 148.785 1.58639C154.066 1.49496 158.772 1.41346 165.366 1.68824C205.714 3.36958 237.794 14.5023 260.311 36.2831C282.826 58.0618 295.85 90.5538 297.951 135.098C297.97 135.499 297.988 135.894 298.007 136.283C298.516 147.052 298.82 153.467 297.952 164.507C295.008 201.931 285.972 233.169 265.444 256.102C244.924 279.027 212.849 293.731 163.66 297.935C153.527 298.8 147.291 298.519 137.989 298.099C136.797 298.045 135.555 297.989 134.248 297.933C95.1774 296.253 62.4629 284.075 39.3078 261.237C16.1558 238.403 2.5042 204.856 1.6634 160.327Z");
	var clubhousePath = new Path2D("M0.664496 160.396C0.489543 151.131 0.404886 145.938 0.664389 136.684C1.93296 91.4475 17.7845 57.4635 42.6806 34.7859C67.5822 12.1034 101.579 0.690202 139.208 0.689938C142.832 0.689912 145.89 0.636968 148.777 0.586972C154.093 0.494924 158.831 0.412874 165.469 0.689504C206.089 2.38216 238.384 13.5898 261.053 35.5175C283.72 57.4431 296.832 90.1541 298.947 134.998C298.966 135.402 298.985 135.799 299.003 136.191C299.516 147.032 299.822 153.49 298.948 164.605C295.985 202.28 286.888 233.729 266.221 256.817C245.563 279.896 213.272 294.699 163.752 298.931C153.551 299.802 147.272 299.519 137.908 299.097C136.709 299.042 135.458 298.986 134.142 298.929C94.8087 297.238 61.8738 284.978 38.5625 261.986C15.2544 238.998 1.51095 205.225 0.664496 160.396Z");
	

	// On mount get elements by ID
	onMount(() => {
		console.log("Mounted");

		// Define canvas
		canvas = document.getElementById('myCanvas');
		// canvas.width = 900;
		// canvas.height = 900;
		
		newProfileImage = document.getElementById("newProfileImage");
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

		// Clip
		ctx.lineWidth = lineWidth;
		ctx.clip(clubhousePathClip);

		// Ring subito
		ctx.drawImage(rings[ringSelection].image, 0, 0, 300, 300)
		
		// Aggiunge immagine
		setTimeout(() => {
			
			// Calcolo proporzioni
			var proporzione = curWidth/300;
			var altezzaDaUsare = curHeight/proporzione
			var larghezzaDaUsare = 300
			var offsetY = -(altezzaDaUsare-300)/2
			var offsetX = 0
			
			// Disegno l'immagine
			ctx.drawImage(newImage, offsetX, offsetY, larghezzaDaUsare, altezzaDaUsare)
			
			// Disegno l'anello
			ctx.drawImage(rings[ringSelection].image, 0, 0, 300, 300)

			// Disegno bordino bianco
			// ctx.lineWidth = lineWidth + 10;
			// ctx.strokeStyle = "#FFFFFF"
			// ctx.stroke(clubhousePath);
			
			// Disegno bordino bianco
			// ctx.lineWidth = lineWidth;
			// ctx.strokeStyle = lineColor;
			// ctx.stroke(clubhousePath);
			
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
			a.href = canvas.toDataURL()
			document.body.appendChild(a)
			a.download = fileName
			a.click()
			document.body.removeChild(a)
		}
	}

	function triggerAlternativeUpload() {
		document.getElementById("file").click()
	}

	function handleColorChange() {
		if (uploaded) {
			drawCanvas(canvas, ctx, newImage)
		}
	}


</script>

<main>

	<a href="mailto:claudiopostinghel@gmail.com" target="_blank" rel="noopener noreferrer">
		<div id="promoteSpace" class="darkContainer dotted" style="padding:12px 24px; display: flex; flex-direction: row;">
			<!-- <img id="plusImage" src="images/plus.svg" alt="plus icon"> -->
			Promote your event on CH her
		</div>
	</a>
	

	<center>
		<h1>Stand out!</h1>
		<p>Rooms are crowded. Never go unnoticed. </p>
		<img id="ryan" src="images/example.png" alt="">
	</center>

	<div class="card">

		<center>

			<div id="canvasSpace" on:click={triggerAlternativeUpload}>

				{#if !uploaded}
					<img src={"images/anteprima_"+ringSelection+".svg"} alt="">
				{/if}
	
				<canvas id="myCanvas" height="300" width="300"></canvas>

			</div>
			
			
			<div id="uploadWrapper" class="upload-btn-wrapper">
				<button class="btn">🚀 Upload</button>
				<input  type="file"  accept="image/*" name="myfile" id="file"  on:change={loadFile} />
			</div>

			<form on:change|preventDefault={handleColorChange}>
				<select bind:value={ringSelection}>
					{#each ringsNames as ring}
						<option value={ring}>
							{ring}
						</option>
					{/each}
				</select>
			</form>

			{#if downloadable}
				<button id="download" class="btn" on:click={downloadAvatar} >😍 Download</button>
			{/if}

		</center>

	</div>


	<div class="darkContainer">
		<h4>How does it work</h4>
		<p>Simple, yet effective — it put a contrast–ring that make you the most notable person in the room!</p>
		<blockquote>
			Gestalt psychologists emphasized that organisms perceive entire patterns or configurations, not merely individual components. <br>
			The view is sometimes summarized using the adage, "the whole is more than the sum of its parts." <br>
			It determine how humans perceive visuals in connection with different objects and environments. <br>
		</blockquote>
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

	#promoteSpace {
		border: 2px dotted #6E664E;
		justify-content: left;
		align-items: center;

		background-color: transparent;

		cursor: pointer;
	}

	#plusImage {
		height: 1em;
		width: auto;

		display: inline-block;
		margin-right: 8px;
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
		width: 300px;
		height: 300px;
		position: relative;

		cursor: pointer;
	}

	canvas {
		/* border: 1px solid red; */
		margin-bottom: 40px;
		background-color: transparent;
		/* background-image: url("images/anteprima.png"); */

		position: absolute;
		top: 0;
		left: 0;
	}

	#ryan {
		height: 120px;
		width: 320px;
		margin-bottom: 24px;
	}

	.hide {
		display: none;
	}
	
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
	#uploadWrapper {
		display: none;
		margin: 0;
	}

	.upload-btn-wrapper {
		position: relative;
		overflow: hidden;
		display: inline-block;

		background: #56AB68;
		box-shadow: 0px 16px 60px rgba(86, 171, 104, 0.5);
		border-radius: 1000px;
	}

	.btn {
		padding: 16px 24px;

		/* Primary Green */
		background: #56AB68;
		border-radius: 1000px;
		font-weight: 600;
		color: #FFFFFF;
		border: none;
		font-size: 24px;

		cursor: pointer !important;
	}

	select {
		margin-top: 32px;
		padding: 16px 24px;

		/* Primary Blue */
		background: #566BAB;
		border-radius: 1000px;
		font-weight: 600;
		color: #FFFFFF;
		border: none;
		font-size: 24px;

		cursor: pointer !important;
	}
	select::-ms-expand {
		display: none;
	}

	#download {
		margin-top: 32px;
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

		cursor: pointer !important;
	}

</style>