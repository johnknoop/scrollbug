<script lang="ts">
	import Portal from './Portal.svelte';

	export let title: string;
	export let videoId: string;
	export let length: string;
	export let thumbnail: string;
	export const play = thumbnailClicked;

	function thumbnailClicked() {
		clicked = true;
	}

	function closeButtonClicked() {
		clicked = false;
	}

	let clicked = false;

	function bodyClicked(event: MouseEvent) {
		if (!clicked) {
			return true;
		}

		if (event.target instanceof HTMLElement && event.target.classList.contains('portal')) {
			closeButtonClicked();
		}
	}
</script>

<div class="thumbnail" data-length={length}>
	
</div>

<svelte:body on:click={bodyClicked} />

{#if clicked}
	<Portal>
		<div class="video-popup">
			<header>
				<h3>{title}</h3>
				<button class="close" on:click={closeButtonClicked}><svg width="18" height="18" viewBox="0 0 18 18" fill="none" xmlns="http://www.w3.org/2000/svg">
					<path fill-rule="evenodd" clip-rule="evenodd" d="M18.0004 2.00015L2.0004 18.0002L0.586182 16.5859L16.5862 0.585938L18.0004 2.00015Z" fill="#232323"/>
					<path fill-rule="evenodd" clip-rule="evenodd" d="M16.586 18.0002L0.586031 2.00015L2.00024 0.585938L18.0002 16.5859L16.586 18.0002Z" fill="#232323"/>
					</svg>
					</button>
			</header>
			<iframe
				src="https://www.youtube.com/embed/{videoId}?rel=0&autoplay=1&color=white&modestbranding=1&start=0"
				{title}
				frameborder="0"
				seamless
				allow="autoplay"
				allowfullscreen
			/>
		</div>
	</Portal>
{/if}

<style lang="scss">
	@import 'mixins';

	$no-chrome-breakpoint-width: 700px;
	$no-chrome-breakpoint-height: 500px;
	$less-chrome-breakpoint-width: 1200px;
	$less-chrome-breakpoint-height: 850px;

	.thumbnail {
		display: flex;
		width: 100%;
    	height: 150px;

		background-color: aquamarine;


		cursor: pointer;
		position: relative;

		&:before {
			content: '';
			background-image: url("data:image/svg+xml,%3Csvg width='42' height='59' viewBox='0 0 42 59' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M32.7486 31.9377L36.147 29.5L32.7486 27.0623L7.9986 9.30877L3.25 5.90254L3.25 11.7465L3.25 47.2535L3.25 53.0975L7.9986 49.6912L32.7486 31.9377Z' fill='%23243541' stroke='white' stroke-width='6'/%3E%3C/svg%3E");
			width: 42px;
			height: 59px;
			position: absolute;
			top: 50%;
			left: 50%;
			margin-top: -29px;
			margin-left: -21px;
		}

		&:after {
			content: attr(data-length);
			position: absolute;
			bottom: 5px;
			right: 5px;
			background: #2DB08F;
			color: white;
			font-size: 15px;
			padding: 3px 6px;
			border-radius: 4px;
		}
	}

	:global(.portal) {
		position: fixed;
		left: 0;
		top: 0;
		right: 0;
		bottom: 0;
		display: flex;
		align-items: center;
		justify-content: center;
		z-index: 1000;

		.video-popup {
			will-change: transform, opacity;
			background-color: white;
			box-shadow: 0 0 0 60vmax #00000071, 0 10px 30px #00000020;

			@media (min-width: $no-chrome-breakpoint-width) and (min-height: $no-chrome-breakpoint-height) {
				padding: 30px;
				border-radius: 4px;
				box-shadow: 0 0 0 60vmax #00000031, 0 10px 30px #00000020;
			}

			@media (min-width: $less-chrome-breakpoint-width) and (min-height: $less-chrome-breakpoint-height) {
				padding: 40px;
				padding-top: 30px;
			}

			@keyframes zoomin {
				0% {
					opacity: 0;
					transform: scale(0.2);
				}
				100% {
					opacity: 1;
					transform: none;
				}
			}

			animation: zoomin 0.3s;

			> header {
				display: flex;
				justify-content: space-between;
				padding-bottom: 10px;

				> h3 {
					padding: 0;
				}

				> button {
					border: none;
					padding: 12px;
					margin: 0;
					outline: none;
					background-color: transparent;
					cursor: pointer;
					border-radius: 4px;
					display: flex;
    				align-items: center;
					width: 38px;

					@include nonmobile {
						position: relative;
						top: -10px;
						right: -20px;
					}

					&:hover {
						background-color: #e5efef;
					}
				}
				
				@media (max-width: $no-chrome-breakpoint-width), (max-height: $no-chrome-breakpoint-height) {
					padding: 10px;

					> h3 {
						padding: 10px;
						font-size: 18px;
						line-height: 1.4;
					}
					
					@media (orientation: landscape) {
						display: none;
					}
				}
			}

			> iframe {
				width: 100vw;

				@media (orientation: landscape) {
					// Här vill vi ha en så hög spelare som möjligt, men som samtidigt lämnar utrymme att kunna tappa utanför för att stänga videon
					width: unset;
					height: min(calc(80vw * .56), 100vh);
				}

				@media (min-width: $no-chrome-breakpoint-width) and (min-height: $no-chrome-breakpoint-height) {
					height: unset;

					@media (orientation: landscape) {
						width: min(1920px, calc((100vh - 140px) * 1.77), calc(100vw - 100px));	
					}

					@media (orientation: portrait) {
						// 140px representerar headern och eventuell padding
						width: min(1920px, calc((100vh - 140px) * 1.77), calc(100vw - 100px));	
					}
					
				}

				@media (min-width: $less-chrome-breakpoint-width) and (min-height: $less-chrome-breakpoint-height) {
					// 150px representerar headern och eventuell padding
					width: min(1920px, calc((100vh - 150px) * 1.77), calc(100vw - 120px));
				}

				aspect-ratio: 16 / 9;
			}
		}
	}
</style>
