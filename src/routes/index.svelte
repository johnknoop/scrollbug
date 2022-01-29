<script lang="ts">
	import ResponsiveSvg from '$lib/ResponsiveSvg.svelte';
	import SlideInController from '$lib/SlideInController.svelte';
	import Video from '$lib/Video.svelte';
	import ScrollPuff from './ScrollPuff.svelte';
	
	let playVideo1, playVideo2;
</script>

<svelte:head>
	<title>foobar - affärsplattform för e-handel</title>
	<SlideInController />
</svelte:head>

<section id="hero">
	<div>
		<div id="hero-message">
			<h1>Lorem ipsum dolor Lorem ipsum dolor</h1>
			<p>
				Lorem ipsum dolorLorem ipsum dolorLorem ipsum dolorLorem ipsum dolorLorem ipsum dolorLorem ipsum dolor
				Lorem ipsum dolorLorem ipsum dolorLorem ipsum dolorLorem ipsum dolor.
			</p>
			<a class="button contrast" sveltekit:prefetch href="/skapa-konto">Starta nu</a>			
		</div>
		<div id="illustration" />
	</div>
</section>

<ScrollPuff />

<section id="videos">
	<i class="target" id="vad-ar-foobar" style="display: block; position: relative; top: -130px; visibility: hidden;"></i>
	<h2>Vad är foobar?</h2>
	<p>
		foobar Lorem ipsum dolor Lorem ipsum dolorLorem ipsum dolorLorem ipsum dolorLorem ipsum dolorLorem ipsum dolor Lorem ipsum dolor.
	</p>
	<div id="videos-container">
		<div class="video slide-in" on:click={playVideo1}>
			<h3>Lorem ipsum dolor</h3>
			<Video videoId="123" thumbnail="order" title="Lorem ipsum dolor" bind:play={playVideo1} length="4:35" />
			<ul>
				<li>Lorem ipsum dolor sit amet.</li>
				<li>
					Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet.
				</li>
				<li>Lorem ipsum dolor sit amet</li>
			</ul>
			<button class="button" on:click={playVideo1}>Se video</button>
		</div>
		<div class="video slide-in" on:click={playVideo2}>
			<h3>Lorem ipsum dolor</h3>
			<Video videoId="123" thumbnail="inkop" title="Lorem ipsum dolor" bind:play={playVideo2} length="3:54" />
			<ul>
				<li>Lorem ipsum dolor sit amet.</li>
				<li>Lorem ipsum dolor sit amet.</li>
				<li>Lorem ipsum dolor sit amet.</li>
				<li>Lorem ipsum dolor sit amet.</li>
			</ul>
			<button class="button" on:click={playVideo2}>Se video</button>
		</div>
	</div>
</section>

