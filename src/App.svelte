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
	var fileName = "Outstanding profile pic.png";
	var clubhousePathClip = new Path2D("M0.993666 481.208C0.468732 453.407 0.214378 437.809 0.993559 410.024C4.80575 274.084 52.4507 171.86 127.368 103.618C202.292 35.372 304.538 1.07061 417.623 1.06981C428.461 1.06974 437.62 0.911257 446.278 0.761454C462.246 0.48516 476.508 0.238381 496.45 1.06938C618.461 6.15372 715.616 39.8256 783.855 105.834C852.093 171.84 891.486 270.247 897.84 404.948C897.897 406.162 897.953 407.358 898.009 408.536C899.549 441.059 900.468 460.486 897.841 493.893C888.944 607.014 861.619 701.617 799.409 771.117C737.207 840.608 640.058 885.08 491.341 897.789C460.67 900.41 441.773 899.557 413.678 898.289C410.079 898.126 406.329 897.957 402.384 897.787C284.204 892.706 185.139 855.862 114.985 786.671C44.835 717.482 3.53678 615.894 0.993666 481.208Z");
	

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
		ctx.drawImage(rings[ringSelection].image, 0, 0, 900, 900)
		
		// Aggiunge immagine
		setTimeout(() => {
			
			// Calcolo proporzioni
			var proporzione = curWidth/900;
			var altezzaDaUsare = curHeight/proporzione
			var larghezzaDaUsare = 900
			var offsetY = -(altezzaDaUsare-900)/2
			var offsetX = 0
			
			// Disegno l'immagine
			ctx.drawImage(newImage, offsetX, offsetY, larghezzaDaUsare, altezzaDaUsare)
			
			// Disegno l'anello
			ctx.drawImage(rings[ringSelection].image, 0, 0, 900, 900)

			// Disegno bordino bianco
			// ctx.lineWidth = lineWidth + 10;
			// ctx.strokeStyle = "#FFFFFF"
			// ctx.stroke(clubhousePath);
			
			// Disegno bordino bianco
			// ctx.lineWidth = lineWidth;
			// ctx.strokeStyle = lineColor;
			// ctx.stroke(clubhousePath);

			newProfileImage.src = canvas.toDataURL()
			
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

	<!-- Promote?
	<a href="mailto:claudiopostinghel@gmail.com" target="_blank" rel="noopener noreferrer">
		<div id="promoteSpace" class="darkContainer dotted" style="padding:12px 24px; display: flex; flex-direction: row;">
			Promote your event on CH here
		</div>
	</a> -->
	

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
	
				<canvas id="myCanvas" height="900" width="900"></canvas>

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
				<br>
				<p style="margin-top: 20px">
					Problem with your browser?<br>Long press and save the image below<br>👇
				</p>
			{/if}

			<center>
				<img id="newProfileImage" width={downloadable? 100:0} height={downloadable? 100:0} src="" alt="">
			</center>

		</center>

	</div>

	<div class="darkContainer">
		<p style="margin:0; text-align:center;">Feel free to
			<a href="http://bit.ly/2OfAgCG" style="font-weight:600 !important;">buy me a coffee</a>
			or
			<a href="https://bit.ly/3oY9c7u" style="font-weight:600 !important;">follow me on Twitter</a> :)</p>
	</div>

	<div class="darkContainer">
		<h4>Why does it work</h4>
		<p>Simple, yet effective — it puts a contrast–ring that make you the most notable person in the room!</p>
		<blockquote>
			Gestalt psychologists emphasized that organisms perceive entire patterns or configurations, not merely individual components. <br>
			The view is sometimes summarized using the adage, "the whole is more than the sum of its parts." <br>
			It determine how humans perceive visuals in connection with different objects and environments. <br>
		</blockquote>
	</div>


	<!-- Stuff for the script -->
	<img id="output" class="hide" width="200" alt="" />
	<img id="exampleImage" class="hide" src="images/ryan_hoover.jpg" width="200" alt="" />
	

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
		overflow: hidden;

		cursor: pointer;
	}

	canvas {
		/* border: 1px solid red; */
		margin-bottom: 40px;
		background-color: transparent;
		/* background-image: url("images/anteprima.png"); */

		transform-origin: 0 0;
		transform: scale(0.333, 0.333);

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
		-webkit-appearance: none;

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