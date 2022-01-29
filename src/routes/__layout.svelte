<script lang="ts">
	import Header from '$lib/header/Header.svelte';
	import '../app.scss';
	import { onMount } from 'svelte';

	onMount(() => {
		const registerIdleCallback = window['requestIdleCallback'] || function (handler) {
			const startTime = Date.now();
		
			return setTimeout(function () {
				handler({
					didTimeout: false,
					timeRemaining: function () {
						return Math.max(0, 50.0 - (Date.now() - startTime));
					}
				});
			}, 1);
		};

		registerIdleCallback(() => {
			if (CSS && 'paintWorklet' in CSS) CSS.paintWorklet.addModule('https://unpkg.com/smooth-corners');
		}, {
			timeout: 1000
		});
	});

	let messengerHidden = true;

	const chatLinkClicked = () => {
		messengerHidden = false;

		FB.CustomerChat.update({  
			logged_in_greeting: 'Hej, hur kan vi hjälpa till?',
			logged_out_greeting: 'Hej, hur kan vi hjälpa till?',
		});

		FB.CustomerChat.showDialog();
		document.body.classList.add('chat-link-clicked');
		document.body.dispatchEvent(new CustomEvent('chat-link-clicked'));
	}
</script>

<Header />

<main>
	<slot />
</main>

<footer>
	<div>
		<menu>
			<h3>för e&#8209;handlare</h3>
			<a href="https://foobar.io">logga in</a>
			<a href="/skapa-konto">skapa konto</a>
		</menu>
		<menu id="contact">
			<h3>kontakt</h3>
			<!-- svelte-ignore a11y-missing-attribute -->
			<a on:click={chatLinkClicked}><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="currentColor" viewBox="0 0 24 24">
				<path fill="currentColor" fill-rule="evenodd" d="M12 5c-4.532 0-8 3.242-8 7 0 1.351.438 2.619 1.21 3.7a1 1 0 01.121.931l-.763 2.036 2.98-.597a1 1 0 01.625.077 8.863 8.863 0 003.824.853H12c4.532 0 8-3.242 8-7s-3.468-7-8-7zM2 12c0-5.078 4.592-9 10-9s10 3.922 10 9c0 5.077-4.59 9-9.998 9H12h.003-.001a10.864 10.864 0 01-4.377-.905l-4.429.886a1 1 0 01-1.132-1.332l1.215-3.24A8.286 8.286 0 012 12zm5 0a1 1 0 011-1h.01a1 1 0 110 2H8a1 1 0 01-1-1zm4 0a1 1 0 011-1h.01a1 1 0 110 2H12a1 1 0 01-1-1zm4 0a1 1 0 011-1h.01a1 1 0 110 2H16a1 1 0 01-1-1z" clip-rule="evenodd" opacity=".75"/>
			  </svg>
			  chatta med oss</a>
			  <a href="mailto:hej@foobar.se"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="currentColor" viewBox="0 0 24 24">
				<path fill="currentColor" d="M20 4H4c-1.1 0-1.99.9-1.99 2L2 18c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2V6c0-1.1-.9-2-2-2zm0 14H4V8l8 5 8-5v10zm-8-7L4 6h16l-8 5z" opacity=".75"/>
			  </svg>
			  hej@foobar.se</a>
			  <a href="tel:+461234567489"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="currentColor" viewBox="0 0 24 24">
				<g opacity=".75">
				  <path fill="currentColor" d="M16.566 21.999h.028c.528 0 1.027-.208 1.405-.586l2.712-2.712a1 1 0 000-1.414l-4-4a1 1 0 00-1.414 0l-1.594 1.594c-.739-.22-2.118-.72-2.992-1.594-.874-.874-1.374-2.253-1.594-2.992l1.594-1.594a1 1 0 000-1.414l-4-4a1.03 1.03 0 00-1.414 0L2.586 5.999c-.38.38-.594.902-.586 1.435.023 1.424.4 6.37 4.298 10.268 3.898 3.898 8.844 4.274 10.268 4.297zM6.005 5.408l2.586 2.586-1.293 1.293a.996.996 0 00-.271.912c.024.115.611 2.842 2.271 4.502 1.66 1.66 4.387 2.247 4.502 2.271a.994.994 0 00.912-.271l1.293-1.293 2.586 2.586-2.006 2.005c-1.248-.021-5.518-.356-8.873-3.712C4.346 12.921 4.02 8.636 4 7.413l2.005-2.005zm13.994 5.591h2c0-5.13-3.873-8.999-9.01-8.999v2c4.062 0 7.01 2.943 7.01 6.999z"/>
				  <path fill="currentColor" d="M12.999 8c2.103 0 3 .897 3 3h2c0-3.225-1.775-5-5-5v2z"/>
				</g>
			  </svg>
			  076&ndash;123456789</a>
			
		</menu>
		<menu>
			<h3>hjälp</h3>
			<a href="/">status</a>
		</menu>
	</div>
