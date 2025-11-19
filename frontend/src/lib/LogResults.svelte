<script lang="ts">
	export let results: any;

	let expandedLogs = new Set<number>();

	function toggleLog(index: number) {
		if (expandedLogs.has(index)) {
			expandedLogs.delete(index);
		} else {
			expandedLogs.add(index);
		}
		expandedLogs = expandedLogs;
	}

	function getLevelColor(level: string): string {
		const upperLevel = level?.toUpperCase() || 'INFO';
		if (upperLevel.includes('ERROR') || upperLevel.includes('FATAL')) return 'danger';
		if (upperLevel.includes('WARN')) return 'attention';
		if (upperLevel.includes('INFO')) return 'primary';
		if (upperLevel.includes('DEBUG')) return 'success';
		return 'primary';
	}

	function formatTimestamp(ts: string): string {
		try {
			return new Date(ts).toLocaleString();
		} catch {
			return ts;
		}
	}

	function copyToClipboard(text: string) {
		navigator.clipboard.writeText(text);
	}
</script>

<div class="Box mt-4">
	<div class="Box-header d-flex align-items-center justify-content-between">
		<h3 class="m-0 text-medium text-semibold">Query Results</h3>
		<span class="Label Label-primary">
			<strong>{results.total}</strong> logs found
		</span>
	</div>

	{#if results.summary}
		<div class="Box-body border-bottom">
			<div class="flash flash-info m-0">
				<svg width="16" height="16" viewBox="0 0 16 16" fill="currentColor" style="flex-shrink: 0;">
					<path d="M0 8a8 8 0 1 1 16 0A8 8 0 0 1 0 8Zm8-6.5a6.5 6.5 0 1 0 0 13 6.5 6.5 0 0 0 0-13ZM6.5 7.75A.75.75 0 0 1 7.25 7h1a.75.75 0 0 1 .75.75v2.75h.25a.75.75 0 0 1 0 1.5h-2a.75.75 0 0 1 0-1.5h.25v-2h-.25a.75.75 0 0 1-.75-.75ZM8 6a1 1 0 1 1 0-2 1 1 0 0 1 0 2Z"></path>
				</svg>
				<div>
					<strong class="d-block mb-1">AI Summary</strong>
					{results.summary}
				</div>
			</div>
		</div>
	{/if}

	{#if results.total === 0}
		<div class="blankslate m-4">
			<svg class="blankslate-icon" width="48" height="48" viewBox="0 0 16 16" fill="currentColor">
				<path d="M10.68 11.74a6 6 0 0 1-7.922-8.982 6 6 0 0 1 8.982 7.922l3.04 3.04a.749.749 0 0 1-.326 1.275.749.749 0 0 1-.734-.215ZM11.5 7a4.5 4.5 0 1 0-8.997.01A4.5 4.5 0 0 0 11.5 7Z"></path>
			</svg>
			<h3 class="blankslate-heading">No logs found</h3>
			<p class="blankslate-text">Try adjusting your search terms or time range</p>
		</div>
	{:else}
		<div class="Box-body p-0">
			<div class="logs-list">
				{#each results.results as log, index}
					<div class="log-entry" class:expanded={expandedLogs.has(index)}>
						<button 
							class="log-header" 
							on:click={() => toggleLog(index)}
							aria-expanded={expandedLogs.has(index)}
						>
							<span class="Label Label-{getLevelColor(log.level)} log-level">
								{log.level || 'INFO'}
							</span>
							<span class="text-mono text-small color-fg-muted">{formatTimestamp(log.timestamp)}</span>
							{#if log.service}
								<span class="Label Label-primary">{log.service}</span>
							{/if}
							<span class="expand-icon" class:expanded={expandedLogs.has(index)}>
								<svg width="16" height="16" viewBox="0 0 16 16" fill="currentColor">
									<path d="M12.78 5.22a.749.749 0 0 1 0 1.06l-4.25 4.25a.749.749 0 0 1-1.06 0L3.22 6.28a.749.749 0 1 1 1.06-1.06L8 8.939l3.72-3.719a.749.749 0 0 1 1.06 0Z"></path>
								</svg>
							</span>
						</button>

						<div class="log-message">{log.message}</div>

						{#if expandedLogs.has(index)}
							<div class="log-details border-top mt-3 pt-3">
								<div class="mb-3">
									<button
										class="btn btn-sm"
										on:click={() => copyToClipboard(JSON.stringify(log, null, 2))}
									>
										<svg width="16" height="16" viewBox="0 0 16 16" fill="currentColor" style="vertical-align: text-bottom;">
											<path d="M0 6.75C0 5.784.784 5 1.75 5h1.5a.75.75 0 0 1 0 1.5h-1.5a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-1.5a.75.75 0 0 1 1.5 0v1.5A1.75 1.75 0 0 1 9.25 16h-7.5A1.75 1.75 0 0 1 0 14.25Z"></path>
											<path d="M5 1.75C5 .784 5.784 0 6.75 0h7.5C15.216 0 16 .784 16 1.75v7.5A1.75 1.75 0 0 1 14.25 11h-7.5A1.75 1.75 0 0 1 5 9.25Zm1.75-.25a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-7.5a.25.25 0 0 0-.25-.25Z"></path>
										</svg>
										Copy JSON
									</button>
								</div>

								{#if log.trace_id}
									<div class="detail-row mb-2">
										<strong class="text-small color-fg-muted d-block mb-1">Trace ID</strong>
										<code class="code-block p-2 text-small">{log.trace_id}</code>
									</div>
								{/if}

								{#if log.fields && Object.keys(log.fields).length > 0}
									<div class="detail-row">
										<strong class="text-small color-fg-muted d-block mb-1">Additional Fields</strong>
										<pre class="code-block text-small m-0">{JSON.stringify(log.fields, null, 2)}</pre>
									</div>
								{/if}
							</div>
						{/if}
					</div>
				{/each}
			</div>
		</div>
	{/if}
</div>

<style>
	.logs-list {
		display: flex;
		flex-direction: column;
	}

	.log-entry {
		padding: var(--space-4);
		border-bottom: 1px solid var(--color-border-muted);
		transition: background-color 0.2s;
	}

	.log-entry:last-child {
		border-bottom: none;
	}

	.log-entry:hover {
		background-color: var(--color-canvas-subtle);
	}

	.log-entry.expanded {
		background-color: var(--color-canvas-subtle);
	}

	.log-header {
		display: flex;
		align-items: center;
		gap: var(--space-3);
		width: 100%;
		padding: 0;
		background: none;
		border: none;
		cursor: pointer;
		text-align: left;
		font-family: var(--font-sans);
	}

	.log-level {
		text-transform: uppercase;
		min-width: 60px;
		text-align: center;
	}

	.expand-icon {
		margin-left: auto;
		color: var(--color-fg-muted);
		transition: transform 0.2s;
		display: flex;
		align-items: center;
	}

	.expand-icon.expanded {
		transform: rotate(180deg);
	}

	.log-message {
		margin-top: var(--space-2);
		color: var(--color-fg-default);
		line-height: var(--line-height-default);
		word-break: break-word;
	}

	.log-details {
		color: var(--color-fg-muted);
	}

	code {
		font-family: var(--font-mono);
	}
</style>
