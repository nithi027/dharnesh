<script>
	import AuthModal from './AuthModal.svelte';

	let mobileOpen = $state(false);
	let authOpen = $state(false);

	function toggleMobile() {
		mobileOpen = !mobileOpen;
		if (typeof document !== 'undefined') {
			document.body.style.overflow = mobileOpen ? 'hidden' : 'auto';
		}
	}

	function closeMobile() {
		mobileOpen = false;
		if (typeof document !== 'undefined') {
			document.body.style.overflow = 'auto';
		}
	}

	function openAuth() {
		closeMobile();
		authOpen = true;
	}
</script>

<div class="mb-[5.5rem] font-figtree">
	<header
		class="fixed top-0 z-50 flex w-full items-center justify-between bg-white py-3 max-[640px]:px-2 sm:px-3 lg:px-12 xl:px-16"
	>
		<a href="/">
			<img
				src="/assets/techgium-logo-Cmw72K-9.png"
				class="max-h-10 w-full max-w-60 object-contain sm:max-h-12 lg:max-h-14"
				alt="TECHgium Logo"
				loading="lazy"
			/>
		</a>

		<!-- Desktop Navigation -->
		<nav
			class="hidden items-center justify-center gap-[10px] font-medium text-stone-800 md:flex md:text-base lg:gap-3 xl:gap-4 2xl:text-lg"
		>
			<a
				href="#!"
				onclick={openAuth}
				class="navButton relative font-semibold text-brand-blue-500 after:absolute after:bottom-0 after:left-0 after:h-0.5 after:w-full after:rounded-full after:bg-brand-blue-500 after:content-['']"
			>
				Home
			</a>
			<a
				href="#!"
				onclick={openAuth}
				class="relative text-nowrap transition-colors duration-300 hover:text-brand-blue-500 after:absolute after:bottom-0 after:left-0 after:h-0.5 after:w-0 after:bg-brand-blue-500 after:transition-all after:duration-500 hover:after:w-full after:content-['']"
			>
				News Feed
			</a>
			<a href="#!" onclick={openAuth} class="button-link">Gallery</a>
			<a href="#!" onclick={openAuth} class="button-link">FAQ</a>
			<button type="button" onclick={openAuth} class="button-link cursor-pointer">Login</button>
			<button
				type="button"
				onclick={openAuth}
				class="cursor-pointer rounded bg-brand-blue-500 font-medium text-electric-blue duration-300 hover:bg-electric-blue hover:text-brand-blue-500 max-lg:py-2 lg:py-3 max-md:px-3 md:px-4 lg:px-5 text-base"
			>
				Signup
			</button>
		</nav>

		<!-- Mobile Menu Button -->
		<div class="flex md:hidden">
			<button
				type="button"
				onclick={toggleMobile}
				class="cursor-pointer p-1 text-3xl text-stone-800"
				aria-label="Toggle navigation menu"
			>
				{#if mobileOpen}
					<svg
						xmlns="http://www.w3.org/2000/svg"
						class="h-8 w-8"
						fill="none"
						viewBox="0 0 24 24"
						stroke="currentColor"
					>
						<path
							stroke-linecap="round"
							stroke-linejoin="round"
							stroke-width="2"
							d="M6 18L18 6M6 6l12 12"
						/>
					</svg>
				{:else}
					<svg
						xmlns="http://www.w3.org/2000/svg"
						class="h-8 w-8"
						fill="none"
						viewBox="0 0 24 24"
						stroke="currentColor"
					>
						<path
							stroke-linecap="round"
							stroke-linejoin="round"
							stroke-width="2"
							d="M4 6h16M4 12h16M4 18h16"
						/>
					</svg>
				{/if}
			</button>
		</div>
	</header>

	<!-- Mobile Drawer Menu -->
	<div
		class="fixed bottom-0 top-16 z-40 h-full w-full bg-white text-stone-700 transition-all duration-500 ease-in md:hidden {mobileOpen
			? 'left-0'
			: '-left-[770px]'}"
	>
		<div
			class="flex h-full flex-col items-center justify-center gap-6 pb-36 text-2xl font-semibold md:hidden"
		>
			<a href="#!" onclick={openAuth} class="button-link">Home</a>
			<a href="#!" onclick={openAuth} class="button-link"
				>News Feed</a
			>
			<a href="#!" onclick={openAuth} class="button-link"
				>Gallery</a
			>
			<a href="#!" onclick={openAuth} class="button-link">FAQ</a>
			<button type="button" onclick={openAuth} class="button-link cursor-pointer">Login</button>
			<button
				type="button"
				onclick={openAuth}
				class="cursor-pointer rounded bg-brand-blue-500 font-medium text-electric-blue duration-300 hover:bg-electric-blue hover:text-brand-blue-500 max-lg:py-2 lg:py-3 max-md:px-3 md:px-4 lg:px-5 text-base"
			>
				Signup
			</button>
		</div>
	</div>
</div>

<!-- Auth Modal -->
<AuthModal bind:isOpen={authOpen} />
