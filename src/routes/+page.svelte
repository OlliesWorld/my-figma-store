<script lang="ts">
	import ProductCard from '$lib/productCard.svelte';
	import { get } from 'svelte/store';
	import { cartItems } from '../cart';
	import Menu from '$lib/nav.svelte';
	import Section from '$lib/section.svelte';
import Hero from '$lib/hero.svelte';
	import MoveWords from '$lib/move-words.svelte';
	const products: Product[] = [
		{
			id: 'price_1MkD2jBZRyL10owZA4QmHbbk',
			name: 'Coffee',
			price: 5
		},
		{
			id: 'price_1MkD3mBZRyL10owZaZud41cr',
			name: 'Sunglasses',
			price: 8
		},
		{
			id: 'price_1MkD4EBZRyL10owZD6RlxurI',
			name: 'Bagel',
			price: 3
		},
		{
			id: 'price_1MkD4bBZRyL10owZbhcpTXuV',
			name: 'Sticker',
			price: 2
		}
	];

	async function checkout() {
		await fetch('api/stripeCheckout', {
			method: 'POST',
			headers: {
				'Content-Type': 'application/json'
			},
			body: JSON.stringify({ items: get(cartItems) })
		})
			.then((data) => {
				return data.json();
			})
			.then((data) => {
				data.url;
				window.location.replace(data.url);
			});
	}
</script>

<main >
	<Menu />
	<Hero />
	<MoveWords />
	<Section />
	<div class="card-grid container">
		{#each products as product}
			<ProductCard {product} />
		{/each}
	</div>
	<div class="col-span-3">
		<button class="btn variant-filled-primary" on:click={() => checkout()}
			>Checkout with Stripe API</button
		>
	</div>
</main>
