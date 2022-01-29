<script lang="ts">
	/**
	 * Den här komponenten måste inkluderas på alla sidor som har slide-ins.
	 * Man kan inte lägga den här koden centralt i t ex __layout, för då körs den bara en gång, dvs om
	 * man navigerar iväg från sidan och sen tillbaka så är inte observern upphookad mot elementen längre
	*/

	import { onDestroy, onMount } from 'svelte';

	let slideInElementsObserver: IntersectionObserver;

	onMount(() => {
		slideInElementsObserver = new IntersectionObserver((entries) => {
			entries
				.filter(x => x.isIntersecting)
				.forEach((x, i) => {
					slideInElementsObserver.unobserve(x.target);

					// Staggering:
					setTimeout(() => {
						x.target.classList.toggle('in-viewport', x.isIntersecting);
					}, 150 * i);
				});
		},
		{
			threshold: .2,
			rootMargin: '0px 0px 100px 0px'
		});

		document.querySelectorAll('.slide-in').forEach(e => slideInElementsObserver.observe(e));
	});

	onDestroy(() => {
		slideInElementsObserver?.disconnect();
	});
</script>