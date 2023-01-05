<script lang="ts">
	import { currentUser } from '$lib/glue/pocketbase';

	type TValue = { [datestring: string]: { [timestamp: string]: string[] } };

	export let value: TValue = {};
	export let datestring: string;
	export let timestamp: string;
	export let isSelecting: boolean;

	const TOTAL_USERS = 2;
	let isDragging = false;

	let backgroundClass: { [key: number]: string } = {
		1: 'bg-primary',
		0.5: 'bg-primary/50',
		0: 'bg-base-300'
	};

	let borderTypeClass: { [key: string]: string } = {
		// '00': 'border-t border-base-content',
		'00': '',
		'15': '',
		// '30': 'border-t border-base-content border-dotted',
		'30': '',
		'45': ''
	};

	const mouseHandler =
		({ datestring, timestamp, isStartDrag }) =>
		(event) => {
			if ($currentUser?.id) {
				if (isDragging || event.type === 'mousedown') {
					const userIds = value[datestring][timestamp];
					// set isSelecting variable if starting drag
					if (isStartDrag) {
						if (userIds?.includes($currentUser?.id as string)) isSelecting = false;
						else isSelecting = true;
					}

					// handle value change
					if (isSelecting && !value[datestring][timestamp]?.includes($currentUser?.id)) {
						value[datestring][timestamp] = [...userIds, $currentUser?.id];
					} else if (!isSelecting) {
						value[datestring][timestamp] = userIds?.filter((id) => id !== $currentUser?.id);
					}
				}
			}
		};
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<svelte:window on:mousedown={() => (isDragging = true)} on:mouseup={() => (isDragging = false)} />
<div
	class={`h-[8px] w-10 ${backgroundClass[value[datestring][timestamp]?.length / TOTAL_USERS]} ${
		borderTypeClass[timestamp?.slice(2, 4)]
	}`}
	on:mousedown={mouseHandler({ datestring, timestamp, isStartDrag: true })}
	on:mouseenter={mouseHandler({ datestring, timestamp, isStartDrag: false })}
/>
