<!-- Hier würde die Recipes Screen ausgebaut werden -->

<script>
	import Navbar from '$lib/components/Navbar.svelte';
</script>

<div class="rgrid">
	<div class="rgrid-item-1" style="height: 755px;">
		<div class="content">
			<h1>RECIPES</h1>
			<p>This page is underconstruction.</p>
		</div>
	</div>
	<div class="rgrid-item-2" style="height: 90px;">
		<div class="content">
			<Navbar />
			<div class="rectangle" />
		</div>
	</div>
</div>

<style>
	.rectangle {
		position: absolute;
		bottom: 0;
		left: 0;
		right: 0;
		height: 90px;
		background-color: black;
	}

	.rgrid {
		display: grid;
		grid-template-columns: 1fr;
		grid-template-rows: 755px 90px;
	}
</style>
