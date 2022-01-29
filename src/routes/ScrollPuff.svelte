<script lang="ts">
	import { browser } from '$app/env';
	import { onDestroy, onMount } from 'svelte';

	let vadArfoobarSeen = browser && localStorage.getItem('FirstHeadlineSeen') || false,
		scrollPuffGracePeriodPassed = false;

	$: showScrollPuff = scrollPuffGracePeriodPassed && !vadArfoobarSeen;

	let vadArfoobarObserver: IntersectionObserver;

	setTimeout(() => {
		scrollPuffGracePeriodPassed = true;
	}, 3_000);

	onMount(() => {
		vadArfoobarObserver = new IntersectionObserver((entries) => {
			if (entries[0].isIntersecting || entries[0].boundingClientRect.bottom < 0 /*om man är nedanför*/) {
				vadArfoobarObserver?.disconnect();
				vadArfoobarSeen = true;
				localStorage.setItem('FirstHeadlineSeen', '1');
			}
		},
		{
			threshold: .2,
		});

		vadArfoobarObserver.observe(document.querySelector('#vad-ar-foobar + h2'));
	});

	onDestroy(() => {
		vadArfoobarObserver?.disconnect();
	});
</script>

<div id="test"></div>

{#if showScrollPuff}
	<a href="#vad-ar-foobar" id="scrollpuff"><img src="/scrollpuff.svg" alt="Läs mer" /></a>
{/if}

<style lang="scss">
	#scrollpuff {
		@import '../lib/mixins';

		@include mobile {
			display: none;
		}

		position: fixed;
		bottom: -40px;
		width: clamp(
			500px,
			calc(var(--content-max-width) + 10vw + 50px - var(--content-margin)),
			calc(var(--content-max-width) + 300px - var(--content-margin))
		);
		max-width: calc(100vw - var(--content-margin) * 2 - 50px);
		left: 50%;
		transform: translateX(-50%);

		z-index: 9999;

		@keyframes bouncein {
			0% { bottom: -150px; }
			7% { bottom: -25px; }
			15% { bottom: -45px; }
			25% { bottom: -32px; }
			35% { bottom: -43px; }
			45% { bottom: -32px; }
			55% { bottom: -41px; }
			65% { bottom: -35px; }
			75% { bottom: -42px; }
			85% { bottom: -36px; }
			95% { bottom: -40px; }
			100% { bottom: -150px; }
		}

		animation: bouncein 7s ease-in-out infinite;

		> img {
			width: 266px;
			height: 156px;
			margin-left: -19px; // skuggan
		}
	}
</style>