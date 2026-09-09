<script>
	import { onMount } from 'svelte';

	let statsElement = $state(null);
	let animated = $state(false);

	let institutesCount = $state(0);
	let studentsCount = $state(0);
	let facultiesCount = $state(0);
	let pocsCount = $state(0);

	function animateValue(setter, target, duration) {
		const start = 0;
		const startTime = performance.now();

		function step(currentTime) {
			const elapsed = currentTime - startTime;
			const progress = Math.min(elapsed / duration, 1);
			// easeOutExpo
			const ease = progress === 1 ? 1 : 1 - Math.pow(2, -10 * progress);
			setter(Math.floor(ease * (target - start) + start));
			if (progress < 1) {
				requestAnimationFrame(step);
			} else {
				setter(target);
			}
		}
		requestAnimationFrame(step);
	}

	onMount(() => {
		if (!statsElement) return;
		const observer = new IntersectionObserver(
			(entries) => {
				entries.forEach((entry) => {
					if (entry.isIntersecting && !animated) {
						animated = true;
						animateValue((v) => (institutesCount = v), 540, 2000);
						animateValue((v) => (studentsCount = v), 62000, 2500);
						animateValue((v) => (facultiesCount = v), 2500, 2000);
						animateValue((v) => (pocsCount = v), 130, 1800);
						observer.unobserve(entry.target);
					}
				});
			},
			{ threshold: 0.3 }
		);
		observer.observe(statsElement);
		return () => observer.disconnect();
	});
</script>

<section
	class="m-2 flex flex-col rounded-xl bg-slate-100 py-16 max-[640px]:px-2 sm:px-2 md:px-6 lg:px-16 xl:px-32 max-sm:gap-5 sm:gap-6 md:gap-7"
