<script lang="ts">
	let response = '';
	let message = '';
	let loading = false;
	let history: [string, string][] = [];

	async function handleSubmit() {
		console.log(message);
		loading = true;

		history = [...history, ['human', message]];
		message = '';

		const response = await fetch('/api/chat', {
			method: 'POST',
			body: JSON.stringify({ history })
		});
		const data = await response.json();

		history = [...history, ['assistant', data.message]];
		loading = false;
	}
</script>

<main class="flex flex-col items-center justify-center h-screen">
	{#each history as h}
		<div
			class="rounded-xl {h[0] == 'human'
				? 'bg-slate-300 py-1 px-3 m-1 float-right'
				: 'bg-blue-500 py-1 px-3 m-1 text-white float-left'}"
		>
			{h[1]}
		</div>
	{/each}

	<form on:submit={handleSubmit} class="flex gap-2 max-w-md mx-auto mt-4">
		<input
			bind:value={message}
			class="flex-1 px-4 py-2 border rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 w-96"
			placeholder="Type your message..."
		/>
		<button
			type="submit"
			class="w-24 bg-blue-500 text-white px-4 py-2 rounded-md hover:bg-blue-600 transition-colors"
			disabled={loading}
		>
			{loading ? '...' : 'Send'}
		</button>
	</form>
</main>
