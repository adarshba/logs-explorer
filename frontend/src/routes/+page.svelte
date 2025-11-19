<script lang="ts">
	import { onMount } from 'svelte';
	import Settings from '../lib/Settings.svelte';
	import QueryInterface from '../lib/QueryInterface.svelte';
	import LogResults from '../lib/LogResults.svelte';

	let showSettings = false;
	let config = {
		source: 'elasticsearch',
		url: '',
		username: '',
		password: '',
		organization: 'default'
	};

	let queryResults: any = null;
	let isLoading = false;
	let error = '';

	// Load config from localStorage
	onMount(() => {
		const saved = localStorage.getItem('logsExplorerConfig');
		if (saved) {
			config = JSON.parse(saved);
		} else {
			showSettings = true;
		}
	});

	function saveConfig(newConfig: any) {
		config = newConfig;
		localStorage.setItem('logsExplorerConfig', JSON.stringify(config));
		showSettings = false;
	}

	async function handleQuery(event: CustomEvent<string>) {
		const query = event.detail;
		isLoading = true;
		error = '';
		queryResults = null;

		try {
			const response = await fetch('/api/query', {
				method: 'POST',
				headers: { 'Content-Type': 'application/json' },
				body: JSON.stringify({
					query,
					source: config.source,
					config: {
						url: config.url,
						username: config.username || undefined,
						password: config.password || undefined,
						organization: config.organization
					}
				})
			});

			if (!response.ok) {
				const errorText = await response.text();
				throw new Error(errorText || 'Query failed');
			}

			queryResults = await response.json();
		} catch (err: any) {
			error = err.message || 'An error occurred';
			console.error('Query error:', err);
		} finally {
			isLoading = false;
		}
	}
</script>

<svelte:head>
	<title>Zeteo - AI-Powered Log Analysis</title>
</svelte:head>

