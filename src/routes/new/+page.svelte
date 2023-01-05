<script lang="ts">
	import { goto } from '$app/navigation';
	import PageContainer from '$lib/glue/components/PageContainer.svelte';
	import Textarea from '$lib/glue/components/Textarea.svelte';
	import TextInput from '$lib/glue/components/TextInput.svelte';
	import { currentUser, pb } from '$lib/glue/pocketbase';

	const formData = new FormData();
	let name = `test item ${Math.floor(Math.random() * 1000)}`;
	let price = '0';
	let isLoading = false;
	let desc =
		'Lorem ipsum dolor sit, amet consectetur adipisicing elit. Aliquam vero ea optio sint. Cupiditate veniam molestiae consectetur aliquam illo debitis.';

	const handleSubmit = async () => {
		isLoading = true;
		if ($currentUser) {
			formData.append('name', name);
			formData.append('price', price);
			formData.append('desc', desc);
			formData.append('user', $currentUser?.id as string);
			const result = await pb.collection('items').create(formData);
			goto(`/item/${result?.id}`);
		}
		isLoading = false;
	};

	const handleFileChange = (event: any) => {
		for (let file of event?.target?.files) {
			formData.append('photos', file);
		}
	};
</script>

<PageContainer title="Sell items">
	<form on:submit|preventDefault={handleSubmit} class="space-y-4">
		<h1 class="text-4xl font-semibold">Sell items</h1>
		<p class="text-base-content/80">
			Lorem ipsum dolor sit amet consectetur adipisicing elit. Adipisci sed totam reprehenderit
		</p>
		<TextInput label="Item name" bind:value={name} />
		<TextInput type="number" label="Price (USD)" bind:value={price} />
		<Textarea label="Description" bind:value={desc} class="h-48" />
		<input
			type="file"
			class="file-input-bordered file-input w-full max-w-xs"
			on:change={handleFileChange}
			multiple
		/>
		<button class={`btn-primary btn ${isLoading ? 'loading' : ''}`}>Save item</button>
	</form>
</PageContainer>
