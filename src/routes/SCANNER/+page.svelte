<script>
	import P5 from 'p5-svelte';
	import Ml5 from '$lib/components/Ml5.svelte';
	import Button from '$lib/components/Button.svelte';
	import Delete from '$lib/components/Button.svelte';
	import { writable } from 'svelte/store';
	import Navbar from '$lib/components/Navbar.svelte';

	let classifier;
	let video;
	let results = [];
	let addColor;
	let saveColor;
	let count = 0;

	let ingredients = [];

	let larder = [];
	let fridge = [];

	//Höhe für Kamera wird definiert
	let width = 487.5;
	let height = 390;

	//Kamera wird mit p5 eingezeichnet
	let sketch = (p5) => {
		p5.setup = () => {
			p5.createCanvas(width, height);
			video = p5.createCapture(p5.VIDEO);
			video.size(width, height);

			// Hide the video element, and just show the canvas
			video.hide();
		};

		p5.draw = () => {
			p5.image(video, -97.5, 0, width, height);
		};
	};

	/**
	 * Load the image classification model or a custom one:
	 * https://learn.ml5js.org/#/reference/image-classifier?id=parameters
	 * @param domElement
	 * @param ml5
	 * @param modelReady
	 */
	const mlSketch = (domElement, ml5, modelReady) => {
		// Here you could load another image classification model
		// OR you could load your custom model from teachable machine
		classifier = ml5.imageClassifier('/pasta-pepper-avocado/model.json', video, modelReady);
	};

	const mlReady = () => {
		classifyVideo();
	};
	const classifyVideo = () => {
		classifier.classify(gotResult);
	};

	const removeIngredient = (e, searchElement) => {
		// Die Position des richtigen Ingredient wird gefunden
		const indexToDelete = ingredients.indexOf(searchElement);
		// Ingredient wird aus dem ingredient array gelöscht
		ingredients.splice(indexToDelete, 1);
		ingredients = ingredients;
		// Änderungen werden im Localstorage abspeichern/aktualisieren.
		localStorage.svelteIngredients = JSON.stringify(ingredients);
	};

	const gotResult = (err, res) => {
		if (err) {
			console.error(err);
		} else if (res) {
			results = res;
			classifyVideo();
		}

		//Farbe des ADD Buttons wird geändert falls die Teachable Machine einer der Ingredients erkennt

		if (results[0].label == 'Pasta') {
			addColor = 'secondary';
		}

		if (results[0].label == 'Pepper') {
			addColor = 'secondary';
		}

		if (results[0].label == 'Avocado') {
			addColor = 'secondary';
		}

		if (results[0].label == 'Default') {
			addColor = 'primary';
		}
	};

	//Hier checked das Teachable Machine nach einem Button on Click ob es einen von den vordefinierten Ingredients erkennt. Wenn Ja, dann werden
	//diese in das Ingredients Array geschoben, welches wiederum eingezeichnet wird unterhalb der script und im style

	function buttonPressed() {
		if (results[0].label == 'Pasta') {
			ingredients.unshift('Pasta');
			larder.unshift('Pasta');
			ingredients = ingredients;
			saveColor = 'secondary';
		}

		if (results[0].label == 'Pepper') {
			ingredients.unshift('Pepper');
			larder.unshift('Pepper');
			ingredients = ingredients;
			saveColor = 'secondary';
		}

		if (results[0].label == 'Avocado') {
			ingredients.unshift('Avocado');
			fridge.unshift('Avocado');
			console.log(ingredients);
			ingredients = ingredients;
			saveColor = 'secondary';
		}

		if (results[0].label == 'Default') {
			console.log('Error No Items Detected');
		}
	}

	//Dies ist der zweite button der in Zukunft ausgebaut werden könnte in dem hier das Ingredient Array in die 4 Spaces aufgeteilt werden

	function saved() {
		console.log("SAVED");
	}
</script>

<!-- Hier wird definiert wie groß jedes element auf der Seite ist und in welchen Grid item was eingezeichtnet wird -->

<div class="grid">
	<div class="grid-item-1" style="height: 390px;">
		<div class="content">
			{#if sketch}
				<div class="wrapper">
					<div class="sketch">
						<P5 {sketch} />
					</div>
					<Ml5 {mlSketch} domElement={video} {mlReady} />
					<div class="overlay" />
					<div class="camera"><img src="camera.svg" alt="Four Triangles Surround Camera" /></div>
				</div>
				{#each results as result}
					<!-- <p>{result.label}: confidence: {result.confidence}</p> -->
				{/each}
			{/if}
		</div>
	</div>
	<div class="grid-item-2" style="height: 272px; overflow: hidden; overflow-y: auto;">
		<div class="content">
			<div class="listcontainer">
				{#each ingredients as ingredient, i}
					<li class="ingredientRow">
						<h4 class="labelHeading">{ingredient}</h4>
						<Button size="Delete" on:click={removeIngredient}
							><div class="X"><img src="X.svg" alt="X to Delete" /></div></Button>
					</li>
				{/each}
			</div>
		</div>
	</div>
	<div class="grid-item-3" style="height: 93px;">
		<div class="content">
			<div class="container">
				<Button type={addColor} size="medium" on:click={buttonPressed}>ADD</Button>
				<Button type={saveColor} size="small" on:click={saved}>
						<a href={'/SPACES'}>SAVE</a>
				</Button>
			</div>
		</div>
	</div>
	<div class="grid-item-4" style="height: 90px;">
		<div class="content">
			<Navbar />
			<div class="rectangle" />
		</div>
	</div>
</div>

<style>
	.sketch {
		width: 390px;
		overflow: hidden;
	}

	.camera {
		position: absolute;
		top: 0;
		left: 0;
		margin-top: 35px;
		margin-left: 35px;
	}

	/* Um zu veranschaulichen das die Kamera ein Scanner ist, werden vier Ecken eingezeichnet über die Live Kamera */
	.overlay {
		position: absolute;
		top: 0;
		left: 0;
		width: 390px;
		height: 390px;
		background-color: rgb(0, 0, 0);
		opacity: 20%;
	}

	.wrapper {
		position: relative;
	}

	.labelHeading {
		font-family: 'parkingregular';
		margin-top: 15px;
		margin-left: -5px;
		margin-right: 162px;
		font-size: 20px;
	}

	li {
		border: none;
		width: 340px;
		height: 53px;
		background-color: #cacaca;
		color: #000000;
		list-style: none;
		justify-content: space-evenly;
		margin-bottom: 10px;
		align-items: top;
		overflow: clip;
		box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.15);
		border-radius: 10px;
	}

	.listcontainer {
		display: flex;
		justify-content: space-between;
		margin-left: 25px;
		margin-right: 25px;
		margin-top: 25px;
		flex-direction: column;
	}

	.container {
		display: flex;
		justify-content: space-between;
		margin-left: 25px;
		margin-right: 25px;
	}

	.grid {
		display: grid;
		grid-template-columns: 1fr;
		grid-template-rows: 390px 291px 73px 90px;
	}

	/* Hier wird das schwarze Rechteck unter der Navbar gezeichnet */
	.rectangle {
		position: absolute;
		bottom: 0;
		left: 0;
		right: 0;
		height: 90px;
		background-color: black;
	}

	.ingredientRow {
		display: flex;
	}
	
	a {
		color: white;
		text-decoration: none;
	}
	
</style>
