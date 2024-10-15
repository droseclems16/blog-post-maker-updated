<script>
	import Input from '$lib/components/layout/Input.svelte';
    import Buttons from '$lib/components/layout/Buttons.svelte';
	let supabaseUrl, supabaseKey;
</script>

<div class="px-3 pb-12 pt-8" style="background-color: #FFC0CB;">
	<!-- Header -->
	<div class="justify-center text-center">
		<h1 class="text-6xl font-semibold" style="line-height: 1.25">Blog Post Maker</h1>
		<h3 class="text-md pt-5">
			Make a blog post to your website with
			<a 
				class="border-b-2 border-gray-200 text-gray-900 duration-500
			hover:border-gray-900"
				href="https://marked.js.org/">Marked</a>, and
			<a 
				class="border-b-2 border-gray-200 text-gray-900 duration-500
			hover:border-gray-900"
				href="https://supabase.io/">Supabase</a>!
		</h3>
	</div>
	<!-- Input Supabase URL and API Key -->
	<Input bind:supabaseUrl bind:supabaseKey />
    <!-- Buttons -->
    <Buttons bind:supabaseUrl bind:supabaseKey />
	<!-- Footer -->
	<div class="flex justify-center pt-28">
		<p>
			Made by KUBEPATROL@OpsWerks <span class="text-red-500">❤</span> <span class="px-2">•</span> Check this page in
			<a
				class="border-b-2 border-gray-200 text-gray-900 duration-500
        hover:border-gray-900"
				href="https://github.com/opswerks-academy/i7-observability.git">GitHub</a
			>
		</p>
	</div>
</div>
