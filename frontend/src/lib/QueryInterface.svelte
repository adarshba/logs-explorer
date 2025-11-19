<script lang="ts">
	import { createEventDispatcher } from 'svelte';

	export let isLoading = false;

	let query = '';
	const dispatch = createEventDispatcher();

	const exampleQueries = [
		'Show recent errors',
		'Database timeouts in last hour',
		'Payment service errors',
		'Authentication failures',
		'Slow requests over 2 seconds'
	];

	function submitQuery() {
		if (query.trim() && !isLoading) {
			dispatch('query', query);
		}
	}

	function useExample(example: string) {
		query = example;
		submitQuery();
	}
</script>

<div class="Box">
	<div class="Box-header">
		<h2 class="m-0 text-medium text-semibold">Ask a question about your logs</h2>
	</div>
	<div class="Box-body">
		<form on:submit|preventDefault={submitQuery}>
			<div class="d-flex gap-2 mb-3">
				<input
					type="text"
					class="form-control flex-1"
					bind:value={query}
					placeholder="e.g., Show me all errors from the last hour"
					disabled={isLoading}
					style="font-size: var(--font-size-medium); padding: 8px 12px;"
				/>
				<button type="submit" class="btn btn-primary" disabled={isLoading || !query.trim()}>
					{#if isLoading}
						<span class="loading-spinner"></span>
					{:else}
						<svg width="16" height="16" viewBox="0 0 16 16" fill="currentColor" style="vertical-align: text-bottom; margin-right: 4px;">
							<path d="M10.68 11.74a6 6 0 0 1-7.922-8.982 6 6 0 0 1 8.982 7.922l3.04 3.04a.749.749 0 0 1-.326 1.275.749.749 0 0 1-.734-.215ZM11.5 7a4.5 4.5 0 1 0-8.997.01A4.5 4.5 0 0 0 11.5 7Z"></path>
						</svg>
						Query
					{/if}
				</button>
			</div>
		</form>

		<div class="border-top pt-3">
			<p class="text-small color-fg-muted mb-2">Try these examples:</p>
			<div class="d-flex flex-wrap gap-2">
				{#each exampleQueries as example}
					<button
						class="btn btn-sm"
						on:click={() => useExample(example)}
						disabled={isLoading}
					>
						{example}
					</button>
				{/each}
			</div>
		</div>
	</div>
</div>