</footer>

{#if messengerHidden}
	<style>
		#fb-root {
			display: none;
		}
	</style>
{/if}


<style lang="scss">
	@import '../lib/mixins';

	main {
		flex: 1;
		display: flex;
		flex-direction: column;
		width: 100%;
		margin: 0 auto;

		> :global(article) {
			width: clamp(
				500px,
				// Minsta tillåtna sidbredd
				calc(var(--content-max-width) + var(--content-margin) + 10vw + 50px),
				// Fönstrets bredd minus vänstermarginalen plus förskjutning relativ till fönstrets storlek
				calc(var(--content-max-width) + var(--content-margin) + 300px) // Max 300px förskjutning
			);
			max-width: calc(100vw - var(--content-margin));
			align-self: center;
			padding: 0 var(--content-margin);
    		align-items: flex-start;

			:global(h1) {
				margin-bottom: 38px;
				max-width: min(19ch, 80vw);
				line-height: calc(2*28px);
			}

			> :global(section) {
				display: flex;
				flex-direction: row;
				gap: 50px;

				> :global(article) {
					flex-grow: 1;
				}

				:global(p) {
					max-width: 55ch;
				}
			}
		}
	}

	footer {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		padding: 40px 0 60px;
		border-top: solid 50px #F4F9F9;
		background-color: #243541;
		color: #D2DADA;
		margin-top: 80px;

		@include mobile {
			padding-bottom: 40px;
			margin-top: 50px;
		}

		> div {
			display: flex;
			flex-direction: row;
			flex-wrap: wrap;

			@include mobile {
				flex-direction: column;
			}

			// Bredd
			width: clamp(
				500px, // Minsta tillåtna sidbredd
				calc(var(--content-max-width) + var(--content-margin) + 10vw + 50px), // Fönstrets bredd minus vänstermarginalen plus förskjutning relativ till fönstrets storlek
				calc(var(--content-max-width) + var(--content-margin) + 300px) // Max 300px förskjutning
			);
			max-width: calc(100vw - var(--content-margin));

			// Intern layout
			justify-content: flex-start;
			padding: 10px var(--content-margin) 10px;
			gap: 24px 120px;

			> menu {
				display: flex;
				flex-direction: column;
				align-items: start;
				padding: 0;

				@media (min-width: 1400px) {
					&#contact {
						display: grid;
						grid-template-columns: 1fr 1fr;
						column-gap: 40px;

						> h3 {
							grid-column: 1/-1;
						}
					}
				}

				> h3 {
					color: #32C39F;
					font-size: 15px;
					font-weight: 450;
					letter-spacing: .01em;
					position: relative;
					margin-bottom: 12px;

					&:after {
						content: '';
						background: #2DB08F;
						height: 2px;
						width: 12px;
						border-radius: 2px;
						position: absolute;
						left: 0;
						top: 26px;
					}
				}

				> a {
					display: flex;
					align-items: center;
					height: 24px;
					font-weight: 500;
					letter-spacing: .03em;
					cursor: pointer;
					font-size: 15px;
					margin: 0;
    				height: 28px;

					@include mobile {
						height: 36px;
					}

					@media (hover: none) {
						height: 36px;
					}

					> svg {
						margin-right: 7px;
					}

					&:hover {
						color: white;
					}
				}
			}
		}
	}
</style>