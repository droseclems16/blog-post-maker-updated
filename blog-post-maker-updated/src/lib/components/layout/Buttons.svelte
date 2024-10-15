<script>
	import Notification from '$lib/components/modals/Notification.svelte';
	import WriteArea from '$lib/components/modals/WriteArea.svelte';
	export let supabaseUrl, supabaseKey;

	const defaultPostContent = `# H1 heading

## H2 heading

### H3 heading

--------

**bold text**

*italicized text*

--------

1. First item
2. Second item
3. Third item

- First item
- Second item
- Third item

[Svelte](https://svelte.dev/)`;

	let loading = false,
		showWriteArea = false,
		showNotifModal = false,
		notifType = 'warning',
		notifMessage = '',
		editMode = false,
		postTitle = '',
		postContent = defaultPostContent;

	const sendToSupabase = async () => {
		loading = true;
		showNotifModal = false;

		let statusCode;

		if (!editMode) {
			const req = await fetch(`${supabaseUrl}/rest/v1/posts`, {
				method: 'POST',
				headers: {
					apikey: supabaseKey,
					Authorization: 'Bearer ' + supabaseKey,
					'Content-Type': 'application/json',
					Prefer: 'return=representation'
				},
				body: JSON.stringify({ title: postTitle, content: postContent })
			});

			statusCode = req.status;
		} else {
			const req = await fetch(`${supabaseUrl}/rest/v1/posts?title=eq.${postTitle}`, {
				method: 'PATCH',
				headers: {
					apikey: supabaseKey,
					Authorization: 'Bearer ' + supabaseKey,
					'Content-Type': 'application/json',
					Prefer: 'return=representation'
				},
				body: JSON.stringify({ content: postContent })
			});

			statusCode = req.status;
		}

		loading = false;
		showNotifModal = true;

		if (statusCode === 401 || statusCode === 404) {
			notifType = 'warning';

			if (statusCode === 401) {
				notifMessage = "You don't have permission to communicate with the Supabase.";
			} else if (statusCode === 404) {
				notifMessage =
					"The Supabase URL you have given doesn't exist or the post title is incorrect.";
			} else {
				notifMessage = 'Something went wrong! Please try again.';
			}
		} else if (statusCode === 200 || statusCode === 201) {
			notifType = 'success';

			if (statusCode === 200) {
				notifMessage = 'Your post has been successfully edited.';
			} else if (statusCode === 201) {
				notifMessage = 'Your post has been successfully created.';
			}
		}
	};
</script>

{#if showNotifModal}
	<Notification bind:showNotifModal {notifType} {notifMessage} />
{/if}

{#if showWriteArea}
	<WriteArea bind:showWriteArea bind:postTitle bind:postContent bind:editMode />
{/if}

<div class="flex justify-center">
	{#if !loading}
		<div
			class="grid grid-cols-1 content-center items-center space-y-3 pt-16 md:grid-cols-2 md:space-x-3 md:space-y-0"
		>
			<button
				class="rounded-md bg-blue-700 p-3 font-semibold text-white duration-300 hover:bg-blue-600"
				on:click|preventDefault={() => showWriteArea = !showWriteArea}
			>
				{editMode ? 'Edit a post' : 'Create a post'}
			</button>
			<button
				class="rounded-md bg-green-800 p-3 px-6 font-semibold text-white duration-300 hover:bg-green-700"
				on:click|preventDefault={sendToSupabase}
			>
				Send to Supabase
			</button>
		</div>
	{:else}
		<img class="pt-16" src="loading.gif" width="50" alt="" />
	{/if}
</div>
