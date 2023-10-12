<script>
	import '../app.css';
	import LOGO from '$lib/assets/logo.png';
	import BackgroundAnimated from '$lib/components/BackgroundAnimated.svelte';
</script>

<header
	class="h-16 px-3 shadow-lg bg-white gap-2 flex items-center justify-center rounded-bl-2xl rounded-br-2xl"
>
	<img class="w-9" src={LOGO} alt="" />
	<h1 class="text-2xl text-black">USD Converter</h1>
</header>

<BackgroundAnimated />

<slot />

<footer class="fixed bottom-0 left-0 w-full bg-white py-3 px-5 flex items-center gap-1">
	<p class="font-semibold">Created by:</p>
	<a class="font-semibold text-purple-800" target="_blank" href="https://github.com/williamsmm"
		>Williams MM</a
	>
</footer>
