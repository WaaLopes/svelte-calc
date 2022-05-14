<script lang="ts">
	const defaultDisplayValue = '0';
	const numbers = Array.from({ length: 10 }, (_, index) => index.toString());
	const operations = ['+', '-', '*', '/', '^'] as const;

	type OperationsType = typeof operations[number];

	let displayValue = defaultDisplayValue;
	let storedValue: number | null;
	let storedOperation: OperationsType | null;

	function clearDisplay() {
		displayValue = defaultDisplayValue;
	}

	function handleNumberClick(selectedNumber: string) {
		if (displayValue === defaultDisplayValue) {
			displayValue = selectedNumber.toString();
			return;
		}

		displayValue += selectedNumber;
	}

	function handleDecimalClick() {
		if (!displayValue.includes('.')) {
			displayValue += '.';
		}
	}

	function handleOperationClick(operation: OperationsType) {
		if (storedValue && storedOperation) {
			storedValue = executeOperation();
		} else {
			storedValue = +displayValue;
		}

		storedOperation = operation;
		clearDisplay();
	}

	function handleEqualsClick() {
		displayValue = executeOperation().toString();
		storedValue = null;
		storedOperation = null;
	}

	function executeOperation(): number {
		const displayValueNumber = +displayValue;
		switch (storedOperation) {
			case '+':
				return displayValueNumber + storedValue;
			case '-':
				return storedValue - displayValueNumber;
			case '*':
				return displayValueNumber * storedValue;
			case '/':
				return storedValue / displayValueNumber;
			case '^':
				return Math.pow(storedValue, displayValueNumber);
		}
	}
</script>

<main>
	<h1>Calculator</h1>

	<div class="calc">
		<input class="display" bind:value={displayValue} >

		<div class="calc-body">
			<div class="calc-numbers">
				{#each numbers as number}
				<button on:click={() => handleNumberClick(number)}>{number}</button>
				{/each}
				<button on:click={handleDecimalClick}>.</button>
				<button on:click={clearDisplay}>C</button>
				<button class="button-equals" on:click={handleEqualsClick}>=</button>
			</div>

			<div class="calc-operations">
				{#each operations as operation}
				<button on:click={() => handleOperationClick(operation)}>{operation}</button>
				{/each}
			</div>
		</div>
	</div>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	button {
		margin: 0;
	}

	.calc {
		display: inline-block;
		padding: 5px;
		border: 1px solid lightgray;
		border-radius: 3px;
	}

	.display {
		width: 200px;
	}

	.calc-body {
		display: flex;
		justify-content: center;
		gap: 5px;
	}

	.calc-numbers {
		display: grid;
		grid-template-columns: repeat(3, 1fr);
		width: 155px;
		gap: 5px;
	}

	.button-equals {
		grid-column-start: span 3;
	}

	.calc-operations {
		display: flex;
		flex-direction: column;
		gap: 5px;
	}

	.calc-operations button {
		width: 40px;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>