<script lang="ts">
	import { to_number } from 'svelte/internal';
	import Toggle from '../../components/Toggle.svelte';

	const sections = [
		{
			name: 'YOUR INFO',
			title: 'Personal info',
			info: 'Please provide your name, email address, and phone number.'
		},
		{
			name: 'SELECT PLAN',
			title: 'Select your plan',
			info: 'You have the option of monthly or yearly billing.'
		},
		{
			name: 'ADD-ONS',
			title: 'Pick add-ons',
			info: 'Add-ons help enhance your gaming experience.'
		},
		{
			name: 'SUMMARY',
			title: 'Finishing up',
			info: 'Double-check everything looks OK before confirming.'
		}
	];
	const plans = [
		{ name: 'Arcade', price: 9, icon: '/images/icon-arcade.svg' },
		{ name: 'Advanced', price: 12, icon: '/images/icon-advanced.svg' },
		{ name: 'Pro', price: 15, icon: '/images/icon-pro.svg' }
	];
	const addons = [
		{ name: 'Online service', info: 'Access to multiplayer games', price: 1 },
		{ name: 'Larger storage', info: 'Extra 1TB of cloud save', price: 2 },
		{ name: 'Profile customisation', info: 'Custom theme on your profile', price: 2 }
	];
	const goBack = () => {
		section -= 1;
	};
	const goNext = () => {
		if (isSetup !== true) Setup();
		if (section === 0) if (!Validate()) return;
		customer.total = Total();
		section += 1;
		if (section === 4)
			sections.push({
				name: 'THANKS',
				title: 'Thanks',
				info: 'Thanks for confirming your subscription! We hope you have fun using our platform. If you ever need support, please feel free to email us at support@loremgaming.com.'
			});
	};
	const setPlan = (i: number) => {
		customer.plan = i;
	};
	const setAddon = (i: number) => {
		console.log('changed addon ' + i + ' from ' + customer.addons[i]);

		customer.addons[i] = !customer.addons[i];
		console.log(' to ' + customer.addons[i]);
	};
	const Validate = () => {
		const form = document.getElementsByTagName('input');
		let error = false;
		for (const input of form) {
			if (input.value === '') {
				input.classList.contains('error') ? null : (input.className += ' error');
				error = true;
			} else {
				input.classList.remove('error');
			}
		}
		// error = false; // DEBUG: forces next button to work
		return !error;
	};
	const Total = () => {
		let t = plans[customer.plan].price;
		for (let i = 0; i < customer.addons.length; i++) {
			if (customer.addons[i]) t += addons[i].price;
		}
		console.log('Total calculated');

		return customer.yearly ? t * 10 : t;
	};
	const Setup = () => {
		for (let i = 0; i < 3; i++) {
			document.getElementsByClassName('plan')[i].addEventListener('click', () => {
				setPlan(i);
			});
		}
		for (let i = 0; i < 3; i++) {
			document.getElementsByClassName('addon')[i].addEventListener('click', () => {
				setAddon(i);
			});
		}
		isSetup = true;
		console.log('setup complete');
	};

	let customer = {
		name: '',
		email: '',
		phone: '',
		plan: 0,
		yearly: false,
		addons: [false, false, false],
		total: 0
	};
	let isSetup = false;
	let section = 0;
</script>

