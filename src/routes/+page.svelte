<script lang="ts">
	import Toggle from '../components/Toggle.svelte';
	let isSetup = false;
	let section = 1;
	let customer = { name: '', email: '', phone: '' };
	let plan = { tier: 0, yearly: false };
	let addon = { online: false, storage: false, profile: false };
	const plans = ['Arcade', 'Advanced', 'Pro'];
	const addons = ['Online service', 'Larger storage', 'Profile customisation'];
	const prices = [9, 12, 15];
	const goBack = () => {
		section -= 1;
	};
	const goNext = () => {
		if (isSetup !== true) {
			setup();
		}
		section += 1;
	};
	const setPlan = (i: number) => {
		plan.tier = i;
	};
	const setAddon = (i: number) => {
		switch (i) {
			case 3:
				addon.online = !addon.online;
				break;
			case 4:
				addon.storage = !addon.storage;
				break;
			case 5:
				addon.profile = !addon.profile;
				break;
		}
	};
	const setup = () => {
		for (let i = 0; i < 3; i++) {
			document.getElementsByClassName('option')[i].addEventListener('click', () => {
				setPlan(i);
			});
		}
		for (let i = 3; i < 6; i++) {
			document.getElementsByClassName('option')[i].addEventListener('click', () => {
				setAddon(i);
			});
		}
		console.log('setup complete');
	};
</script>

