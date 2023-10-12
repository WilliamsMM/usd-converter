<div class="gradient-background w-full h-full fixed top-0 left-0 -z-50" />

<style>
	.gradient-background {
		background: linear-gradient(300deg, deepskyblue, darkviolet, blue);
		background-size: 180% 180%;
		animation: gradient-animation 18s ease infinite;
	}

	@keyframes gradient-animation {
		0% {
			background-position: 0% 50%;
		}
		50% {
			background-position: 100% 50%;
		}
		100% {
			background-position: 0% 50%;
		}
	}
</style>
