<!-- Hier befindet sich die Navbar, welche immer am unteren randes des Screen eingezeichnet ist und dem User erlaubt zwischen den einzelnen Seiten der App zu toggeln -->

<script>
	import { getStores, navigating, page, updated } from '$app/stores';
	import { element, loop_guard } from 'svelte/internal';

	const pages = ['/SCANNER', '/SPACES', '/RECIPES'];

	const capitalize = (word) => {
		if (word.length === 0) {
			return '';
		} else {
			return word[0].toUpperCase() + word.slice(1);
		}
	};

	const navigation = pages.map((page) => {
		const name = capitalize(page.replace(/\//gm, ''));

		return { name: name, link: page };
	});
	$: isActive = (link) => {
		return $page.url.pathname === link ? true : false;
	};
</script>

<div class="nav-bar">
	<nav>
		<ul>
			{#each navigation as element}
				<li>
					{#if element.link === '/'}
						<a class:active={isActive(element.link)} href={element.link}>🤖</a>
					{:else}
						<a class:active={isActive(element.link)} href={element.link}>{element.name}</a>
					{/if}
				</li>
			{/each}
		</ul>
	</nav>
</div>

<style>
	.active {
		font-size: 20px;
		border-bottom: 1px solid rgb(255, 255, 255);
	}

	.nav-bar {
		margin-top: 32px;
		position: relative;
		z-index: 10000;
	}

	nav ul {
		display: flex;
		flex-direction: row;
		justify-content: space-evenly;
		gap: 62px;
		margin-right: 45px;
		list-style: none;
		font-family: 'parkingregular';
		font-size: 20px;
	}

	a {
		text-decoration: none;
		color: rgb(255, 255, 255);
		/* position: absolute; */
		font-size: 16px;
	}
</style>
