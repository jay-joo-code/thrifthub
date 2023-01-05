<script>
	import PageContainer from '$lib/glue/components/PageContainer.svelte';
	import { currentUser, pb } from '$lib/glue/pocketbase';
	import { onMount } from 'svelte';

	let items = [];

	const fetchMyItems = async () => {
		if ($currentUser) {
			items = await pb.collection('items').getFullList(200, {
				user: $currentUser,
				expand: 'user'
			});
		}
	};

	onMount(fetchMyItems);
</script>

<PageContainer>
	<div class="space-y-4">
		<p class="ml-2 text-3xl font-semibold">My items</p>
		<div class="space-y-2">
			{#each items as item (item?.id)}
				<a href={`/item/${item?.id}`}>
					<div class="flex space-x-5 rounded p-2">
						<img
							class="h-[100px] rounded-xl object-cover"
							src={pb.getFileUrl(item, item?.photos[0], { thumb: '300x0' })}
							alt=""
							width="150"
							height="100"
						/>
						<div class="flex h-[100px] w-full flex-col justify-between">
							<div>
								<p class="text-xl font-semibold">{item?.name}</p>
								<p class="text-lg font-medium">{item?.expand?.user?.name}</p>
							</div>
							<p class="text-xl font-medium">${item?.price}</p>
						</div>
					</div>
				</a>
			{/each}
		</div>
	</div>
</PageContainer>
