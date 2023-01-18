<script lang="ts">
	export let labels: string[];
	export let value: boolean = false;

	const Toggle = () => {
		value = !value;
	};
</script>

<div class="root">
	<div class="label {!value ? 'active' : ''}">{labels[0]}</div>
	<div class="slot" on:click={Toggle} on:keypress={Toggle}>
		<div class="switch {value ? 'active' : ''}" />
	</div>
	<div class="label {value ? 'active' : ''}">{labels[1]}</div>
</div>

<style>
	.root {
		display: grid;
		grid-template-columns: 1fr 2rem 1fr;
		text-align: center;
		background-color: var(--magnolia);
		font-size: 0.9rem;
		font-weight: 500;
		padding: 0.8rem;
		border-radius: 0.4rem;
		color: var(--cool-gray);
	}
	.active {
		color: var(--marine-blue);
	}
	.slot {
		position: relative;
		width: 2.2rem;
		height: 1.2rem;
		top: 0.12rem;
		border-radius: 1rem;
		background-color: hsl(213, 96%, 18%);
	}
	.switch {
		position: absolute;
		width: 0.7rem;
		height: 0.7rem;
		background-color: aliceblue;
		border-radius: 0.6rem;
		left: 0.24rem;
		top: 0.24rem;
		transition: 200ms ease;
	}
	.switch.active {
		left: 1.2rem;
	}
</style>
