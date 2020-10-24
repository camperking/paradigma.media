<script>
	import { quartOut } from 'svelte/easing';
	import { products } from './products';
	import shuffle from './shuffle';
	import { onMount, tick } from 'svelte';


	let shuffledProducts = shuffle(products);

	let mounted = false;
	onMount(()=> {
		mounted = true;
	});

	function style(node) {
		const r = Math.random();
		node.style.fontSize = `${(r*1.2)+1.5}em`;
		node.style.marginBottom = `${r}em`;
	}

	function fadeAsync(node, options) {
		return {
			...options,
			css(t) {
				return `opacity: ${t};`;
			}
		};
	}

	async function update() {
		mounted = false;
		await tick();
		shuffledProducts = shuffle(products);
		mounted = true;
	}


</script>

<main>
	<div class="content">
		<div class="background">
			{#if mounted}
			{#each shuffledProducts as product, index (product)}
			<a href="https://paradigma.media/" use:style  in:fadeAsync={{delay: Math.random()*2500, duration: 2000, easing: quartOut}} class="product">
				{product.name}
			</a>
			{/each}
			{/if}
		</div>
		
		<div class="logo">
			<a href="https://paradigma.media/">
				<img src="paradigma.png" alt="paradigma.media logo" />
			</a>
		</div>
	</div>

	<div on:click={update} class="material-icons">
		refresh
	</div>
</main>

<style>

	main {
		display: flex;
		flex-direction: column;
		align-items: center;
	}

	.content {
		position: relative;
		text-align: center;
		max-width: 65vw;
		margin: 2em auto 0 auto;
		display: flex;
		flex-direction: column;
		justify-content: space-around;
		align-items: center;

	}

	a {
		text-decoration: none;
	}

	.logo {
		position: absolute;
	}

	img {
		background-color: white;
		border: 1px solid black;
		border-radius: 1vw;
		box-shadow: 2px 2px 4px 2px rgba( 0, 0, 0, 0.2 );
		width: 15vw;
		padding: 1vw;
	}

	.background {
		position: relative;
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
		justify-content: space-between;
		align-items: baseline;
		font-size: 0.8vw;
	}

	.product {
		cursor: pointer;
		margin: 0 1.25rem;
		color: #858585;
		font-family: 'Gayathri', sans-serif;
		transition: transform 0.5s linear;
	}

	.product:hover {
		transform: scale(1.2);
		color: hsla(0, 0%, 0%, 0.65);
	}

	.product:hover::first-letter {
		color: #fe2ed4;
	}

	.material-icons {
		cursor: pointer;
		font-size: 2vw;
		color: hsla(0, 0%, 0%, 0.40);
	}

	.material-icons:hover {
		color: hsla(0, 0%, 0%, 0.65);
	}


	@media (max-width: 992px) {

		.content {
			max-width: 80vw;
			margin-top: 25vh;
		}

		.background {
			font-size: 1.1vw;
		}

		.product {
			margin: 0.25rem;
		}
	}
</style>