<div class="app">
	<header class="Header">
		<div class="d-flex align-items-center flex-1 gap-3">
			<svg width="24" height="24" viewBox="0 0 16 16" fill="currentColor" aria-hidden="true">
				<path d="M10.68 11.74a6 6 0 0 1-7.922-8.982 6 6 0 0 1 8.982 7.922l3.04 3.04a.749.749 0 0 1-.326 1.275.749.749 0 0 1-.734-.215ZM11.5 7a4.5 4.5 0 1 0-8.997.01A4.5 4.5 0 0 0 11.5 7Z"></path>
			</svg>
			<h1 class="m-0 text-large text-bold">Zeteo</h1>
			<span class="color-fg-muted text-normal">AI-powered log analysis</span>
		</div>
		<button class="btn btn-sm" on:click={() => (showSettings = !showSettings)}>
			<svg width="16" height="16" viewBox="0 0 16 16" fill="currentColor" style="vertical-align: text-bottom;">
				<path d="M8 0a8.2 8.2 0 0 1 .701.031C9.444.095 9.99.645 10.16 1.29l.288 1.107c.018.066.079.158.212.224.231.114.454.243.668.386.123.082.233.09.299.071l1.103-.303c.644-.176 1.392.021 1.82.63.27.385.506.792.704 1.218.315.675.111 1.422-.364 1.891l-.814.806c-.049.048-.098.147-.088.294.016.257.016.515 0 .772-.01.147.038.246.088.294l.814.806c.475.469.679 1.216.364 1.891a7.977 7.977 0 0 1-.704 1.218c-.428.609-1.176.806-1.82.63l-1.103-.303c-.066-.019-.176-.011-.299.071a4.49 4.49 0 0 1-.668.386c-.133.066-.194.158-.212.224l-.288 1.107c-.17.645-.716 1.195-1.459 1.26a8.006 8.006 0 0 1-1.402 0c-.743-.065-1.289-.615-1.459-1.26l-.288-1.107c-.018-.066-.079-.158-.212-.224a4.468 4.468 0 0 1-.668-.386c-.123-.082-.233-.09-.299-.071l-1.103.303c-.644.176-1.392-.021-1.82-.63a8.084 8.084 0 0 1-.704-1.218c-.315-.675-.111-1.422.363-1.891l.815-.806c.05-.048.098-.147.088-.294a6.214 6.214 0 0 1 0-.772c.01-.147-.038-.246-.088-.294l-.815-.806C.635 6.045.431 5.298.746 4.623a7.92 7.92 0 0 1 .704-1.217c.428-.61 1.176-.807 1.82-.63l1.102.302c.067.019.177.011.3-.071.214-.143.437-.272.668-.386.133-.066.194-.158.212-.224l.288-1.107C5.71.645 6.256.095 7 .03 7.295.017 7.587 0 7.889 0h.222ZM7 2.382a5.903 5.903 0 0 0-.535.02c-.139.023-.276.13-.314.314l-.288 1.107c-.144.543-.52.976-.99 1.222a2.977 2.977 0 0 0-.445.257c-.433.28-.99.349-1.486.19l-1.103-.303a.593.593 0 0 0-.64.21 6.083 6.083 0 0 0-.535.92c-.096.212-.059.46.106.67l.815.806c.411.407.562.98.53 1.524a4.713 4.713 0 0 0 0 .514c.032.544-.119 1.117-.53 1.524l-.815.806a.593.593 0 0 0-.106.67c.155.323.335.631.535.92.14.196.359.277.64.21l1.103-.303c.496-.159 1.053-.09 1.486.19.154.1.302.186.445.257.47.246.846.679.99 1.222l.288 1.107c.038.184.175.291.314.314.359.06.724.06 1.07 0 .139-.023.276-.13.314-.314l.288-1.107c.144-.543.52-.976.99-1.222.143-.071.291-.157.445-.257.433-.28.99-.349 1.486-.19l1.103.303c.281.067.5-.014.64-.21.2-.289.38-.597.535-.92a.593.593 0 0 0-.106-.67l-.815-.806c-.411-.407-.562-.98-.53-1.524a4.643 4.643 0 0 0 0-.514c-.032-.544.119-1.117.53-1.524l.815-.806a.593.593 0 0 0 .106-.67 6.083 6.083 0 0 0-.535-.92c-.14-.196-.359-.277-.64-.21l-1.103.303c-.496.159-1.053.09-1.486-.19a2.977 2.977 0 0 0-.445-.257c-.47-.246-.846-.679-.99-1.222l-.288-1.107c-.038-.184-.175-.291-.314-.314A5.903 5.903 0 0 0 8 2.382ZM8 5.5a2.5 2.5 0 1 1 0 5 2.5 2.5 0 0 1 0-5ZM8 7a1 1 0 1 0 0 2 1 1 0 0 0 0-2Z"></path>
			</svg>
			Settings
		</button>
	</header>

	{#if showSettings}
		<Settings {config} on:save={(e) => saveConfig(e.detail)} on:close={() => (showSettings = false)} />
	{/if}

	<main class="container mt-4">
		<QueryInterface on:query={handleQuery} {isLoading} />

		{#if error}
			<div class="flash flash-danger mt-4">
				<svg width="16" height="16" viewBox="0 0 16 16" fill="currentColor" style="flex-shrink: 0;">
					<path d="M2.343 13.657A8 8 0 1 1 13.657 2.343 8 8 0 0 1 2.343 13.657ZM6.03 4.97a.751.751 0 0 0-1.042.018.751.751 0 0 0-.018 1.042L6.94 8 4.97 9.97a.749.749 0 0 0 .326 1.275.749.749 0 0 0 .734-.215L8 9.06l1.97 1.97a.749.749 0 0 0 1.275-.326.749.749 0 0 0-.215-.734L9.06 8l1.97-1.97a.749.749 0 0 0-.326-1.275.749.749 0 0 0-.734.215L8 6.94Z"></path>
				</svg>
				<div>
					<strong>Error</strong>
					<p class="m-0">{error}</p>
				</div>
			</div>
		{/if}

		{#if isLoading}
			<div class="Box mt-4">
				<div class="Box-body text-center p-6">
					<div class="loading-spinner-large" style="margin: 0 auto var(--space-3);"></div>
					<p class="color-fg-muted m-0">Analyzing logs with AI...</p>
				</div>
			</div>
		{/if}

		{#if queryResults}
			<LogResults results={queryResults} />
		{/if}
	</main>

	<footer class="mt-6 p-4 text-center color-fg-muted text-small border-top">
		<p class="m-0">
			Powered by Neurolink AI · 
			<a href="https://github.com/adarshba/zeteo" target="_blank" class="color-accent">GitHub</a>
		</p>
	</footer>
</div>

<style>
	.app {
		min-height: 100vh;
		display: flex;
		flex-direction: column;
	}

	main {
		flex: 1;
	}

	a {
		text-decoration: none;
	}

	a:hover {
		text-decoration: underline;
	}
</style>