<section id="hub">
	<div>
		<h2>Allt ingår.</h2>
		<p>Vi har bara en prisnivå och där ingår allt. Vill ni testa foobar under sex månader så är detta helt gratis &mdash; inget kreditkort behövs.</p>
		<div class="teasers">
			<div>
				<h3>Prova gratis</h3>
				<p>utan kostnad i sex månader</p>
				<a href="/skapa-konto" class="button contrast">Starta nu</a>
			</div>
			<div>
				<h3>Har du frågor?</h3>
				<p>Ring oss!</p>
				<a href="tel:+460123456789" class="button"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="currentColor" viewBox="0 0 24 24">
					<g opacity=".75">
					  <path fill="currentColor" d="M16.566 21.999h.028c.528 0 1.027-.208 1.405-.586l2.712-2.712a1 1 0 000-1.414l-4-4a1 1 0 00-1.414 0l-1.594 1.594c-.739-.22-2.118-.72-2.992-1.594-.874-.874-1.374-2.253-1.594-2.992l1.594-1.594a1 1 0 000-1.414l-4-4a1.03 1.03 0 00-1.414 0L2.586 5.999c-.38.38-.594.902-.586 1.435.023 1.424.4 6.37 4.298 10.268 3.898 3.898 8.844 4.274 10.268 4.297zM6.005 5.408l2.586 2.586-1.293 1.293a.996.996 0 00-.271.912c.024.115.611 2.842 2.271 4.502 1.66 1.66 4.387 2.247 4.502 2.271a.994.994 0 00.912-.271l1.293-1.293 2.586 2.586-2.006 2.005c-1.248-.021-5.518-.356-8.873-3.712C4.346 12.921 4.02 8.636 4 7.413l2.005-2.005zm13.994 5.591h2c0-5.13-3.873-8.999-9.01-8.999v2c4.062 0 7.01 2.943 7.01 6.999z"/>
					  <path fill="currentColor" d="M12.999 8c2.103 0 3 .897 3 3h2c0-3.225-1.775-5-5-5v2z"/>
					</g>
				  </svg>
				  076&ndash;686 80 05</a>
			</div>
		</div>
		<hr/>
		<div class="integrations">
			<div>
				<h2>Lorem ipsum dolor sit amet, consectetur adipiscing</h2>
				<p>fLorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla vehicula nulla vel placerat lobortis</p>
				<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla vehicula nulla vel placerat lobortis.</p>
				<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla vehicula nulla vel placerat lobortis.</p>
			</div>
			<div class="image-wrapper">
				<ResponsiveSvg sources={[
					{ path: '/integrationer_liten_animerad.svg', minScreenWidth: 0 },
					{ path: '/integrationer_stor_animerad.svg', minScreenWidth: 1250 },
				]} altText="Woocommerce, Klarna, Unifaun, Fortnox, PostNord" intersectionMargin="-150px 0px 0px 0px" />
			</div>
		</div>
	</div>
</section>

