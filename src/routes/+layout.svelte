<main>
	<slot><!-- optional fallback --></slot>
</main>

<style type="postcss">

@font-face {
    font-family: 'parkingregular';
    src: url('/fonts/parking-regular-webfont.woff') format("woff");
    font-weight: normal;
    font-style: normal;
}
	:global(body) {
		margin-top: 0;
		margin-bottom: 0;
		max-width: 100%;
		margin-left: auto;
		margin-right: auto;
		font-family: 'parkingregular';
	}

	@media (max-width: 560px) {
		:global(body) {
			max-width: 100%;
			/* padding: 0 20px; */
			position: flex;
			background-color: #ddd;
		}
	}


</style>
