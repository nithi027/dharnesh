<script>
	let isOpen = $state(false);
	let view = $state('welcome'); // 'welcome' | 'questions' | 'answer'
	let chatHistory = $state([]);
	let isThinking = $state(false);

	const botData = {
		welcome: {
			title: 'How can we help you today?',
			subtitle: 'Let me know if you have any questions!',
			greeting: "Hello there! 👋 It's nice to meet you!",
			description:
				'What brings you here today? Please use the navigation below or ask me anything about our hackathon platform.'
		},
		questions: [
			{
				id: 1,
				text: 'Student',
				answer: 'Great! Select any category here.',
				followups: [
					{
						id: 11,
						text: 'Registration',
						answer: 'Great! Now select your question here.',
						followups: [
							{
								id: 111,
								text: 'Who can register for TECHgium®?',
								answer:
									'Students from engineering colleges can register and participate in the program.'
							},
							{
								id: 112,
								text: 'Can I register individually?',
								answer: 'Yes, you can register individually for the program.'
							},
							{
								id: 113,
								text: 'Are there any registration Fees?',
								answer:
									'No, there is no registration fees. LTTS does not ask for any kind of fees.'
							},
							{
								id: 114,
								text: 'Should the team members be registered individually?',
								answer: 'Yes, the team members are required to register individually.'
							},
							{
								id: 115,
								text: 'What is the registration process?',
								answer:
									'There are two steps of registration:\n1st is the Institute Registration and 2nd is the Students registration. Institute registration should be done by the institute’s Dean/Placement officer/Director/Principal. Once the institution is registered, a unique institute code will be sent to the Institute representative. The unique code is to be shared with the students with which students can register in the TECHgium portal.'
							}
						]
					},
					{
						id: 12,
						text: 'Abstract Submission',
						answer: 'Great! Now select your question here.',
						followups: [
							{
								id: 121,
								text: 'What is the format for abstract submission?',
								answer:
									'Abstracts should be concise and clearly outline the proposed solution.'
							},
							{
								id: 122,
								text: 'Can I submit multiple abstracts?',
								answer: 'Yes, multiple abstracts can be submitted.'
							},
							{
								id: 123,
								text: 'Can I submit Abstracts for multiple challenges?',
								answer: 'Yes, you can submit for different challenge statements.'
							},
							{
								id: 124,
								text: 'What should the team size and configuration be?',
								answer:
									'You can participate either as an individual (1 member) or as a team (maximum team size: 4 members). There is no restriction on team configuration; team members can be from same or different department(s).'
							},
							{
								id: 125,
								text: 'How should I submit the Abstract?',
								answer:
									'Login (as student) -> Click on Challenge -> Select Challenge Statement -> Submit Abstract.'
							}
						]
					},
					{
						id: 13,
						text: 'Presentation Round',
						answer: 'Great! Now select your question here.',
						followups: [
							{
								id: 131,
								text: 'How will I know if my Abstract is shortlisted?',
								answer:
									'Shortlisted teams will receive a mail from TECHgium team. The shortlisted teams will also be updated in the portal.'
							},
							{
								id: 132,
								text: 'How do I proceed if selected for Stage 2: Technical Paper Presentation?',
								answer:
									'Once your idea/solution gets shortlisted for stage 2, you can start working towards the Presentation file using the provided template.'
							},
							{
								id: 133,
								text: 'How does the presentation happen for Stage 2?',
								answer:
									'Presentation will be held online through Microsoft Teams/Zoom; shortlisted teams will present their concept in detail to a panel of judges.'
							},
							{
								id: 134,
								text: 'Will any templates be shared for presentations?',
								answer:
									'Yes, TECHgium team will share the presentation template with the shortlisted teams.'
							}
						]
					},
					{
						id: 14,
						text: 'Evaluation',
						answer: 'Great! Now select your question here.',
						followups: [
							{
								id: 141,
								text: 'What are the evaluation criteria?',
								answer:
									'Key evaluation criteria include innovation, technical depth, commercial feasibility, and presentation quality.'
							}
						]
					}
				]
			},
			{
				id: 2,
				text: 'Faculty',
				answer: 'Great! Select any category here.',
				followups: [
					{
						id: 21,
						text: 'Student Participation',
						answer: 'Great! Now select your question here.',
						followups: [
							{
								id: 211,
								text: 'How can faculty support student participation?',
								answer:
									'By encouraging students to register, guiding their technical ideation, and helping them refine their abstracts.'
							},
							{
								id: 212,
								text: 'Can faculty be involved in mentoring?',
								answer:
									'Faculty can support and guide students, while official mentorship is additionally provided by LTTS experts.'
							}
						]
					}
				]
			}
		]
	};

	function toggleChat() {
		isOpen = !isOpen;
		if (!isOpen) {
			view = 'welcome';
			chatHistory = [];
		}
	}

	function handleQuestionClick(item) {
		isThinking = true;
		setTimeout(() => {
			isThinking = false;
			chatHistory = [...chatHistory, item];
			view = 'answer';
		}, 300);
	}

	function handleFollowupClick(item) {
		isThinking = true;
		setTimeout(() => {
			isThinking = false;
			chatHistory = [...chatHistory, item];
		}, 400);
	}

	function handleBack() {
		if (chatHistory.length > 1) {
			chatHistory = chatHistory.slice(0, -1);
		} else if (chatHistory.length === 1) {
			chatHistory = [];
			view = 'questions';
		} else {
			view = 'welcome';
		}
	}