<div class="root">
	<div class="nav">
		{#each sections as s, i}
			<div class="section {section === i ? 'active' : ''}">{s.name}</div>
		{/each}
	</div>
	<div class="panel {section === 0 ? 'visible' : 'hidden'}">
		<div class="head">
			<div>Personal info</div>
			<div>Please provide your name, email address, and phone number.</div>
		</div>
		<div class="body">
			<form action="#">
				<label for="name">Name</label>
				<input type="text" bind:value={customer.name} placeholder="e.g. Stephen King" />
				<label for="email">Email</label>
				<input type="text" bind:value={customer.email} placeholder="e.g. stephenking@lorem.com" />
				<label for="phone">Phone </label>
				<input type="text" bind:value={customer.phone} placeholder="e.g. +1 234 567 890" />
			</form>
		</div>
	</div>
	<div class="panel {section === 1 ? 'visible' : 'hidden'}">
		<div class="head">
			<div>Select your plan</div>
			<div>You have the option of monthly or yearly billing.</div>
		</div>
		<div class="body">
			{#each plans as plan, i}
				<div class="plan option {customer.plan === i ? 'active' : ''}" data-plan="0">
					<img src={plan.icon} alt="Icon" />
					<div class="title">{plan.name}</div>
					<div class="price">
						{customer.yearly ? '$' + plan.price * 10 + '/yr' : '$' + plan.price + '/mo'}
					</div>
					<div class="offer {customer.yearly ? 'visible' : 'hidden'}">2 months free</div>
				</div>
			{/each}
			<div>
				<Toggle labels={['Monthly', 'Yearly']} bind:value={customer.yearly} />
			</div>
		</div>
	</div>
	<div class="panel {section === 2 ? 'visible' : 'hidden'}">
		<div class="head">
			<div>Pick add-ons</div>
			<div>Add-ons help enhance your gaming experience.</div>
		</div>
		<div class="body">
			{#each addons as addon, i}
				<div class="addon option {customer.addons[i] ? 'active' : ''}">
					<div class="checkbox" />
					<div class="title">{addon.name}</div>
					<div class="info">{addon.info}</div>
					<div class="price">
						{customer.yearly ? '+$' + addon.price * 10 + '/yr' : '+$' + addon.price + '/mo'}
					</div>
				</div>
			{/each}
		</div>
	</div>
	<div class="panel {section === 3 ? 'visible' : 'hidden'}">
		<div class="head">
			<div>Finishing up</div>
			<div>Double-check everything looks OK before confirming.</div>
		</div>
		<div class="body bill">
			<div class="plan">
				<div class="title">
					{plans[customer.plan].name} ({customer.yearly ? 'Yearly' : 'Monthly'})
				</div>
				<div class="info"><a href="#top">Change</a></div>
			</div>
			<div class="price">
				${customer.yearly
					? plans[customer.plan].price * 10
					: plans[customer.plan].price}/{customer.yearly ? 'yr' : 'mo'}
			</div>
			{#if customer.addons[0] || customer.addons[1] || customer.addons[2]}
				<hr />
			{/if}
			{#each addons as addon, i}
				{#if customer.addons[i]}
					<div class="item">{addons[i].name}</div>
					<div class="price">
						+${customer.yearly ? addons[i].price * 10 + '/yr' : addons[i].price + '/mo'}
					</div>
				{/if}
			{/each}
		</div>
		<div class="total">
			<div class="item">Total ({customer.yearly ? 'per year' : 'per month'})</div>
			<div class="price">${customer.total}{customer.yearly ? '/yr' : '/mo'}</div>
		</div>
	</div>
	<div class="thanks panel {section === 4 ? 'visible' : 'hidden'}">
		<img src="/images/icon-thank-you.svg" alt="Thanks" />
		<div class="title">Thank you!</div>
		<div class="info">
			Thanks for confirming your subscription! We hope you have fun using our platform. If you ever
			need support, please feel free to email us at support@loremgaming.com.
		</div>
	</div>
	<div class="back-next {section !== 4 ? 'visible' : 'hidden'}">
		<button on:click={goBack} class={section !== 0 ? 'visible' : 'hidden'}>Go Back</button>
		{#if section === 3}
			<button class="confirm" on:click={goNext}>Confirm</button>
		{:else if section < 5}
			<button on:click={goNext}>Next Step</button>
		{/if}
	</div>
</div>

<style>
	hr {
		grid-column: 1 / span 2;
		width: 100%;
		border: none;
		border-bottom: 1px solid var(--light-grey);
		height: 0;
		margin: 0;
	}
	.total {
		display: grid;
		grid-template-columns: 1fr 0fr;
		padding: 1rem 2rem;
	}
	.total > .price {
		grid-column: 2;
		color: var(--purplish-blue);
		font-size: 1.3rem;
		font-weight: 700;
	}
	a {
		color: var(--cool-grey);
	}
	.root {
		position: absolute;
		display: grid;
		grid-template-columns: 0fr 1fr;
		grid-template-rows: 1fr 4rem;
		background-color: white;
		border-radius: 1rem;
		padding: 1rem;
		width: 900px;
		left: 50%;
		top: 50%;
		transform: translateX(-50%) translateY(-50%);
		transition: all 200ms, translate 0ms, scale 0ms;
	}
	.nav {
		display: grid;
		grid-template-rows: repeat(4, 2rem) 1fr;
		background-image: url('/images/bg-sidebar-desktop.svg');
		background-size: cover;
		background-position: top;
		border-radius: 0.5rem;
		grid-row: span 2;
		color: white;
		font-size: 0.9rem;
		font-weight: 700;
		gap: 2rem;
		counter-reset: s;
		line-height: 1.1rem;
		min-height: 24rem;
		max-width: 15rem;
	}
	.section {
		min-width: max-content;
		margin: 3rem 5rem;
		counter-increment: s;
		user-select: none;
	}
	.section::before {
		content: 'STEP ' counter(s);
		display: block;
		font-weight: 400;
		font-size: 0.8rem;
		color: var(--light-gray);
	}
	.section::after {
		position: absolute;
		border: 1px white solid;
		content: counter(s);
		left: 3rem;
		transform: translateY(-50%);
		width: 1.9rem;
		border-radius: 2rem;
		text-align: center;
		height: 1.6rem;
		padding-top: 0.3rem;
	}
	.section.active::after {
		background-color: var(--light-blue);
		color: var(--marine-blue);
		border-color: var(--light-blue);
	}
	.panel {
		display: grid;
		margin: 4rem 5rem;
		gap: 1rem;
		grid-row: 1;
		grid-column: 2;
		grid-template-rows: 0fr 0fr;
		z-index: 5;
	}
	.panel > .head > :first-child {
		font-weight: 700;
		font-size: 2rem;
		padding-bottom: 1rem;
	}
	.panel > .head > :nth-child(2) {
		font-size: 0.9rem;
		color: var(--cool-grey);
	}
	form {
		display: grid;
		line-height: 2rem;
	}
	form > label {
		display: block;
		font-size: 0.9rem;
		margin-top: 1rem;
	}
	input[type='text'] {
		font-size: 1rem;
		font-weight: 500;
		padding: 0.8rem;
		border-radius: 0.5rem;
		border: 1px solid var(--light-grey);
		min-width: 0px;
	}
	input[type='text']:focus {
		border: 1px solid var(--purplish-blue);
		outline: none;
	}
	input[type='text']::placeholder {
		color: var(--cool-grey);
	}
	input[type='text']:global(.error) {
		border-color: var(--strawberry-red);
	}
	.panel > .body {
		display: grid;
		gap: 1rem;
	}
	.panel > .body:has(.plan.option) {
		gap: 2rem 1rem;
		grid-template-columns: repeat(3, 1fr);
		padding-top: 1rem;
	}
	.panel > .body:has(.plan.option) > :last-child {
		grid-column: 1 / span 3;
	}
	.option {
		display: grid;
		border: 1px solid var(--light-grey);
		border-radius: 0.5rem;
		padding: 1rem;
		cursor: default;
		user-select: none;
	}
	.addon.option {
		grid-template-columns: 0fr 1fr 0fr;
		grid-template-rows: 0fr 0fr;
		gap: 0 1.5rem;
		padding: 1rem 1.5rem;
	}
	.addon.option > .checkbox {
		grid-row: 1 / span 2;
	}
	.checkbox {
		position: relative;
		display: block;
		border: 1px solid var(--light-grey);
		border-radius: 0.3rem;
		width: 1.2rem;
		height: 1.2rem;
		place-self: center;
	}
	.active > .checkbox {
		border-color: var(--purplish-blue);
		background-color: var(--purplish-blue);
		background-image: url('/images/icon-checkmark.svg');
		background-size: 0.85rem;
		background-position: 2px 5px;
		background-repeat: no-repeat;
	}
	.addon.option > .price {
		grid-column: 3;
		grid-row: 1 / span 2;
		align-self: center;
		color: var(--purplish-blue);
		font-size: 0.9rem;
	}
	.info {
		font-size: 0.9rem;
		color: var(--cool-grey);
	}
	.plan.option {
		grid-template-rows: 1fr 0fr 0fr 0fr;
	}
	.plan.option > :first-child {
		padding-bottom: 2.5rem;
	}
	.option:hover {
		border-color: var(--purplish-blue);
	}
	.option.active {
		border-color: var(--purplish-blue);
		background-color: var(--magnolia);
	}
	.title {
		font-weight: 500;
		font-size: 1rem;
	}
	.plan.option > .price {
		font-size: 0.9rem;
		color: var(--cool-grey);
	}
	.plan.option > .offer {
		font-size: 0.8rem;
	}
	.plan.option > .offer.hidden {
		height: 0;
	}
	.bill {
		display: grid;
		background-color: var(--magnolia);
		padding: 1.5rem;
		border-radius: 1rem;
		grid-template-columns: 1fr 0fr;
		font-size: 0.9rem;
	}
	.bill > .price {
		grid-column: 2;
	}
	.bill > .plan + .price {
		font-weight: 500;
		place-self: center;
	}
	.item {
		color: var(--cool-grey);
	}
	.back-next {
		display: grid;
		grid-template-columns: 1fr 1fr;
		padding: 0.5rem 2rem;
		padding-left: 0;
		gap: 40%;
	}
	.back-next > button {
		font-size: 1rem;
		font-weight: 500;
		max-width: 8rem;
	}
	.back-next > :first-child {
		background: none;
		border: none;
		color: var(--cool-grey);
	}
	.back-next > :nth-child(2) {
		background-color: var(--marine-blue);
		color: white;
		border: none;
		padding: 0 1rem;
		border-radius: 0.5rem;
	}
	.back-next > :nth-child(2):hover {
		filter: brightness(150%) contrast(80%);
	}
	.visible {
		visibility: visible;
		opacity: 1;
		transition: opacity 200ms 200ms, visibility 0ms 0ms;
	}
	.hidden {
		visibility: hidden;
		opacity: 0;
		transition: opacity 200ms 0ms, visibility 0ms 200ms;
	}
	.body {
		margin-top: 1rem;
	}
	.back-next > .confirm {
		background-color: var(--purplish-blue);
	}
	.thanks.panel > img {
		place-self: center;
		margin: 2rem;
	}
	.thanks.panel {
		padding: 3rem;
		text-align: center;
	}
	.thanks.panel > .title {
		font-size: 2rem;
	}
	@media (max-width: 800px) {
		.root {
			position: absolute;
			width: 100%;
			height: 100%;
			border-radius: 0;
			padding: 0;
			margin: 0;
			background-color: var(--magnolia);
			inset: 0;
			transform: none;
			min-width: 0;
		}
		.nav {
			position: absolute;
			display: block;
			background-image: url('/images/bg-sidebar-mobile.svg');
			background-size: cover;
			min-width: 100%;
			max-width: 100%;
			min-height: 25vh;
			border-radius: 0;
		}
		.section {
			display: inline-flex;
			min-width: 0;
			padding: 0;
			margin: 0;
			width: 0;
			height: 0;
			overflow: hidden;
			padding-left: 18vw;
		}
		.section::before {
			display: none;
		}
		.section::after {
			left: auto;
			top: 4rem;
		}
		.panel {
			position: absolute;
			background-color: white;
			width: 78vw;
			padding: 1.5rem;
			border-radius: 1rem;
			margin: 0;
			top: 15vh;
			left: 50%;
			transform: translateX(-50%);
		}
		.back-next {
			position: absolute;
			background-color: white;
			bottom: 0;
			width: calc(100vw - 2rem);
			place-content: center;
			padding: 1rem;
		}
		.back-next > :nth-child(2) {
			border-radius: 0.3rem;
			font-size: 0.9rem;
			padding: 0.6rem;
		}
	}
</style>
