<svelte:head>
	<meta name="theme-color" content={themeColor}>
</svelte:head>

<script lang="ts">
	import { page } from '$app/stores';
	import { onMount } from 'svelte';
	import { goto } from '$app/navigation';

	let isHeaderStuck = false;

	$: themeColor = isHeaderStuck ? '#243541' : '#f4f9f9';

	const debounce = (fn) => {
		// This holds the requestAnimationFrame reference, so we can cancel it if we wish
		let frame;

		// The debounce function returns a new function that can receive a variable number of arguments
		return (...params) => {
			// If the frame variable has been defined, clear it now, and queue for next frame
			if (frame) {
				cancelAnimationFrame(frame);
			}

			// Queue our function call for the next frame
			frame = requestAnimationFrame(() => {
				// Call our function and pass any params we received
				fn(...params);
			});
		};
	};

	let rootElement: HTMLElement;

	const scrollThreshold = 70;

	const debouncedScrollHandler = debounce(() => {
		isHeaderStuck = window.scrollY > scrollThreshold;
		rootElement.classList.toggle('stuck', isHeaderStuck);
	});

	onMount(() => {
		rootElement = document.getElementById('svelte');

		debouncedScrollHandler();

		window.addEventListener('scroll', debouncedScrollHandler, { passive: true });
	});

	let hamburgerMenuOpen = false;

	function hamburgerClicked() {
		hamburgerMenuOpen = !hamburgerMenuOpen;
		rootElement.classList.toggle('hamurger-menu-open', hamburgerMenuOpen);
	}

	function menuClicked(event:MouseEvent) {
		if (event.target instanceof Element && event.target.tagName === 'A') {
			hamburgerMenuOpen = false;
			rootElement.classList.toggle('hamurger-menu-open', hamburgerMenuOpen);
		}
	}
</script>

