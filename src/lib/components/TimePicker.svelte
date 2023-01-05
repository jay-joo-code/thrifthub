<script lang="ts">
	import TimeBlock from './TimeBlock.svelte';

	type TValue = { [datestring: string]: { [timestamp: string]: string[] } };

	export let value: TValue = {};

	const timestampLabels = [
		'8am',
		'9am',
		'10am',
		'11am',
		'12pm',
		'1pm',
		'2pm',
		'3pm',
		'4pm',
		'5pm',
		'6pm',
		'7pm',
		'8pm',
		'9pm',
		'10pm',
		'11pm',
		'12pm'
	];

	let isSelecting = false;
</script>

<div class="flex space-x-2">
	<!-- timestamp labels -->
	<div class="space-y-[16px]">
		{#each timestampLabels as timestamp}
			<p class="text-xs">{timestamp}</p>
		{/each}
	</div>

	<!-- clickable availability -->
	<div class="pt-2">
		{#each Object.keys(value) as datestring}
			<div class="">
				{#each Object.keys(value[datestring])?.sort() as timestamp (`${value[datestring]}${timestamp}`)}
					<TimeBlock bind:value {datestring} {timestamp} bind:isSelecting />
				{/each}
			</div>
		{/each}
	</div>
</div>