<div class="root">
	<div class="sidebar">
		<div class="nav">
			<div class="section {section === 1 ? 'active' : ''}">YOUR INFO</div>
			<div class="section {section === 2 ? 'active' : ''}">SELECT PLAN</div>
			<div class="section {section === 3 ? 'active' : ''}">ADD-ONS</div>
			<div class="section {section === 4 ? 'active' : ''}">SUMMARY</div>
		</div>
	</div>
	<div class="panel {section === 1 ? 'visible' : 'hidden'}">
		<h2>Personal info</h2>
		<div class="info">Please provide your name, email address, and phone number.</div>
		<div class="wrapper">
			<div class="wrapper">
				<div class="label">Name</div>
				<input type="text" placeholder="e.g. Stephen King" />
			</div>
			<div class="wrapper">
				<div class="label">Email Address</div>
				<input type="text" placeholder="e.g. stephenking@lorem.com" />
			</div>
			<div class="wrapper">
				<div class="label">Phone Number</div>
				<input type="text" placeholder="e.g. +1 234 567 890" />
			</div>
		</div>
	</div>
	<div class="panel {section === 2 ? 'visible' : 'hidden'}">
		<h2>Select your plan</h2>
		<div class="info">You have the option of monthly or yearly billing.</div>
		<div class="option-group {plan.yearly ? 'yearly' : ''}">
			<div class="option {plan.tier === 0 ? 'active' : ''}" data-plan="0">
				<img src="/images/icon-arcade.svg" alt="Arcade Icon" />
				<div class="title">Arcade</div>
				<div class="price">{plan.yearly ? '$90/yr' : '$9/mo'}</div>
				<div class="offer">2 months free</div>
			</div>
			<div class="option {plan.tier === 1 ? 'active' : ''}">
				<img src="/images/icon-advanced.svg" alt="Advanced Icon" />
				<div class="title">Advanced</div>
				<div class="price">{plan.yearly ? '$120/yr' : '$12/mo'}</div>
				<div class="offer">2 months free</div>
			</div>
			<div class="option {plan.tier === 2 ? 'active' : ''}">
				<img src="/images/icon-pro.svg" alt="Pro Icon" />
				<div class="title">Pro</div>
				<div class="price">{plan.yearly ? '$150/yr' : '$15/mo'}</div>
				<div class="offer">2 months free</div>
			</div>
		</div>
		<Toggle labels={['Monthly', 'Yearly']} bind:value={plan.yearly} />
	</div>
	<div class="panel {section === 3 ? 'visible' : 'hidden'}">
		<h2>Pick add-ons</h2>
		<div class="info">Add-ons help enhance your gaming experience.</div>
		<div class="option-group">
			<div class="option {addon.online ? 'active' : ''}">
				<input type="checkbox" name="online" id="online" bind:checked={addon.online} />
				<div class="title">Online service</div>
				<div class="info">Access to multiplayer games</div>
				<div class="price">{plan.yearly ? '+$10/yr' : '+$1/mo'}</div>
			</div>
			<div class="option {addon.storage ? 'active' : ''}">
				<input type="checkbox" name="storage" id="storage" bind:checked={addon.storage} />
				<div class="title">Larger storage</div>
				<div class="info">Extra 1TB of cloud save</div>
				<div class="price">{plan.yearly ? '+$20/yr' : '+$2/mo'}</div>
			</div>
			<div class="option {addon.profile ? 'active' : ''}">
				<input type="checkbox" name="profile" id="profile" bind:checked={addon.profile} />
				<div class="title">Customizable Profile</div>
				<div class="info">Custom theme on your profile</div>
				<div class="price">{plan.yearly ? '+$20/yr' : '+$2/mo'}</div>
			</div>
		</div>
	</div>
	<div class="panel {section === 4 ? 'visible' : 'hidden'}">
		<h2>Finishing up</h2>
		<div class="info">Double-check everything looks OK before confirming.</div>
		<div class="bill">
			<div class="plan">
				<div class="title">
					{plans[plan.tier]} ({plan.yearly ? 'Yearly' : 'Monthly'})
				</div>
				<div class="info"><a href="#top">Change</a></div>
				<div class="price">
					${plan.yearly ? prices[plan.tier] * 10 : prices[plan.tier]}/{plan.yearly ? 'yr' : 'mo'}
				</div>
			</div>
			{#if addon.online || addon.storage || addon.profile}
				<hr />
			{/if}
			{#if addon.online}
				<div class="item">{addons[0]}</div>
				<div class="price">+${plan.yearly ? '10/yr' : '1/mo'}</div>
			{/if}
			{#if addon.storage}
				<div class="item">{addons[1]}</div>
				<div class="price">+${plan.yearly ? '20/yr' : '2/mo'}</div>
			{/if}
			{#if addon.profile}
				<div class="item">{addons[2]}</div>
				<div class="price">+${plan.yearly ? '20/yr' : '2/mo'}</div>
			{/if}
		</div>
		<div class="total">
			<div class="item">Total ({plan.yearly ? 'per year' : 'per month'})</div>
			<div class="price">
				${plan.yearly
					? (prices[plan.tier] +
							(addon.online ? 1 : 0) +
							(addon.storage ? 2 : 0) +
							(addon.profile ? 2 : 0)) *
							10 +
					  '/yr'
					: prices[plan.tier] +
					  (addon.online ? 1 : 0) +
					  (addon.storage ? 2 : 0) +
					  (addon.profile ? 2 : 0) +
					  '/mo'}
			</div>
		</div>
	</div>
	<div class="panel thanks {section === 5 ? 'visible' : 'hidden'}">
		<img src="/images/icon-thank-you.svg" alt="Thanks" />
		<h2>Thank you!</h2>
		<div class="info">
			Thanks for confirming your subscription! We hope you have fun using our platform. If you ever
			need support, please feel free to email us at support@loremgaming.com.
		</div>
	</div>
	<div class="back-next {section !== 5 ? 'visible' : 'hidden'}">
		<button on:click={goBack} class={section !== 1 ? 'visible' : 'hidden'}>Go Back</button>
		<div class="spacer" />
		<button on:click={goNext} class="active">Next Step</button>
	</div>
</div>
<div class="attribution {section === 5 ? 'visible' : 'hidden'}">
	Challenge by <a
		href="https://www.frontendmentor.io?ref=challenge"
		target="_blank"
		rel="noreferrer">Frontend Mentor</a
	>.<br />Coded by <a href="https://www.gihub.com/richardhosler">Richard Hosler</a>.
</div>

<style>
	.attribution {
		position: absolute;
		bottom: 0;
		width: 100%;
		text-align: center;
	}
	.info {
		color: var(--cool-gray);
	}
	.total {
		display: grid;
		grid-template-columns: 1fr 0.5fr;
		font-size: 0.9rem;
		margin: 0 1rem;
	}
	.total > .price {
		font-size: 1rem;
		font-weight: 700;
		color: var(--purplish-blue);
		text-align: right;
	}
	hr {
		border: none;
		border-bottom: 1px solid var(--light-gray);
		width: 100%;
		grid-column: span 2;
	}
	.bill {
		display: grid;
		place-content: justify;
		background-color: var(--magnolia);
		padding: 1rem;
		border-radius: 0.5rem;
		gap: 0.5rem;
		grid-template-columns: 1fr 0fr;
	}
	.bill > .item {
		font-size: 0.9rem;
		color: var(--cool-gray);
	}
	.bill > .price {
		font-size: 0.9rem;
	}
	.bill > .plan {
		display: grid;
		place-content: center;
		grid-template-columns: 1fr 0fr;
		grid-template-rows: 1.2rem 1.2rem;
		grid-column: span 2;
	}
	.bill > .plan > .title {
		font-weight: 500;
		font-size: 0.9rem;
		grid-column: 1;
	}
	.bill > .plan > .info {
		font-size: 0.9rem;
		color: var(--cool-gray);
		grid-column: 1;
	}
	.bill > .plan > .price {
		grid-column: 2;
		grid-row: 1 / span 2;
		font-weight: 700;
		font-size: 0.9rem;
		align-self: center;
	}
	.option.active {
		border-color: var(--purplish-blue);
		background-color: var(--magnolia);
	}
	.option > input {
		grid-column: 1;
		appearance: none;
		border: none;
		margin: 0;
		grid-row: 1 / span 2;
	}
	.option:has(input) {
		display: grid;
		grid-template-columns: 1.5rem 1fr 2.5rem;
		grid-template-rows: 0.8rem 1.1rem;
	}
	.option > input ~ .info {
		font-size: 0.75rem;
	}
	.option > input ~ .title {
		font-size: 0.9rem;
	}
	.option > input ~ .price {
		color: var(--purplish-blue);
		font-size: 0.75rem;
		grid-column: 3;
		grid-row: 1 / span 2;
		place-self: center;
	}
	.option > input::before {
		position: relative;
		content: ' ';
		display: block;
		left: -0.75rem;
		width: 1.2rem;
		height: 1.2rem;
		border: solid 1px var(--light-gray);
		border-radius: 0.3rem;
		transition: all 200ms ease, visibility 0ms;
	}
	.option > input:checked::before {
		background-color: var(--purplish-blue);
		border: inset 1px var(--light-gray);
		display: block;
		color: var(--alabaster);
		font-size: 1.4rem;
		line-height: 1rem;
		text-align: center;
		content: '\2713';
	}
	.option {
		padding: 0.9rem;
		border: solid var(--light-gray) 1px;
	}
	.option > .offer {
		font-size: 0.75rem;
		font-weight: 500;
		opacity: 0;
		transition: all 150ms;
	}
	.yearly > .option > .offer {
		opacity: 1;
	}
	.nav > .section.active::after {
		background-color: var(--light-blue);
		color: var(--marine-blue);
		border: solid var(--light-blue) 1px;
	}
	a {
		color: var(--cool-gray);
	}
	h2 {
		margin: 0;
	}
	.label {
		font-size: 0.8rem;
		color: var(--marine-blue);
	}
	input {
		padding: 0.6rem 0.8rem;
		font-weight: 500;
		color: var(--marine-blue);
		border-radius: 0.2rem;
		border: 1px solid var(--light-gray);
		width: 95%;
		font-size: 0.9rem;
	}
	input::placeholder {
		color: var(--cool-gray);
		opacity: 1;
	}
	input[type='text']:focus {
		border: 1px solid var(--purplish-blue);
		outline: none;
	}
	.attribution {
		visibility: hidden;
		opacity: 0;
	}
	.visible {
		visibility: visible;
		opacity: 1;
		transition: all 200ms 200ms, visibility 0ms 0ms;
	}
	.hidden {
		visibility: hidden;
		opacity: 0;
		transition: all 200ms 0ms, visibility 0ms 200ms;
	}
	@media (max-width: 899px) {
		.option {
			border-radius: 0.3rem;
			margin-bottom: 0.75rem;
			display: grid;
			gap: 0.1rem 0.8rem;
			place-items: center left;
			transition: height 150ms 200ms ease-out;
		}
		.option > .offer {
			grid-column: 2;
		}
		.yearly > .option {
			height: 67.2px;
			transition: all 150ms ease-in;
		}
		.option:has(img) {
			height: 41.6px;
			grid-template-columns: 3.5rem 1fr;
			grid-template-rows: 1.2rem 1.3rem;
		}
		.option > .title {
			font-weight: 500;
			grid-column: 2;
		}
		.option > .info {
			font-size: 0.8rem;
			grid-column: 2;
		}
		.option > img {
			grid-column: 1;
			grid-row: 1 / span 2;
		}
		.option > img ~ .price {
			font-size: 0.86rem;
			color: var(--cool-gray);
			grid-column: 2;
			grid-row: 2;
		}
		.sidebar {
			background-image: url(/images/bg-sidebar-mobile.svg);
			background-size: cover;
			height: 10.75rem;
		}
		.back-next {
			position: absolute;
			bottom: 0;
			display: grid;
			grid-template-columns: 8rem 1fr 8rem;
			width: 100%;
			justify-content: center;
			align-content: center;
			align-items: center;
			background-color: var(--alabaster);
		}
		.nav {
			position: absolute;
			left: 50%;
			transform: translateX(-50%);
			padding: 0;
			margin: 0;
			gap: 1rem;
			display: grid;
			grid-template-columns: repeat(4, 2rem);
			color: transparent;
			counter-reset: c;
		}
		.nav > .section::after {
			counter-increment: c;
			display: block;
			content: counter(c);
			position: absolute;
			top: 0;
			color: var(--alabaster);
			border: solid 1px var(--alabaster);
			padding: 0.4rem;
			text-align: center;
			top: 2rem;
			border-radius: 1rem;
			width: 1rem;
			height: 1rem;
			font-weight: bold;
			font-size: 0.9rem;
			line-height: 1rem;
		}
		.panel {
			position: absolute;
			display: grid;
			width: 78vw;
			top: 6.2rem;
			left: 50%;
			transform: translateX(-50%);
			border-radius: 0.5rem;
			background-color: white;
			padding: 2rem 1.6rem;
			gap: 1.2rem;
		}
		.panel.thanks {
			gap: 0.5rem;
			padding: 6rem 1.3rem;
			text-align: center;
			font-size: 0.95rem;
		}
		.panel.thanks > img {
			place-self: center;
			width: 55px;
			margin: 1rem;
		}
		button {
			padding: 0.6rem;
			color: var(--cool-gray);
			font-weight: 500;
			font-size: 0.8rem;
			border-style: hidden;
			border-radius: 0.2rem;
			background-color: var(--alabaster);
			margin: 1rem;
		}
		button.active {
			background-color: var(--marine-blue);
			color: var(--alabaster);
		}
	}
	@media (min-width: 900px) {
		.root {
			position: absolute;
			left: 50%;
			top: 50%;
			transform: translateX(-50%) translateY(-50%);
			display: grid;
			grid-template-columns: 20rem 1fr;
			background-color: white;
			border-radius: 1rem;
		}
		.sidebar {
			position: relative;
			background-image: url('/images/bg-sidebar-desktop.svg');
			background-size: cover;
			margin: 1rem;
			border-radius: 1rem;
			counter-reset: c;
			display: flex;
			gap: 1rem;
			padding: 2rem;
			padding-left: 5rem;
			grid-row: 1 / span 2;
		}
		.nav {
			display: flex;
			flex-direction: column;
			gap: 2rem;
		}
		.nav > .section {
			position: relative;
			display: grid;
			font-weight: 700;
			font-size: 0.9rem;
			letter-spacing: 0.1rem;
			color: var(--alabaster);
			place-content: center left;
			line-height: 1rem;
		}
		.nav > .section::before {
			counter-increment: c;
			content: 'STEP ' counter(c);
			display: block;
			font-size: 0.8rem;
			font-weight: 400;
			letter-spacing: 0;
		}
		.nav > .section::after {
			position: absolute;
			left: -3rem;
			content: counter(c);
			color: var(--alabaster);
			border: 1px solid var(--alabaster);
			width: 1.9rem;
			height: 1.5rem;
			padding-top: 0.4rem;
			padding-left: 0.05rem;
			border-radius: 2rem;
			text-align: center;
			transition: all 200ms;
		}
		.panel {
			grid-row: 1;
			grid-column: 2;
			padding: 3rem 5rem;
			display: grid;
			gap: 2rem 1rem;
			grid-template-rows: 1rem 2rem 1fr 3rem 0.1fr;
			height: 80%;
		}
		.panel > .info {
			color: var(--cool-gray);
			margin-bottom: 1rem;
		}
		.option-group {
			display: grid;
			grid-template-columns: 1fr 1fr 1fr;
			gap: 1rem;
		}
		.option-group:has(input) {
			grid-template-columns: 1fr;
			grid-template-rows: repeat(3, 4rem);
		}
		.option:has(img) {
			display: grid;
			grid-template-columns: 1fr;
			grid-template-rows: 1fr;
			border-radius: 0.5rem;
		}
		.option > .title {
			font-weight: 500;
		}
		.back-next {
			position: absolute;
			display: grid;
			grid-template-columns: 1fr 2fr 1fr;
			padding: 2rem;
			right: 2rem;
			bottom: 0;
		}
		button {
			font-size: 0.9rem;
			font-weight: 700;
			padding: 1rem 2rem;
			border: none;
			border-radius: 0.5rem;
			background: none;
			color: var(--marine-blue);
		}
		button.active {
			background-color: var(--marine-blue);
			color: var(--alabaster);
		}
		h2 {
			font-size: 2rem;
		}
		.option > img ~ .price {
			font-size: 0.86rem;
			color: var(--cool-gray);
		}
		.option {
			border-radius: 0.5rem;
		}
		.panel.thanks {
			display: grid;
			text-align: center;
			grid-template-columns: 1fr;
			grid-template-rows: 0.5fr 2rem 4rem 0.1fr;
			padding: 5rem;
		}
		.panel.thanks > img {
			position: relative;
			margin: auto;
		}
	}
</style>
