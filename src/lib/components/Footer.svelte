<script>
	const currentYear = new Date().getFullYear();
</script>

<footer
	class="m-2 flex flex-col gap-2 rounded-xl border border-gray-400/20 bg-brand-blue-500 px-3 sm:px-16"
>
	<!-- Top Bar: Logo & Social Icons -->
	<div
		class="flex flex-col items-start justify-between gap-4 border-b border-gray-400 py-4 px-1 sm:flex-row sm:items-center sm:gap-2"
	>
		<a href="https://www.ltts.com/" target="_blank" rel="noopener noreferrer">
			<img src="/assets/ltts_logo_white-BWg9RZXG.svg" alt="L&T Technology Services" class="h-10" />
		</a>

		<div
			class="flex items-center gap-4 text-gray-200 max-sm:text-base sm:text-lg md:text-xl lg:text-2xl"
		>
			<!-- X / Twitter -->
			<a
				href="https://twitter.com/LnTTechservices"
				target="_blank"
				rel="noopener noreferrer"
				class="transition-transform hover:scale-110 hover:text-white"
				aria-label="Twitter"
			>
				<svg class="h-5 w-5 fill-current" viewBox="0 0 24 24">
					<path
						d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"
					/>
				</svg>
			</a>

			<!-- LinkedIn -->
			<a
				href="https://www.linkedin.com/company/1739031/"
				target="_blank"
				rel="noopener noreferrer"
				class="transition-transform hover:scale-110 hover:text-white"
				aria-label="LinkedIn"
			>
				<svg class="h-5 w-5 fill-current" viewBox="0 0 448 512">
					<path
						d="M416 32H31.9C14.3 32 0 46.5 0 64.3v383.4C0 465.5 14.3 480 31.9 480H416c17.6 0 32-14.5 32-32.3V64.3c0-17.8-14.4-32.3-32-32.3zM135.4 416H69V202.2h66.5V416zm-33.2-243c-21.3 0-38.5-17.3-38.5-38.5S80.9 96 102.2 96c21.2 0 38.5 17.3 38.5 38.5 0 21.3-17.2 38.5-38.5 38.5zm282.1 243h-66.4V312c0-24.8-.5-56.7-34.5-56.7-34.6 0-39.9 27-39.9 54.9V416h-66.4V202.2h63.7v29.2h.9c8.9-16.8 30.6-34.5 62.9-34.5 67.2 0 79.7 44.3 79.7 101.9V416z"
					/>
				</svg>
			</a>

			<!-- YouTube -->
			<a
				href="https://www.youtube.com/channel/UCxrAGA4YYput8eQQtn62mUg"
				target="_blank"
				rel="noopener noreferrer"
				class="transition-transform hover:scale-110 hover:text-white"
				aria-label="YouTube"
			>
				<svg class="h-6 w-6 fill-current" viewBox="0 0 576 512">
					<path
						d="M549.655 124.083c-6.281-23.65-24.787-42.276-48.284-48.597C458.781 64 288 64 288 64S117.22 64 74.629 75.486c-23.497 6.322-42.003 24.947-48.284 48.597-11.412 42.867-11.412 132.305-11.412 132.305s0 89.438 11.412 132.305c6.281 23.65 24.787 41.5 48.284 47.821C117.22 448 288 448 288 448s170.78 0 213.371-11.486c23.497-6.321 42.003-24.171 48.284-47.821 11.412-42.867 11.412-132.305 11.412-132.305s0-89.438-11.412-132.305zm-317.51 213.508V175.185l142.739 81.205-142.739 81.201z"
					/>
				</svg>
			</a>

			<!-- Facebook -->
			<a
				href="https://www.facebook.com/LnTTechnologyServices/"
				target="_blank"
				rel="noopener noreferrer"
				class="transition-transform hover:scale-110 hover:text-white"
				aria-label="Facebook"
			>
				<svg class="h-5 w-5 fill-current" viewBox="0 0 512 512">
					<path
						d="M504 256C504 119 393 8 256 8S8 119 8 256c0 123.78 90.69 226.38 209.25 245V327.69h-63V256h63v-54.64c0-62.15 37-96.48 93.67-96.48 27.14 0 55.52 4.84 55.52 4.84v61h-31.28c-30.8 0-40.41 19.12-40.41 38.73V256h68.78l-11 71.69h-57.78V501C413.31 482.38 504 379.78 504 256z"
					/>
				</svg>
			</a>

			<!-- Instagram -->
			<a
				href="https://www.instagram.com/lifeatltts/"
				target="_blank"
				rel="noopener noreferrer"
				class="transition-transform hover:scale-110 hover:text-white"
				aria-label="Instagram"
			>
				<svg class="h-6 w-6 fill-current" viewBox="0 0 448 512">
					<path
						d="M224.1 141c-63.6 0-114.9 51.3-114.9 114.9s51.3 114.9 114.9 114.9S339 319.5 339 255.9 287.7 141 224.1 141zm0 189.6c-41.1 0-74.7-33.5-74.7-74.7s33.5-74.7 74.7-74.7 74.7 33.5 74.7 74.7-33.6 74.7-74.7 74.7zm146.4-194.3c0 14.9-12 26.8-26.8 26.8-14.9 0-26.8-12-26.8-26.8s12-26.8 26.8-26.8 26.8 12 26.8 26.8zm76.1 27.2c-1.7-35.9-9.9-67.7-36.2-93.9-26.2-26.2-58-34.4-93.9-36.2-37-2.1-147.9-2.1-184.9 0-35.8 1.7-67.6 9.9-93.9 36.1s-34.4 58-36.2 93.9c-2.1 37-2.1 147.9 0 184.9 1.7 35.9 9.9 67.7 36.2 93.9s58 34.4 93.9 36.2c37 2.1 147.9 2.1 184.9 0 35.9-1.7 67.7-9.9 93.9-36.2 26.2-26.2 34.4-58 36.2-93.9 2.1-37 2.1-147.8 0-184.8zM398.8 388c-7.8 19.6-22.9 34.7-42.6 42.6-29.5 11.7-99.5 9-132.1 9s-102.7 2.6-132.1-9c-19.6-7.8-34.7-22.9-42.6-42.6-11.7-29.5-9-99.5-9-132.1s-2.6-102.7 9-132.1c7.8-19.6 22.9-34.7 42.6-42.6 29.5-11.7 99.5-9 132.1-9s102.7-2.6 132.1 9c19.6 7.8 34.7 22.9 42.6 42.6 11.7 29.5 9 99.5 9 132.1s2.7 102.7-9 132.1z"
					/>
				</svg>
			</a>
		</div>
	</div>

	<!-- Bottom Bar: Links & Copyright -->
	<div
		class="flex flex-col items-center justify-between gap-3 py-4 px-1 text-sm font-medium text-gray-300 sm:text-base lg:flex-row"
	>
		<div class="flex items-center gap-4">
			<a href="/" class="hover:text-white">Home</a>
			<a href="https://ltts.techgium.com/faq" class="hover:text-white">FAQ</a>
			<a href="https://ltts.techgium.com/gallery" class="hover:text-white">Gallery</a>
			<a href="https://ltts.techgium.com/summary" class="hover:text-white">News & Feed</a>
		</div>
		<p class="text-center">
			© {currentYear} L&T Technology Services Limited. All Rights Reserved.
		</p>
	</div>
</footer>
