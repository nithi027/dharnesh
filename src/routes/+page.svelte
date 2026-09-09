<script>
	import Navbar from '$lib/components/Navbar.svelte';
	import MarqueeBanner from '$lib/components/MarqueeBanner.svelte';
	import Hero from '$lib/components/Hero.svelte';
	import DreamReality from '$lib/components/DreamReality.svelte';
	import Winners from '$lib/components/Winners.svelte';
	import Awards from '$lib/components/Awards.svelte';
	import HowItWorks from '$lib/components/HowItWorks.svelte';
	import ValueProps from '$lib/components/ValueProps.svelte';
	import EduTech from '$lib/components/EduTech.svelte';
	import ContactUs from '$lib/components/ContactUs.svelte';
	import Footer from '$lib/components/Footer.svelte';
	import ChatBot from '$lib/components/ChatBot.svelte';
	import AuthModal from '$lib/components/AuthModal.svelte';

	let heroAuthOpen = $state(false);
</script>

<svelte:head>
	<title>L&T Technology Services</title>
	<meta
		name="description"
		content="TECHgium® - 10th Edition. Cultivating a global talent pool with a solution-oriented mindset to tackle industry challenges."
	/>
</svelte:head>

<div class="relative min-h-screen bg-white font-figtree text-stone-900 selection:bg-brand-blue-500 selection:text-white">
	<!-- Top Navigation -->
	<Navbar />

	<main>
		<!-- Announcement Marquee -->
		<MarqueeBanner />

		<!-- Hero Banner -->
		<Hero onSignupClick={() => (heroAuthOpen = true)} />

		<!-- Dream Reality Section -->
		<DreamReality />

		<!-- Winners & Hall of Fame -->
		<Winners />

		<!-- Awards and Recognitions -->
		<Awards />

		<!-- How It Works, Process & Timeline -->
		<HowItWorks />

		<!-- Win Big Features -->
		<ValueProps />

		<!-- L&T EduTech Banner -->
		<EduTech />

		<!-- Contact Us -->
		<ContactUs />
	</main>

	<!-- Footer -->
	<Footer />

	<!-- Interactive Help Chatbot -->
	<ChatBot />

	<!-- Auth Modal for Hero Signup button -->
	<AuthModal bind:isOpen={heroAuthOpen} />
</div>