</script>

<div class="fixed bottom-6 right-6 z-50 font-figtree">
	{#if isOpen}
		<div
			class="animate-slideUp flex h-96 w-80 flex-col rounded-2xl border border-gray-200 bg-white shadow-2xl sm:h-[480px] sm:w-[400px] md:h-[550px] md:w-[450px]"
		>
			<!-- Chat Header -->
			<div
				class="flex items-center justify-between rounded-t-2xl border-b border-gray-200 bg-neutral-100 p-4"
			>
				<div class="flex items-center space-x-3">
					{#if view !== 'welcome'}
						<button
							type="button"
							onclick={() => {
								view = 'welcome';
								chatHistory = [];
							}}
							class="cursor-pointer rounded-full bg-neutral-200 p-2 text-gray-600 transition-colors duration-200 hover:bg-gray-300"
							aria-label="Reset chat"
						>
							<svg class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
								<path
									stroke-linecap="round"
									stroke-linejoin="round"
									stroke-width="2"
									d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15"
								/>
							</svg>
						</button>
					{/if}

					<div class="flex items-center space-x-2">
						<div class="flex h-8 w-8 items-center justify-center rounded-full bg-blue-600">
							<svg
								class="h-4 w-4 text-white"
								fill="none"
								viewBox="0 0 24 24"
								stroke="currentColor"
							>
								<path
									stroke-linecap="round"
									stroke-linejoin="round"
									stroke-width="2"
									d="M8 10h.01M12 10h.01M16 10h.01M9 16H5a2 2 0 01-2-2V6a2 2 0 012-2h14a2 2 0 012 2v8a2 2 0 01-2 2h-5l-5 5v-5z"
								/>
							</svg>
						</div>
						<span class="font-semibold text-gray-800">ChatBot</span>
					</div>
				</div>

				<div class="flex items-center space-x-2">
					{#if view === 'answer' || view === 'questions'}
						<button
							type="button"
							onclick={handleBack}
							class="mr-1 flex cursor-pointer items-center gap-1 rounded-full bg-neutral-200 px-3 py-1 text-xs font-medium text-gray-700 transition-colors duration-200 hover:bg-gray-300"
						>
							<svg class="h-3.5 w-3.5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
								<path
									stroke-linecap="round"
									stroke-linejoin="round"
									stroke-width="2"
									d="M15 19l-7-7 7-7"
								/>
							</svg>
							Back
						</button>
					{/if}

					<button
						type="button"
						onclick={toggleChat}
						class="cursor-pointer rounded-full bg-neutral-200 p-2 text-gray-600 transition-colors duration-200 hover:bg-gray-300"
						aria-label="Close chat"
					>
						<svg class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
							<path
								stroke-linecap="round"
								stroke-linejoin="round"
								stroke-width="2"
								d="M6 18L18 6M6 6l12 12"
							/>
						</svg>
					</button>
				</div>
			</div>

			<!-- Chat Body -->
			<div class="flex-1 overflow-y-auto p-4">
				{#if view === 'welcome'}
					<!-- Welcome Screen -->
					<div class="animate-fadeIn flex h-full flex-col justify-around gap-3 p-2">
						<div class="space-y-1 text-center">
							<div
								class="mx-auto flex h-12 w-12 items-center justify-center rounded-full bg-blue-600"
							>
								<svg
									class="h-6 w-6 text-white"
									fill="none"
									viewBox="0 0 24 24"
									stroke="currentColor"
								>
									<path
										stroke-linecap="round"
										stroke-linejoin="round"
										stroke-width="2"
										d="M14.828 14.828a4 4 0 01-5.656 0M9 10h.01M15 10h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"
									/>
								</svg>
							</div>
							<h3 class="text-2xl font-bold text-gray-800">{botData.welcome.title}</h3>
							<p class="text-sm text-gray-600">{botData.welcome.subtitle}</p>
						</div>

						<div class="rounded-xl border border-blue-100 bg-blue-50 px-4 py-3">
							<div class="mb-1.5 flex items-center space-x-2">
								<div class="flex h-7 w-7 items-center justify-center rounded-full bg-blue-600">
									<svg
										class="h-3.5 w-3.5 text-white"
										fill="none"
										viewBox="0 0 24 24"
										stroke="currentColor"
									>
										<path
											stroke-linecap="round"
											stroke-linejoin="round"
											stroke-width="2"
											d="M8 12h.01M12 12h.01M16 12h.01M21 12c0 4.418-4.03 8-9 8a9.863 9.863 0 01-4.255-.949L3 20l1.395-3.72C3.512 15.042 3 13.574 3 12c0-4.418 4.03-8 9-8s9 3.582 9 8z"
										/>
									</svg>
								</div>
								<span class="font-medium text-gray-800">ChatBot</span>
							</div>
							<p class="text-sm leading-relaxed text-gray-700">{botData.welcome.greeting}</p>
						</div>

						<button
							type="button"
							onclick={() => (view = 'questions')}
							class="flex w-full cursor-pointer items-center justify-center space-x-2 rounded-xl bg-brand-blue-500 px-4 py-3 font-medium text-white duration-200 hover:bg-brand-blue-700"
						>
							<span>Chat with us</span>
							<svg class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
								<path
									stroke-linecap="round"
									stroke-linejoin="round"
									stroke-width="2"
									d="M14 5l7 7m0 0l-7 7m7-7H3"
								/>
							</svg>
						</button>
					</div>
				{:else if view === 'questions'}
					<!-- Question Categories Screen -->
					<div class="animate-fadeIn space-y-5 p-2">
						<div class="space-y-2 text-center">
							<div
								class="mx-auto flex h-10 w-10 items-center justify-center rounded-full bg-brand-blue-500"
							>
								<svg
									class="h-5 w-5 text-white"
									fill="none"
									viewBox="0 0 24 24"
									stroke="currentColor"
								>
									<path
										stroke-linecap="round"
										stroke-linejoin="round"
										stroke-width="2"
										d="M8.228 9c.549-1.165 2.03-2 3.772-2 2.21 0 4 1.343 4 3 0 1.4-1.278 2.575-3.006 2.907-.542.104-.994.54-.994 1.093m0 3h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"
									/>
								</svg>
							</div>
							<h3 class="text-xl font-bold text-gray-800">ChatBot</h3>
							<p class="text-sm leading-relaxed text-gray-600">{botData.welcome.description}</p>
						</div>

						<div class="grid grid-cols-1 gap-3">
							{#each botData.questions as q}
								<button
									type="button"
									onclick={() => handleQuestionClick(q)}
									class="group flex cursor-pointer items-center space-x-3 rounded-xl border border-gray-200 bg-white p-4 text-left duration-200 hover:border-blue-300 hover:bg-blue-50"
								>
									<div
										class="flex h-8 w-8 items-center justify-center rounded-lg bg-blue-100 text-brand-blue-500 duration-200 group-hover:scale-110"
									>
										{#if q.id === 1}
											<!-- Student Icon -->
											<svg class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
												<path
													stroke-linecap="round"
													stroke-linejoin="round"
													stroke-width="2"
													d="M12 14l9-5-9-5-9 5 9 5zm0 0l6.16-3.422a12.083 12.083 0 01.665 6.479A11.952 11.952 0 0012 20.055a11.952 11.952 0 00-6.824-2.998 12.078 12.078 0 01.665-6.479L12 14zm-4 6v-7.5"
												/>
											</svg>
										{:else}
											<!-- Faculty Icon -->
											<svg class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
												<path
													stroke-linecap="round"
													stroke-linejoin="round"
													stroke-width="2"
													d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"
												/>
											</svg>
										{/if}
									</div>
									<span class="text-sm font-semibold text-gray-800 group-hover:text-brand-blue-500">
										{q.text}
									</span>
								</button>
							{/each}
						</div>
					</div>
				{:else if view === 'answer'}
					<!-- Conversation Stream -->
					<div class="animate-fadeIn space-y-4">
						{#each chatHistory as item, index}
							<!-- User Message -->
							<div class="flex justify-end">
								<div class="max-w-[80%] rounded-xl bg-blue-600 px-4 py-2 text-sm text-white shadow">
									{item.text}
								</div>
							</div>

							<!-- Bot Message -->
							<div class="flex items-start justify-start space-x-2">
								<div
									class="mt-1 flex h-7 w-7 shrink-0 items-center justify-center rounded-full bg-blue-600"
								>
									<svg
										class="h-3.5 w-3.5 text-white"
										fill="none"
										viewBox="0 0 24 24"
										stroke="currentColor"
									>
										<path
											stroke-linecap="round"
											stroke-linejoin="round"
											stroke-width="2"
											d="M8 10h.01M12 10h.01M16 10h.01M9 16H5a2 2 0 01-2-2V6a2 2 0 012-2h14a2 2 0 012 2v8a2 2 0 01-2 2h-5l-5 5v-5z"
										/>
									</svg>
								</div>
								<div
									class="max-w-[80%] whitespace-pre-line rounded-xl bg-gray-100 px-4 py-2.5 text-sm text-gray-800 shadow-sm leading-relaxed"
								>
									{item.answer}
								</div>
							</div>

							<!-- Follow-up Options if this is the last item -->
							{#if index === chatHistory.length - 1 && item.followups && item.followups.length > 0}
								<div class="ml-9 space-y-2 pt-2">
									<p class="text-xs font-semibold uppercase tracking-wider text-gray-500">
										Choose an option:
									</p>
									<div class="grid grid-cols-1 gap-2">
										{#each item.followups as f}
											<button
												type="button"
												onclick={() => handleFollowupClick(f)}
												class="cursor-pointer rounded-xl border border-gray-200 bg-white px-3 py-2 text-left text-xs font-medium text-gray-700 duration-200 hover:border-blue-300 hover:bg-blue-50 hover:text-blue-700"
											>
												{f.text}
											</button>
										{/each}
									</div>
								</div>
							{/if}
						{/each}

						{#if isThinking}
							<div class="ml-9 flex items-center space-x-2">
								<div class="h-2 w-2 animate-bounce rounded-full bg-blue-600"></div>
								<div
									class="h-2 w-2 animate-bounce rounded-full bg-blue-600 [animation-delay:0.2s]"
								></div>
								<div
									class="h-2 w-2 animate-bounce rounded-full bg-blue-600 [animation-delay:0.4s]"
								></div>
								<span class="text-xs italic text-gray-500">ChatBot is typing...</span>
							</div>
						{/if}
					</div>
				{/if}
			</div>

			<!-- Footer info -->
			<div class="rounded-b-2xl border-t border-gray-200 bg-neutral-50 p-2.5 text-center">
				<span class="text-xs font-medium text-blue-600">TECHgium®</span>
			</div>
		</div>
	{:else}
		<!-- Floating Trigger Button -->
		<button
			type="button"
			onclick={toggleChat}
			class="flex h-14 w-14 cursor-pointer items-center justify-center rounded-full bg-blue-600 text-white shadow-xl duration-300 hover:bg-blue-700 hover:scale-105"
			aria-label="Open ChatBot"
		>
			<svg class="h-6 w-6 text-white" fill="none" viewBox="0 0 24 24" stroke="currentColor">
				<path
					stroke-linecap="round"
					stroke-linejoin="round"
					stroke-width="2"
					d="M8 10h.01M12 10h.01M16 10h.01M9 16H5a2 2 0 01-2-2V6a2 2 0 012-2h14a2 2 0 012 2v8a2 2 0 01-2 2h-5l-5 5v-5z"
				/>
			</svg>
		</button>
	{/if}
</div>
