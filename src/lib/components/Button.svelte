<!-- Hier befindet sich das Button, dass dem User erlaubt neue Ingredients hinzuzufügen in dem er auf dem Button klicked -->

<script>
	export let type = 'primary';
	export let size = 'medium';
</script>

<button
	class={`${type} ${size}`}
	{...$$restProps}
	small={size === 'small'}
	medium={size === 'medium'}
	Delete={size === 'Delete'}
	on:click
>
	<slot />
</button>

<style>
	button {
		font-family: 'parkingregular';
		border: none;
		box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.2);
		border-radius: 10px;
		font-size: 20px;
	}

	/* Es wird zwischen primary, secondary und tertiary unterschieden, damit, dass button grau bleibt außer das Teachable Machine erkennt ein Ingredient, dann wird der Button schwarz und deutet an das man klicken kann*/
	.primary {
		background-color: #464646;
		color: white;
	}

	.secondary {
		background-color: black;
		color: white;
	}

	.tertiary {
		background-color: #000000;
		color: white;
	}

	.small {
		width: 94px;
		height: 53px;
	}

	.medium {
		width: 231px;
		height: 53px;
	}

	.Delete {
		background-color:  #cacaca;;
		color: white;		
		box-shadow: none;
	}
</style>
