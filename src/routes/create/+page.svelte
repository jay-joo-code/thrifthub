<script lang="ts">
	import PageContainer from '$lib/glue/components/PageContainer.svelte';
	import TextInput from '$lib/glue/components/TextInput.svelte';
	import DatePicker from '$lib/glue/components/DatePicker.svelte';
	import { addDays } from 'date-fns';
	import { pb } from '$lib/glue/pocketbase';
	import { goto } from '$app/navigation';

	let name = '';
	let startDate = new Date();
	let endDate = addDays(new Date(), 7);
	let isLoading = false;

	const handleSubmit = async () => {
		isLoading = true;
		const result = await pb.collection('events').create({
			name,
			startDate,
			endDate
		});
		goto(`/event/${result?.id}`);
	};
</script>

<PageContainer title="Create event">
	<form on:submit|preventDefault={handleSubmit} class="space-y-4">
		<h1 class="text-4xl font-semibold">Create event</h1>
		<p class="text-base-content/80">
			Lorem ipsum dolor sit amet consectetur adipisicing elit. Adipisci sed totam reprehenderit
		</p>
		<TextInput label="Event name" bind:value={name} />
		<!-- TODO: replace with single calendar like when2meet -->
		<DatePicker bind:value={startDate} label="Start date" />
		<DatePicker bind:value={endDate} label="End date" />
		<button class={`btn-primary btn ${isLoading ? 'loading' : ''}`}>Create event</button>
	</form>
</PageContainer>
