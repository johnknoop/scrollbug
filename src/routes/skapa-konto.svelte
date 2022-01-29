<svelte:head>
	<title>Skapa konto - foobar</title>
	<script src="https://www.google.com/recaptcha/api.js" async defer></script>
	<link href="https://fonts.googleapis.com/css2?family=Mulish:wght@800&display=swap" rel="stylesheet">
</svelte:head>

<script lang="ts">
	import { browser } from '$app/env';
	import { onDestroy, onMount } from 'svelte';

	let companyName = '';
	let firstName = '';
	let lastName = '';
	let email = '';

	let successful = false;
	let errorResponse = false as string | boolean;
	let loading = false;
	let submitted = false;

	const emailPattern = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;

	const validateEmail = (email) => {	
		return emailPattern.test(String(email).toLowerCase());
	}

	const reset = () => {
		successful = false;
		errorResponse = false;
		loading = false;
		submitted = false;

		grecaptcha.reset();
	}

	const validate = (e) => {
		e.preventDefault();

		submitted = true;

		document.forms[0]['email'].setCustomValidity('')

		const valid = document.forms[0]['companyName'].checkValidity() &&
			document.forms[0]['firstName'].checkValidity() &&
			document.forms[0]['lastName'].checkValidity() &&
			document.forms[0]['email'].checkValidity();

		if (!valid) {
			return;
		}

		if (!validateEmail(email)) {
			document.forms[0]['email'].setCustomValidity("Felaktig e-postadress")
			return;
		}

		grecaptcha.execute();
	}

	const submit = async () => {
		loading = true;

		var response = await fetch('https://foobar.io/public-api/manage/create-organization', {
			method: 'POST',
			headers: {
				'Content-Type': 'application/json'
			},
			body: JSON.stringify({
				"name": companyName,
				"firstUserEmail": email,
				"firstUserFirstName": firstName,
				"firstUserLastName": lastName,
				captchaToken: grecaptcha.getResponse()
			})
		});

		loading = false;

		if (response.status === 200) {
			successful = true;
		} else {
			switch (response.status) {
				case 409:
					errorResponse = {
						'User exists': 'E-postadressen har redan används för att skapa ett konto i foobar',
						'Org exists': 'Ett företag med det namnet har redan skapat ett konto'
					}[await response.text()]
					break;
			
				case 400:
					const error = await response.text();

					if (error === 'Bad captcha token') {
						errorResponse = "Din captcha-token kunde inte valideras. Om du är säker på att du inte är en robot så kontakta oss så ska vi undersöka detta på omedelbart."
					} else {
						errorResponse = "Det verkar som att det saknas information. Vi jobbar på att undersöka detta. Vi får tyvärr be dig pröva igen om en liten stund."
					}

					break;

				default:
					errorResponse = "Ett fel inträffade. Vi jobbar på att undersöka detta. Vi får tyvärr be dig pröva igen om en liten stund."
					break;
			}
		}

		return false;
	};

	onMount(() => {
		window['submit'] = submit;
	})

	onDestroy(() => {
		if (browser) {
			window['submit'] = null;
		}
	})
</script>

<style lang="scss" scoped>
	@import '../lib/mixins';
	@import '../lib/forms';

	.g-recaptcha {
		display: none;
	}

	article {
		> section {
			align-items: start;

			> article {
				> .early-access {
					background-color: #ECF8F7;
					border-radius: 13px;
					padding: 22px 26px 10px;
					margin: 1.5em 0 2.5em;

					@include nonmobile {
						display: none;
					}
				}
			}

			> aside {
				@include smooth-corners(14);

				@include mobile {
					display: none;
				}

				background-color: #ECF8F7;
				border-radius: 53px;
				padding: 50px;
				flex-basis: 615px;
				flex-shrink: 3;

				transition: opacity .2s;

				&.hidden {
					opacity: 0;
				}
			}

			.result {
				padding: 40px;
				margin: 58px 0;
				border-radius: 40px;
				max-width: 50ch;
				@include smooth-corners(14);

				&.success {
					background-color: #ECF8F7;
				}

				&.error {
					background-color: #f9f1f1;
				}
			}

			.button-group {
				margin-top: 45px;

				> button {
					width: 120px;
				}
			}
		}
	}
</style>


<article>
	<h1>Skapa konto</h1>
	<section>
		<article>
			<p>Det kostar ingenting, men om det går bra så kanske vi kontaktar er för att lyssna till era tankar kring produkten.</p>
			<p>Fyll i era uppgifter nedan så skickas ett aktiveringsmail automatiskt.</p>
			
			<div class="early-access">
				<h4>Early access</h4>
				<p>Ni kan börja använda foobar redan idag, men vi arbetar fortfarande på att integrera Fortnox för automatisk bokföring.</p>
			</div>

			{ #if !successful && !errorResponse }
				<form novalidate on:submit={validate} class:submitted={submitted} >
					<section>
						<label>
							<span>Företag</span>
							<input
								placeholder="Företagsnamn"
								minlength="3"
								required="yes"
								type="text"
								name="companyName"
								size="22"
								bind:value={companyName} />
						</label>
					</section>
					<section>
						<label>
							<span>Dina uppgifter</span>
							<input
								placeholder="Förnamn"
								minlength="2"
								required="yes"
								type="text"
								name="firstName"
								size="17"
								bind:value={firstName} />
						</label>
						<label>
							<input
								placeholder="Efternamn"
								minlength="2"
								required="yes"
								type="text"
								name="lastName"
								size="20"
								bind:value={lastName} />
						</label><br>
						<label>
							<input
								placeholder="E-post"
								type="email"
								required="yes"
								name="email"
								size="30"
								bind:value={email} />
						</label>
					</section>

					<div class="button-group">
						<button type="submit" class="button contrast has-loader" class:loading={loading} disabled="{loading}">
							<svg width="20" height="20">
								<style>
									@keyframes uaded18c0 {
										0% {
											stroke-dashoffset: 0;
										}
										100% {
											stroke-dashoffset: -145px;
										} 
									}
									
									circle {
										stroke: white;
										stroke-width: 2px;
										stroke-dasharray: 45px 100px;
									
										animation: uaded18c0 1.4s 0s;
										animation-timing-function: ease;
										animation-iteration-count: infinite;
										animation-fill-mode: both;
									}
								</style>
								<circle cx="10" cy="10" r="9" fill="none"></circle>
							</svg>
							<span>Fortsätt</span>
						</button>
					</div>
					<div class="g-recaptcha"
						data-sitekey="6Lejf8EZAAAAACU9CSwr-VOzqUFBxFgQWs273JvT"
						data-callback="submit"
						data-size="invisible">
					</div>
				</form>
			{/if}
			{#if successful}
				<p class="result success">
					Ett e-postmeddelande har skickats till {email}
				</p>
			{/if}
			{#if errorResponse}
				<p class="result error">{errorResponse}</p>
				<button type="button" on:click={() => reset()} class="button contrast" class:loading={loading} disabled="{loading}">Försök igen</button>
			{/if}
		</article>
		<aside class:hidden={successful || errorResponse}>
			<h3>Early access</h3>
			<p>Ni kan börja använda foobar redan idag, men vi arbetar fortfarande på att integrera Fortnox för automatisk bokföring.</p>
			<p>Väljer ni att köra igång under EAP-perioden så får ni sedan fortsätta utan kostnad i minst ett år.</p>
		</aside>
	</section>
</article>