<script lang="ts">
	// This is a minimal reproduction of a state object I used
	// while developing a project in Svelte 5:
	const strat = $state({
		steps: [
			{
				players: [
					{
						id: '1',
						fromStep: 0
					}
					// For demonstration purposes, I'm only using one player object in each step.
					// The issue is the same when there are multiple players, though:
					// All objects in the array get copied over.
				],
				stickies: [
					{
						id: '2'
					}
				]
			},
			{
				players: [
					{
						id: '1',
						fromStep: 1
					}
				],
				stickies: [
					{
						id: '2'
					}
				]
			},
			{
				players: [
					{
						id: '1',
						fromStep: 2
					}
				],
				stickies: [
					{
						id: '2'
					}
				]
			}
		]
	});

	// Changing this initial value to 1 or 2, you can observe that the
	// behaviour is the same, except the "origin" step of it changes.
	const view = $state({
		activeStep: 0
	});

	// Whenever an active step is set, the "origin step"'s player object
	// gets overwritten by the one from the the step being activated.
	function setActiveStep(index: number) {
		view.activeStep = index;
	}

	// The same issue occurs when using this commented out derived state
	// instead of directly accessing the strat.steps[view.activeStep] object
	// in the template:
	//
	// const activeStepData = $derived(strat.steps[view.activeStep]);
</script>

<button on:click={async () => setActiveStep(0)}>Step 0</button>
<button on:click={async () => setActiveStep(1)}>Step 1</button>
<button on:click={async () => setActiveStep(2)}>Step 2</button>

{#each strat.steps[view.activeStep].players as activePlayer}
	<div>view.activeStep: {view.activeStep}</div>
	<div style="margin-bottom: 1rem">activePlayer.fromStep: {activePlayer.fromStep}</div>
{/each}

<div>'players' objects are identical in the following step combinations:</div>
<div>0 & 1: {strat.steps[0].players[0] === strat.steps[1].players[0]}</div>
<div>0 & 2: {strat.steps[0].players[0] === strat.steps[2].players[0]}</div>
<div style="margin-bottom: 1rem">
	1 & 2: {strat.steps[1].players[0] === strat.steps[2].players[0]}
</div>

<div>'stickies' objects are identical in the following step combinations:</div>
<div>0 & 1: {strat.steps[0].stickies[0] === strat.steps[1].stickies[0]}</div>
<div>0 & 2: {strat.steps[0].stickies[0] === strat.steps[2].stickies[0]}</div>
<div style="margin-bottom: 1rem">
	1 & 2: {strat.steps[1].stickies[0] === strat.steps[2].stickies[0]}
</div>
