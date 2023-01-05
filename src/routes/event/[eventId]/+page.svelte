<script lang="ts">
	import TimePicker from '$lib/components/TimePicker.svelte';
	import PageContainer from '$lib/glue/components/PageContainer.svelte';
	import IconAdd from '$lib/glue/icons/IconAdd.svelte';
	import { pb } from '$lib/glue/pocketbase';
	import defaultAvailability from '$lib/util/defaultAvailability';
	import { onMount } from 'svelte';
	import debounce from 'just-debounce-it';
	import type { TValue } from '$lib/types/availability';

	export let data;

	const { eventId } = data;
	let event: any = null;

	const fetchEvent = async () => {
		event = await pb.collection('events').getOne(eventId);

		if (!event.availability) {
			event.availability = defaultAvailability();
		}
	};

	onMount(fetchEvent);

	const saveAvailability = (availability: TValue) => {
		if (event?.id) {
			pb.collection('events').update(event?.id, {
				availability
			});
		}
	};
	const debouncedSaveAvailability = debounce(saveAvailability, 200);

	$: debouncedSaveAvailability(event?.availability);
</script>

{#if event}
	<PageContainer title={event?.name}>
		<div class="space-y-8">
			<div class="space-y-4">
				<h1 class="text-4xl font-semibold">{event?.name}</h1>
				<button class="btn-primary btn"><IconAdd /> Add availability</button>
				<TimePicker bind:value={event.availability} />
			</div>
		</div>
	</PageContainer>
{/if}