>
	<div class="flex flex-col gap-2">
		<h2 class="text-3xl font-bold text-brand-blue-500 md:text-4xl 2xl:text-[41px]">
			Celebrating The Next-Gen Talent Pool
		</h2>
		<p class="text-xl font-semibold text-stone-800 md:text-2xl">
			Winners of TECHgium<span class="relative text-lg" style="top: -0.5em; left: 0px;">®</span> 9th
			edition
		</p>
	</div>

	<!-- Winners 3 cards -->
	<div
		class="grid grid-cols-1 items-center justify-around pb-2 max-sm:gap-5 max-md:gap-2 sm:grid-cols-2 sm:items-start md:gap-6 lg:grid-cols-3"
	>
		<!-- Card 1: Winner -->
		<div class="flex w-full flex-col gap-2 sm:max-w-[500px]">
			<img
				src="/assets/winner-9th-BlyoTbwM.jpg"
				class="min-h-60 w-full rounded object-cover sm:min-h-72"
				alt="Winner"
				loading="lazy"
			/>
			<div class="flex flex-col gap-1.5 lg:px-2">
				<span
					class="w-fit rounded-md bg-brand-blue-500 px-3 py-0.5 text-xs font-bold text-white md:text-sm"
				>
					Winner
				</span>
				<span class="text-sm font-bold text-brand-blue-500 md:text-[15px]">
					NextGen Self‑Charging Hybrid Scooter
				</span>
				<h3 class="text-xl font-bold text-stone-900 xl:text-[20px] 2xl:text-2xl">
					M. S. Ramaiah Institute of Technology, Bengaluru, Karnataka
				</h3>
			</div>
		</div>

		<!-- Card 2: First Runner-up -->
		<div class="flex w-full flex-col gap-2 sm:max-w-[500px]">
			<img
				src="/assets/silver-9th-BhFevqkB.jpg"
				class="min-h-60 w-full rounded object-cover sm:min-h-72"
				alt="First Runner-up"
				loading="lazy"
			/>
			<div class="flex flex-col gap-1.5 lg:px-2">
				<span
					class="w-fit rounded-md bg-brand-blue-500 px-3 py-0.5 text-xs font-bold text-white md:text-sm"
				>
					First Runner-up
				</span>
				<span class="text-sm font-bold text-brand-blue-500 md:text-[15px]">
					TETROBOT – Modular Robotic Arm
				</span>
				<h3 class="text-xl font-bold text-stone-900 xl:text-[20px] 2xl:text-2xl">
					RNS Institute of Technology, Bengaluru, Karnataka
				</h3>
			</div>
		</div>

		<!-- Card 3: Second Runner-up -->
		<div class="flex w-full flex-col gap-2 sm:max-w-[500px]">
			<img
				src="/assets/bronze-9th-DEH4-tHS.jpg"
				class="min-h-60 w-full rounded object-cover sm:min-h-72"
				alt="Second Runner-up"
				loading="lazy"
			/>
			<div class="flex flex-col gap-1.5 lg:px-2">
				<span
					class="w-fit rounded-md bg-brand-blue-500 px-3 py-0.5 text-xs font-bold text-white md:text-sm"
				>
					Second Runner-up
				</span>
				<span class="text-sm font-bold text-brand-blue-500 md:text-[15px]">
					AERIS – Stability Starts with Comfort
				</span>
				<h3 class="text-xl font-bold text-stone-900 xl:text-[20px] 2xl:text-2xl">
					PSNA College of Engineering and Technology, Dindigul, Tamil Nadu
				</h3>
			</div>
		</div>
	</div>

	<!-- Gallery Preview & Live Stats Block -->
	<div
		bind:this={statsElement}
		class="mt-10 grid grid-cols-1 justify-around gap-4 rounded-xl bg-slate-300 p-3 sm:grid-cols-2 sm:p-5 lg:gap-10"
	>
		<div class="flex flex-col justify-center gap-4">
			<h3 class="text-2xl font-bold text-stone-800 sm:text-3xl md:text-2xl 2xl:text-3xl">
				Take a sneak peek at the{' '}
				<span style="white-space: nowrap;">
					TECHgium{' '}
					<span class="relative text-2xl" style="top: -0.4em; left: -7px;">®</span>
				</span>
				moments and highlights from our previous events.
			</h3>
			<div>
				<a
					href="https://ltts.techgium.com/gallery"
					class="cursor-pointer rounded bg-brand-blue-500 font-semibold text-electric-blue duration-300 hover:bg-electric-blue hover:text-brand-blue-500 max-lg:py-2 lg:py-3 max-md:px-3 md:px-4 lg:px-5 text-base"
				>
					View More
				</a>
			</div>
		</div>

		<div class="flex w-full items-center justify-center">
			<div
				class="grid w-full grid-cols-2 justify-center gap-1 sm:grid-cols-1 md:grid-cols-2 md:gap-2 lg:gap-4"
			>
				<!-- Stat 1 -->
				<div
					class="h-fit rounded bg-brand-blue-500 px-2 py-1.5 lg:gap-1 lg:px-4 lg:py-[10px] flex flex-col"
				>
					<div class="text-xl font-bold text-electric-blue md:text-2xl lg:text-3xl">
						<span>{institutesCount.toLocaleString()}</span>
						<span class="ml-1">+</span>
					</div>
					<div class="text-xs font-semibold text-white md:text-sm lg:text-base">
						Registered Institutes
					</div>
				</div>

				<!-- Stat 2 -->
				<div
					class="h-fit rounded bg-brand-blue-500 px-2 py-1.5 lg:gap-1 lg:px-4 lg:py-[10px] flex flex-col"
				>
					<div class="text-xl font-bold text-electric-blue md:text-2xl lg:text-3xl">
						<span>{studentsCount.toLocaleString()}</span>
						<span class="ml-1">+</span>
					</div>
					<div class="text-xs font-semibold text-white md:text-sm lg:text-base">
						Registered Students
					</div>
				</div>

				<!-- Stat 3 -->
				<div
					class="h-fit rounded bg-brand-blue-500 px-2 py-1.5 lg:gap-1 lg:px-4 lg:py-[10px] flex flex-col"
				>
					<div class="text-xl font-bold text-electric-blue md:text-2xl lg:text-3xl">
						<span>{facultiesCount.toLocaleString()}</span>
						<span class="ml-1">+</span>
					</div>
					<div class="text-xs font-semibold text-white md:text-sm lg:text-base">
						Registered Faculties
					</div>
				</div>

				<!-- Stat 4 -->
				<div
					class="h-fit rounded bg-brand-blue-500 px-2 py-1.5 lg:gap-1 lg:px-4 lg:py-[10px] flex flex-col"
				>
					<div class="text-xl font-bold text-electric-blue md:text-2xl lg:text-3xl">
						<span>{pocsCount.toLocaleString()}</span>
						<span class="ml-1">+</span>
					</div>
					<div class="text-xs font-semibold text-white md:text-sm lg:text-base">No.Of PoCs</div>
				</div>
			</div>
		</div>
	</div>
</section>
