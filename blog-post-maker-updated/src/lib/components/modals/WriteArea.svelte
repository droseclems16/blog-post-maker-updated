<script>
	import { fade } from 'svelte/transition';
	import { marked } from 'marked';
	export let showWriteArea, postTitle, postContent, editMode;

	let markdown;

	marked.setOptions({
		breaks: true
	});

	$: markdown = marked(postContent);
</script>

<div
	class="fixed left-0 top-0 z-10 h-full w-full bg-black bg-opacity-50"
	transition:fade={{ duration: 300 }}
>
	<div class="mx-auto mt-[4vh] w-9/12 rounded-lg bg-[#282c34] p-2 shadow-xl">
		<div class="float-right flex items-center pr-1.5 pt-1.5 md:space-x-6">
			<div class="-mr-1 -mt-5 mt-5 flex items-center md:mt-0">
				{#if editMode}
					<input type="checkbox" on:click={() => (editMode = !editMode)} checked />
				{:else}
					<input type="checkbox" on:click={() => (editMode = !editMode)} />
				{/if}
				<h3 class="pl-2 text-lg text-white">Edit mode?</h3>
			</div>
			<button
				class="hidden rounded-lg bg-red-700 px-4 py-2 font-semibold text-white duration-200 hover:bg-red-600 md:inline"
				id="close-modal"
				on:click|preventDefault={() => showWriteArea = !showWriteArea}>Close editor</button
			>
			<button
				class="-mr-5 -mt-5 rounded-lg p-3 text-3xl font-semibold text-black duration-200 md:hidden"
				on:click|preventDefault={() => showWriteArea = !showWriteArea}
			>
				<svg
					class="text-red-700"
					xmlns="http://www.w3.org/2000/svg"
					aria-hidden="true"
					role="img"
					width="1em"
					height="1em"
					preserveAspectRatio="xMidYMid meet"
					viewBox="0 0 1024 1024"
					><path
						d="M512 64C264.6 64 64 264.6 64 512s200.6 448 448 448s448-200.6 448-448S759.4 64 512 64zm165.4 618.2l-66-.3L512 563.4l-99.3 118.4l-66.1.3c-4.4 0-8-3.5-8-8c0-1.9.7-3.7 1.9-5.2l130.1-155L340.5 359a8.32 8.32 0 0 1-1.9-5.2c0-4.4 3.6-8 8-8l66.1.3L512 464.6l99.3-118.4l66-.3c4.4 0 8 3.5 8 8c0 1.9-.7 3.7-1.9 5.2L553.5 514l130 155c1.2 1.5 1.9 3.3 1.9 5.2c0 4.4-3.6 8-8 8z"
						fill="currentColor"
					/></svg
				>
			</button>
		</div>
		<div class="pl-5 pt-2.5">
			<p class="pb-2 text-xl text-white">Post Title</p>
			<input
				class="w-11/12 rounded-lg border border-black py-1.5 pl-2 pr-1 outline-none md:w-96"
				type="text"
				placeholder="Your post's title"
				bind:value={postTitle}
			/>
		</div>
		<div class="flex w-full p-5">
			<div class="flex w-3/5 items-center overflow-hidden rounded-l-lg">
				<textarea
					class="h-[70vh] w-full resize-none overflow-auto bg-[#36393f] pb-2 pl-3 pr-2 pt-3 text-xl text-white outline-none"
					type="text"
					bind:value={postContent}
					placeholder="The content of your post"
				></textarea>
			</div>
			<div class="w-3/5 overflow-hidden break-all rounded-r-lg">
				<div
					class="prose prose-sm h-[70vh] w-full max-w-none overflow-auto bg-white pb-2 pl-3 pr-2 pt-3 text-[#36393f] md:prose-lg"
				>
					{@html markdown}
				</div>
			</div>
		</div>
	</div>
</div>
