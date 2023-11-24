<script lang="ts">
	import _ from 'lodash';

	const size = 4;
	const colors = ['white', 'red'];
	const patterns = [
		'none',
		'top-left',
		'top-right',
		'bottom-left',
		'bottom-right',
		'solid'
	] as const;
	interface Square {
		color: string;
		pattern: (typeof patterns)[number];
	}

	const squares: Square[] = Array(size * size)
		.fill(0)
		.map(() => ({ color: colors[0], pattern: _.sample(patterns) }));

	let hovering: number | null = null;

	const click = (i: number) => () => {
		// squares[i].color = squares[i].color === colors[0] ? colors[1] : colors[0];
		squares[i].pattern = patterns[(patterns.indexOf(squares[i].pattern) + 1) % patterns.length];
	};
</script>

<div class="quilt">
	{#each Array(size) as _, i}
		<div class="row">
			{#each Array(size) as _, j}
				{@const index = i * size + j}
				{@const square = squares[index]}
				<div
					class="square {hovering === index ? 'hover' : ''}"
					on:click={click(index)}
					on:mouseenter={() => {
						hovering = index;
					}}
					on:mouseleave={() => {
						hovering = null;
					}}
				>
					<div class="pattern {square.pattern}" />
				</div>
			{/each}
		</div>
	{/each}
</div>

<style>
	.quilt {
		display: flex;
		flex-direction: column;
	}

	.row {
		display: flex;
		flex-direction: row;
	}

	.square {
		flex-basis: 100px;
		flex-grow: 0;
		height: 100px;
		box-sizing: border-box;
	}

	.hover {
		border: 1px solid black;
	}

	.pattern {
		background-color: #dd4f39;
		height: 100%;
		width: 100%;
	}

	.none {
		background-color: unset;
	}

	.top-left {
		clip-path: polygon(0 0, 100% 0, 100% 100%);
	}

	.top-right {
		clip-path: polygon(0 0, 100% 0, 0 100%);
	}

	.bottom-left {
		clip-path: polygon(0 0, 100% 100%, 0 100%);
	}

	.bottom-right {
		clip-path: polygon(100% 0, 0 100%, 100% 100%);
	}
</style>