{#if $page.path !== '/'}
	<div id="headerblob" />
{/if}
<header class:with-blob={$page.path !== '/'} class:landingpage={$page.path === '/'}>
	<nav on:click={menuClicked}>
		<a href="/" on:click={() => goto('/')}>
			<svg xmlns="http://www.w3.org/2000/svg" class="logo" fill="none" viewBox="0 0 80 97">
				<circle cx="30" cy="30" r="30" fill="#0eba90"/>
			</svg>
			<span>foobar</span>
		</a>
		<ul>
			{#if $page.path !== '/'}
				<li><a href="/#vad-ar-foobar">vad är foobar?</a></li>
			{/if}

			<li class="mobile-only"><a href="#contact">kontakt</a></li>

			<li class="separator mobile-only" />

			<li><a href="/skapa-konto">skapa konto</a></li>
			<li><a href="https://foobar.io">logga in</a></li>
		</ul>
	</nav>

	<button class="hamburger" on:click={hamburgerClicked}>
		<svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 18 12">
			<path
				fill="currentColor"
				style="pointer-events: none;"
				d="M0.75 0.5H17.25V2.33333H0.75V0.5ZM0.75 5.08333H17.25V6.91667H0.75V5.08333ZM0.75 9.66667H17.25V11.5H0.75V9.66667Z"
			/>
		</svg></button
	>
</header>

<style lang="scss">
	@import '../../lib/mixins';

	#headerblob {
		position: fixed;
		background-image: url('/blob.svg');
		background-repeat: no-repeat;
		background-position: calc(50% + 73px) -438px;
		background-size: 1907.5px;
		height: 170px;
		width: 100%;
		top: 0;
		pointer-events: none;
		z-index: -1;
		transition: top 0.2s 0.15s;
	}

	header {
		position: sticky;
		top: 0;
		z-index: 2;
		left: 0;
		right: 0;

		height: 117px;
		display: flex;
		justify-content: center;

		transition: height 0.1s ease-out, right 0.2s;
		pointer-events: none;

		&.with-blob {
			margin-bottom: 80px;

			@include mobile {
				margin-bottom: 45px;
			}
		}

		@include mobile {
			:global(.hamurger-menu-open) & {
				right: 200px;
			}
		}

		&:before {
			content: '';
			background-color: #243541;
			position: absolute;
			height: 60px;
			width: 100%;
			top: -60px;
			transition: top 0.3s 0s;
		}

		> nav {
			display: flex;
			z-index: 1;
			align-items: flex-start;
			pointer-events: all;

			// Bredd
			width: clamp(
				500px,
				// Minsta tillåtna sidbredd
				calc(var(--content-max-width) + var(--content-margin) + 10vw + 50px),
				// Fönstrets bredd minus vänstermarginalen plus förskjutning relativ till fönstrets storlek
				calc(var(--content-max-width) + var(--content-margin) + 300px) // Max 300px förskjutning
			);
			max-width: calc(100vw - var(--content-margin));

			// Intern layout
			justify-content: space-between;
			padding: 10px var(--content-margin) 10px;

			> a {
				padding-top: 12px;
				color: #0eba90;
				display: flex;
				align-items: center;
				gap: 16px;

				&:hover {
					color: #10a781;
				}

				> svg {
					height: 80px;
					transition: height 0.1s ease-out;
					will-change: height, width;

					.fluga {
						transform-origin: center top;
						transform: translateY(0) scale(1);
						fill: transparent;
					}
				}

				> span {
					font-family: lexend, sans-serif;
					font-weight: 500;
					font-size: 27px;
					margin-bottom: 6px;
				}
			}

			> ul {
				display: flex;
				justify-content: center;
				list-style-type: none;
				gap: 30px;
				transition: padding 0.2s;

				> li {
					&.mobile-only {
						display: none;
					}

					> a {
						font-weight: 400;
						position: relative;
						font-family: lexend;
						transition: color 0.1s 0s;
						display: block;
    					height: 36px;
						color: #1C272F;
						overflow: hidden;

						&:after {
							content: '';
							background: #2bc9a2;
							height: 3px;
							width: 15px;
							border-radius: 3px;
							position: absolute;
							right: 0;
							top: 28px;
							transition: width .08s ease-out;
						}

						&:hover {
							color: black;

							&:after {
								width: 200px;
								background: #1C272FD0;
							}
						}
					}
				}
			}
		}

		> .hamburger {
			border: none;
			padding: 0;
			margin: 0;
			outline: none;
			width: 60px;
			height: 60px;
			top: 0;
			right: 0;
			position: absolute;
			background-color: transparent;
			z-index: 2;
			pointer-events: all;
			cursor: pointer;

			@include nonmobile {
				display: none;
			}

			> svg {
				display: none;
				color: #676767;

				padding: 20px;
				height: 60px;
				width: 60px;
				pointer-events: none;

				transition: color 0s .1s;
			}
		}

		&:not(.landingpage) {
			@include mobile {
				> nav > a {
					gap: 14px;

					> svg {
						height: 65px;
					}

					> span {
						font-size: 24px;
					}
				}
			}
		}

		@include mobile {
			> nav > a {
				position: relative;
				left: 0;
				transition: left 0.2s;

				:global(.hamurger-menu-open) & {
					left: -200px;
				}
			}

			> nav > ul {
				position: fixed;
				right: -200px;
				width: 200px;
				bottom: 0;
				top: 0;
				box-shadow: inset 14px 0px 14px -14px #00000033;
				transition: right 0.2s;
				background: #dce0e0;
				flex-direction: column;
				justify-content: start;
				padding: 30px;
				gap: 0;

				:global(.hamurger-menu-open) & {
					right: 0;
				}

				> li {
					&.separator {
						margin-bottom: 14px;

						&:after {
							content: '';
							background: #243541;
							height: 2px;
							width: 12px;
							border-radius: 2px;
							display: inline-block;
						}
					}

					&.mobile-only {
						display: block;
					}

					> a {
						height: 42px;
						display: inline-flex;
    					align-items: center;

						&:after {
							content: none;
						}
					}
				}
			}

			> .hamburger > svg {
				display: block;
			}
		}
	}

	:global(#svelte.stuck) > :global(#headerblob) {
		top: -107px;
		transition-delay: 0s;
	}

	:global(#svelte.stuck) > :global(header) {
		&:before {
			top: 0 !important;
			transition-delay: 0.02s;
		}

		> .hamburger > svg {
			color: #ffffffbf;
		}

		> nav {
			max-height: 60px;
		}

		> nav > a {
			padding-top: 4px;
			gap: 11px;
			color: #2DB085;

			&:hover {
				color: #33c493;
			}

			> svg {
				transition-duration: 0.3s;
				height: 35px;
				width: 28px;

				path {
					stroke-width: 5;
				}

				.fluga {
					fill: currentColor;
					transform: translateY(26px) scale(0.75);
				}
			}

			> span {
				font-size: 17px;
				margin-bottom: 2px;
				font-weight: 450;
			}
		}

		@include nonmobile {
			> nav > ul {
				padding: 6px 0 4px;

				> li > a {
					color: #d2dada;
					font-size: 15px;

					&:after {
						top: 25px;
						height: 2px;
						width: 11px;
						background-color: #269f81;
					}

					&:hover {
						&:after {
							background-color: #d2dadae0;
							width: 120px;
						}
					}
				}
			}
		}
	}
</style>