<style lang="scss">
	@import '../lib/mixins';

	:global(.slide-in) {
		opacity: 0;
		transform-origin: top right;
		transform: scale(.8);
		transition: transform .4s .1s cubic-bezier(.17,1.14,.83,1.03), opacity .4s .1s ease-out;
	}

	:global(.slide-in.in-viewport) {
		transform: translateX(0);
		opacity: 1;
	}

	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 1;

		p {
			max-width: min(57ch, 80vw);
			line-height: 28px;
		}

		&#hero {
			overflow: hidden; // Annars visas horisontell scrollbar när fönstret är smalare än 1800px av någon anledning (kanske borde undersöka varför)

			// Bakgrund:
			&::before {
				content: '';
				background-image: url('/blob.svg');
				background-repeat: no-repeat;
				background-position: calc(50% + 40px) -125px;
				background-size: 3039.5px 1166px;
				position: absolute;
				top: 0;
				left: 0;
				right: 0;
				height: 970px;

				@media (max-width: 1720px) {
					// Knuffa ned blobben något
					background-position: calc(50% + 100px) -100px;
				}

				@media (max-width: 1620px) {
					background-position: calc(50% + 100px) -90px;
					height: 1000px;
				}

				@media (max-width: 1470px) {
					background-position: calc(50% + 100px) -70px;
				}

				@include mobile {
					background-position: calc(50% + 100px) -280px;
				}
			}

			// Layout
			min-height: 916px; // Höjden som den får pga sitt innehåll när illustrationen är synlig
			padding-top: 23px;

			@include mobile {
				margin-top: -160px;
			}

			> div {
				@include nonmobile {
					margin-top: 31px; // Så att illustrationen linar upp snyggt med blobbens underkant
				}

				position: relative;
				left: var(
					--content-margin
				); // Padding verkar inte funka pga grid-template så använd position istället

				@media (max-width: 1510px) {
					margin-left: 0;
				}

				// Bredd
				max-width: calc(100vw - var(--content-margin));

				// Layout
				display: grid;
				grid-template-columns: clamp(
					500px,
					// Minsta tillåtna sidbredd
					calc(var(--content-max-width) + var(--content-margin) + 10vw + 50px),
					// Fönstrets bredd minus vänstermarginalen plus förskjutning relativ till fönstrets storlek
					calc(var(--content-max-width) + var(--content-margin) + 300px) // Max 300px förskjutning
				);

				> #hero-message {
					grid-row: 1;
					grid-column: 1;
					justify-self: start;

					> h1 {
						font-size: 50px;
						max-width: min(19ch, 80vw);
						line-height: calc(2*28px);
    					margin-bottom: calc(40px - 28px);

						@include mobile {
							// Minska avståndet upp till loggan så att Starta nu-knappen hamnar över folden
							margin-top: 0;
						}

						@media (min-width: 2400px) {
							// Vid jättebred skärm ser det bättre ut med mer luft mellan h1 och loggan
							margin-top: 70px;
						}

						> em {
							font-style: normal;
							font-family: inherit;

							@include mobile {
								background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='184' fill='none' viewBox='0 0 184 41'%3E%3Cpath fill='%23C4EBE2' fill-rule='evenodd' d='M13.576 4.796A7 7 0 008.875 16.78l-1.26.034a7 7 0 10.383 13.995l3.633-.1A7 7 0 0018 40.876l154.966-2.75a7 7 0 004.781-11.993 7 7 0 00-1.125-13.936l-7.081.194A7 7 0 00164.583.247L13.576 4.796z' clip-rule='evenodd'/%3E%3C/svg%3E%0A");
								background-repeat: no-repeat;
								background-position: center 70%;
								background-size: 102%;
							}
						}
					}

					> p {
						margin-bottom: 28px;
					}

					@include nonmobile {
						> .button {
							&::before {
								content: '';
								background-image: url("data:image/svg+xml,%3Csvg width='30' height='34' viewBox='0 0 30 34' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath fill-rule='evenodd' clip-rule='evenodd' d='M20.6855 0.524592C21.4189 0.721522 21.8539 1.47571 21.6569 2.20913L19.1609 11.5049L28.4566 14.0009C29.19 14.1978 29.625 14.952 29.428 15.6854C29.2311 16.4188 28.4769 16.8537 27.7435 16.6568L18.4478 14.1608L15.9517 23.4565C15.7548 24.1899 15.0006 24.6248 14.2672 24.4279C13.5338 24.231 13.0989 23.4768 13.2958 22.7434L15.7918 13.4476L6.49611 10.9516C5.7627 10.7547 5.32779 10.0005 5.52472 9.26708C5.72166 8.53366 6.47585 8.09876 7.20926 8.29569L16.505 10.7917L19.001 1.49598C19.1979 0.762567 19.9521 0.327662 20.6855 0.524592Z' fill='%23C2E6D7'/%3E%3Ccircle cx='2' cy='32' r='2' fill='%23C2E6D7'/%3E%3C/svg%3E");
								position: absolute;
								width: 30px;
								height: 34px;
								left: -52px;
								margin-bottom: 60px;
								background-size: 30px 34px;
							}
						}
					}
				}

				> #illustration {
					background-image: url('/illustration.svg');
					background-size: 1422px;
					background-repeat: no-repeat;
					background-position: 0% 100%;
					height: 860px;
					width: 1422px;
					max-width: 1520px;

					grid-row: 1;
					grid-column: 1;
					justify-self: end;

					position: relative;
    				right: -220px;

					@media (max-width: 1720px) {
						width: 1450px; // Gör elementet bredare så att bakgrunden flyttas längre åt vänster
						margin-top: 20px;
					}

					@media (max-width: 1620px) {
						width: calc(3005px - 100vw);
						margin-top: 30px;
					}

					@media (max-width: 1540px) {
						width: calc(2950px - 100vw);
					}

					@media (max-width: 1470px) {
						width: calc(2850px - 100vw);
						background-image: url('/illustration_1320px.svg');
						background-size: 1320px;
						margin-top: 40px;
					}

					@include mobile {
						display: none;
					}
				}
			}
		}

		&#videos {
			$wrap-breakpoint: 1205px;

			margin-top: -60px; // Så att den kommer högre upp
			align-self: center;

			width: clamp(
				500px,
				// Minsta tillåtna sidbredd
				calc(var(--content-max-width) + var(--content-margin) + 10vw + 50px),
				// Fönstrets bredd minus vänstermarginalen plus förskjutning relativ till fönstrets storlek
				calc(var(--content-max-width) + var(--content-margin) + 300px) // Max 300px förskjutning
			);
			max-width: calc(100vw - var(--content-margin));

			padding: 0 0 0 var(--content-margin);
			margin-bottom: 20px;

			@media (min-width: $wrap-breakpoint) {
				padding: 0 var(--content-margin);
				margin-bottom: 0;
			}

			align-items: flex-start;

			position: relative; // Bara eftersom :before är absolut

			&:before {
				content: '';
				background-image: url('/dot.svg');
				width: 50%;
				display: block;
				position: absolute;
				right: 0;
				border-radius: 240px;
				top: 120px;
				bottom: 100px;
			}

			> * {
				z-index: 1;
			}

			> p {
				margin-bottom: 10px;
				max-width: 50ch;
			}

			> #videos-container {
				display: flex;
				flex-direction: column;
				width: 100%;
				justify-content: end;
				position: relative;

				@media (max-width: $wrap-breakpoint) {
					margin-bottom: 20px;
				}

				@media (min-width: $wrap-breakpoint) {
					flex-direction: row;
					column-gap: clamp(40px, 3vw, 53px);
					margin-bottom: 80px;
					align-items: flex-start;
					max-width: 1081px;
					margin-left: -58px;

					&:before {
						content: '';
						position: absolute;
						width: 800px;
						height: 290px;
						left: min(306px, 22vw);
						top: 310px;

						background: #F9FBFB;
						filter: blur(100px);
					}
				}

				> .video {
					max-width: 100%;
					background: white;
					box-shadow: -20px 4px 49px -10px rgba(0, 71, 94, 0.09), 17px 0 8px -8px white; // Den vita är bara för att täcka över bakgrunden
					border-radius: 28px 0 0 28px;
					
					display: flex;
					flex-direction: column;	
					align-items: flex-start;
					gap: 22px;
					flex-wrap: wrap;
					flex: 1;

					padding: clamp(32px, 5vw, 50px);
					margin: clamp(.5em, 3vw, 1.2em) -5px;

					cursor: pointer;

					> ul {
						margin-left: 1.2em;
						opacity: .8;
						max-width: 50ch;
					}

					@media (max-width: $wrap-breakpoint) and (min-width: 900px) {
						margin-bottom: 20px;
						align-content: start;
						max-height: 470px;
						gap: 22px 45px;

						> ul {
							margin-top: 51px;
							margin-left: 0;
							max-width: 35ch;
						}
					}

					
					@media (min-width: $wrap-breakpoint) {
						border-radius: 0 0 54px 54px;
						box-shadow: 4px 4px 14px rgba(0, 71, 94, 0.01), 26px 26px 43px -10px rgba(0, 71, 94, 0.03);
						padding: 53px 57px;
						margin: 9px 0 35px;
						padding-top: 48px;
						gap: 35px;

						&:last-child {
							position: relative;

							&:after {
								content: 'Fler videor kommer snart';
								padding: 29px 49px;
								opacity: 0.71;
								box-shadow: 0px 0px 34px rgba(0, 71, 94, 0.12);
								position: absolute;
								bottom: -125px;
								right: -30px;
								border-radius: 50px;
								background-color: white;
								cursor: pointer;
								pointer-events: none;
							}
						}
					}

					&:hover {						
						> ul {
							opacity: 1;
						}

						@media (min-width: $wrap-breakpoint) {
							box-shadow: 4px 4px 14px rgba(0, 71, 94, 0.01), 26px 26px 43px -10px rgba(0, 71, 94, 0.03), 0 0 40px #19242c09;
							border-radius: 54px
						}

						> :global(.thumbnail)::before {
							background-image: url("data:image/svg+xml,%3Csvg width='42' height='59' viewBox='0 0 42 59' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M32.7486 31.9377L36.147 29.5L32.7486 27.0623L7.9986 9.30877L3.25 5.90254L3.25 11.7465L3.25 47.2535L3.25 53.0975L7.9986 49.6912L32.7486 31.9377Z' fill='%232DB08F' stroke='white' stroke-width='6'/%3E%3C/svg%3E");
						}
					}

					> h3 {
						font-size: 20px;
						position: relative;

						@media (min-width: $wrap-breakpoint) {
							font-size: 24px;
						}
						
						&:after {
							content: '';
							background: #2BC9A2;
							height: 3px;
							width: 22px;
							border-radius: 4px;
							position: absolute;
							left: 0;
							bottom: 6px;

							@media (min-width: $wrap-breakpoint) {
								height: 4px;
								width: 33px;
								bottom: 2px;
							}
						}
					}

					> button {
						@media (max-width: $wrap-breakpoint) {
							display: none;
						}

						gap: 14px;
						padding-left: 20px;

						&:before {
							content: '';
							background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 12 17' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath opacity='0.92' d='M12 8.5L0.75 16.2942L0.750001 0.705771L12 8.5Z' fill='%23434343'/%3E%3C/svg%3E%0A");
							width: 12px;
							height: 17px;
						}
					}
				}
			}
		}

		&#hub {
			position: relative; // Bara eftersom :after är absolut

			&:before {
				content: '';
				position: absolute;
				background-image: url('/blob.svg');
				background-repeat: no-repeat;
				background-position: center top;
				background-size: 3039.5px 1166px;
				height: 1066px;
				width: 100%;
				transform: rotate(180deg);
				top: 0;

				-webkit-mask-image: linear-gradient(to top, black 100px, transparent 600px);
				mask-image: linear-gradient(to top, black 100px, transparent 600px);

				opacity: .6;
				
				z-index: 1; // Så att det blurrade bakgrundselementet ovan inte täcker blobben
			}

			> * {
				z-index: 1;
			}

			p {
				max-width: 50ch;
			}

			> div {
				margin-top: 120px;

				width: clamp(
					500px,
					// Minsta tillåtna sidbredd
					calc(var(--content-max-width) + var(--content-margin) + 10vw + 50px),
					// Fönstrets bredd minus vänstermarginalen plus förskjutning relativ till fönstrets storlek
					calc(var(--content-max-width) + var(--content-margin) + 300px) // Max 300px förskjutning
				);
				max-width: calc(100vw - var(--content-margin));
				padding: 0 var(--content-margin);

				$single-col-breakpoint: 660px;
				$smaller-image-breakpoint: 1250px;

				@include mobile {
					margin-top: 84px;
				}

				@media (max-width: $single-col-breakpoint) {
					margin-top: 64px;
				}

				> .teasers {
					display: flex;
					flex-direction: row;
					gap: 43px;
					margin-top: 3em;

					@include mobile {
						margin-top: 2em;
					}
					
					> div {
						background-color: #F1F8F8;
						border-radius: 28px;
						padding: 40px 45px 45px;
						width: 334px;
						@include smooth-corners;
						

						&:first-child {
							background-color: #2f4453;

							> h3, p {
								color: #fff;
							}
						}

						&:last-child {
							@media (max-width: 760px) {
								display: none;
							}
						}

						> h3, p {
							padding: 0 0 2px;
						}

						> .button {
							margin-top: 2em;
							
							&.contrast {
								background-color: #2DB085;

								&:hover {
									background-color: darken(#2DB085, 6%);
								}
							}
						}
					}
				}

				> hr {
					margin: 8em 0 7em;

					@media (max-width: $single-col-breakpoint) {
						display: none;
					}
				}

				> .integrations {
					display: flex;
					column-gap: 70px;
					position: relative;

					@media (max-width: $single-col-breakpoint) {
						flex-wrap: wrap-reverse;
						margin-top: 60px;
					}

					@media (max-width: $smaller-image-breakpoint) {
						column-gap: 40px;
					}

					@media (max-width: 1000px) and (min-width: $single-col-breakpoint) {
						column-gap: 5px;
					}

					> * {
						z-index: 1;
					}

					> div {
						> h2 {
							max-width: 26ch;
						}	
					}

					> .image-wrapper {
						margin-top: 18px;
						width: 643px;
						max-width: calc(100vw - var(--content-margin));
						overflow: hidden;
						flex-shrink: 0;

						@media (max-width: $smaller-image-breakpoint) {
							width: 425px;
						}

						@media (max-width: 1000px) and (min-width: $single-col-breakpoint) {
							width: 385px;
							transform: translateX(30px);
						}

						@media (max-width: 800px) and (min-width: $single-col-breakpoint) {
							width: 300px;
							transform: translateX(40px);
						}

						@media (max-width: $single-col-breakpoint) {
							position: relative;
    						left: -20px;
						}
					}
				}
			}
		}
	}
</style>