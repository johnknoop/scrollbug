<script lang="ts">
import { browser } from '$app/env';

	import { onDestroy, onMount } from 'svelte';

	export let sources: Array<{ path: string; minScreenWidth: number; }>;
	export let altText;

	export let intersectionThreshold = 1;
	export let intersectionMargin = '0px';

	let mostSuitableImagePath;
	let pathToLoad;
	let intersectionObserver: IntersectionObserver;

	const selectMostSuitableImage = () => {
		mostSuitableImagePath = sources
			.filter(x => x.minScreenWidth < window.innerWidth)
			.sort((a, b) => a.minScreenWidth < b.minScreenWidth ? 1:-1)
			[0].path;
	};

	const resizeListener = () => {
		selectMostSuitableImage();
		loadSelectedImage();
	};

	const loadSelectedImage = () => {
		pathToLoad = mostSuitableImagePath;
	};

	onMount(() => {
		window.addEventListener('resize', resizeListener);
		
		// Låt object-taggen parsas av browsern med pathToLoad så att bilddatan cachas upp. Sen tar vi bort taggen igen tills elementet är inom viewporten.
		selectMostSuitableImage();
		loadSelectedImage();

		intersectionObserver = new IntersectionObserver((entries) => {
			if (entries[0].isIntersecting) {
				loadSelectedImage();
				intersectionObserver.disconnect();
			}
		},
		{
			threshold: intersectionThreshold,
			rootMargin: intersectionMargin
		});
	})

	const onFirstLoad = (e: Event) => {
		pathToLoad = '';
		intersectionObserver.observe((e.target as HTMLElement));
	};

	onDestroy(() => {
		if (browser) {
			window.removeEventListener('resize', resizeListener);
			intersectionObserver.disconnect();
		}
	});
</script>

<object type="image/svg+xml" on:load|once={onFirstLoad} data={pathToLoad} title={altText} />