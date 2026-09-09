<script>
	let { onSignupClick = () => {} } = $props();
</script>

<div class="relative h-fit w-full p-2">
	<div class="relative">
		<img
			src="/assets/hero_banner-BZ19wDxE.jpg"
			class="min-h-80 w-screen rounded-xl object-fill opacity-90 lg:h-[calc(99vh-100px)]"
			alt="TECHgium Banner"
		/>
		<div
			class="absolute bottom-0 left-0 right-0 top-0 rounded-xl bg-brand-blue-700 opacity-40 sm:opacity-30 md:opacity-0"
		></div>
	</div>

	<div
		class="absolute max-sm:left-8 sm:left-16 max-sm:right-4 sm:right-10 top-4 max-[400px]:top-2 sm:top-4 md:top-8 lg:top-14 xl:top-32 max-sm:bottom-2 sm:bottom-4 md:bottom-10 flex"
	>
		<div
			class="max-sm:-mt-5 sm:-mt-8 lg:-mt-20 xl:-mt-36 flex flex-col justify-center max-sm:gap-3 sm:gap-3 md:gap-6 2xl:gap-10"
		>
			<h1
				class="grid text-white font-bold max-sm:text-3xl sm:text-4xl lg:text-[48px] xl:text-[53px] lg:leading-[65px] sm:grid-cols-[1fr_0.5fr] md:grid-cols-2 lg:grid-cols-[1fr_0.5fr] xl:grid-cols-2"
			>
				<div>Tap Into The World Of Possibilities with TECHgium® 10th Edition</div>
			</h1>

			<p class="font-medium text-white max-sm:text-sm sm:text-base lg:text-lg xl:text-xl">
				<span class="sm:block"
					>Be a part of the legacy that fosters innovation and solutions{' '}</span
				>
				<span>that are set to shape a new future</span>
			</p>

			<div>
				<button
					type="button"
					onclick={onSignupClick}
					class="cursor-pointer rounded bg-electric-blue font-medium text-brand-blue-500 duration-300 hover:bg-white max-lg:py-2 lg:py-3 max-md:px-3 md:px-4 lg:px-5 text-base"
				>
					Signup
				</button>
			</div>
		</div>
	</div>
</div>
