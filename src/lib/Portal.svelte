<script>
import { browser } from '$app/env';

	import { onMount, onDestroy } from 'svelte';
	let ref;
	let portal;

	onMount(() => {
		portal = document.createElement('div');
		portal.className = 'portal';
		document.getElementById('svelte').appendChild(portal);
		portal.appendChild(ref);
	});

	onDestroy(() => {
		if (browser) {
			document.getElementById('svelte').removeChild(portal);
		}
	});
</script>

<div class="portal-clone">
	<div bind:this={ref}>
		<slot />
	</div>
</div>

<style>
	.portal-clone {
		display: none;
	}
</style>
