<script lang="ts">
	import { createEventDispatcher } from 'svelte';

	export let config: any;

	const dispatch = createEventDispatcher();

	let localConfig = { ...config };
	let activeTab = 'logs'; // 'logs' or 'ai'

	function save() {
		if (!localConfig.url) {
			alert('Please enter a URL');
			return;
		}
		dispatch('save', localConfig);
	}

	function close() {
		dispatch('close');
	}
</script>

<div class="overlay" on:click={close} on:keydown={(e) => e.key === 'Escape' && close()}>
	<div class="modal" on:click|stopPropagation on:keydown|stopPropagation>
		<div class="modal-header">
			<h2>⚙️ Settings</h2>
			<button class="close-btn" on:click={close}>✕</button>
		</div>

		<!-- Tab Navigation -->
		<div class="tab-navigation">
			<button 
				class="tab-btn" 
				class:active={activeTab === 'logs'}
				on:click={() => activeTab = 'logs'}
			>
				📊 Log Sources
			</button>
			<button 
				class="tab-btn" 
				class:active={activeTab === 'ai'}
				on:click={() => activeTab = 'ai'}
			>
				🤖 AI Providers
			</button>
		</div>

		<div class="modal-body">
			<!-- Log Sources Tab -->
			{#if activeTab === 'logs'}
				<div class="form-group">
					<label>
						Log Source
						<select bind:value={localConfig.source}>
							<option value="elasticsearch">Elasticsearch / Kibana</option>
							<option value="openobserve">OpenObserve</option>
						</select>
					</label>
				</div>

				<div class="form-group">
					<label>
						URL *
						<input
							type="text"
							bind:value={localConfig.url}
							placeholder={localConfig.source === 'elasticsearch'
								? 'http://localhost:9200'
								: 'http://localhost:5080'}
							required
						/>
					</label>
					<small>
						{#if localConfig.source === 'elasticsearch'}
							Example: http://localhost:9200 or https://your-cluster.es.io:9200
						{:else}
							Example: http://localhost:5080
						{/if}
					</small>
				</div>

				<div class="form-group">
					<label>
						Username
						<input
							type="text"
							bind:value={localConfig.username}
							placeholder={localConfig.source === 'elasticsearch'
								? 'elastic'
								: 'root@example.com'}
						/>
					</label>
				</div>

				<div class="form-group">
					<label>
						Password
						<input type="password" bind:value={localConfig.password} placeholder="••••••••" />
					</label>
				</div>

				{#if localConfig.source === 'openobserve'}
					<div class="form-group">
						<label>
							Organization
							<input
								type="text"
								bind:value={localConfig.organization}
								placeholder="default"
							/>
						</label>
					</div>
				{/if}

				<div class="info-box">
					<h4>🔒 Security Note</h4>
					<p>
						Credentials are stored locally in your browser and sent directly to your log source.
						They are never stored on any server.
					</p>
				</div>
			{/if}

			<!-- AI Providers Tab -->
			{#if activeTab === 'ai'}
				<div class="info-box info">
					<h4>ℹ️ AI Provider Configuration</h4>
					<p>
						AI provider settings are configured via environment variables for security.
						API keys should never be stored in the browser.
					</p>
				</div>

				<div class="provider-section">
					<h3>📚 Supported Providers</h3>
					
					<div class="provider-card">
						<div class="provider-header">
							<strong>OpenAI</strong>
							<span class="badge">Recommended</span>
						</div>
						<p>Direct access to GPT-4o, GPT-4o-mini, and other OpenAI models.</p>
						<div class="config-example">
							<strong>Configuration:</strong>
							<pre><code>AI_PROVIDER=openai
OPENAI_API_KEY=sk-...
AI_MODEL=gpt-4o-mini</code></pre>
						</div>
					</div>

					<div class="provider-card">
						<div class="provider-header">
							<strong>Azure OpenAI</strong>
							<span class="badge enterprise">Enterprise</span>
						</div>
						<p>Enterprise-grade OpenAI access via Microsoft Azure with compliance and data residency.</p>
						<div class="config-example">
							<strong>Configuration:</strong>
							<pre><code>AI_PROVIDER=azure-openai
AZURE_OPENAI_API_KEY=...
AZURE_OPENAI_ENDPOINT=https://...
AZURE_OPENAI_DEPLOYMENT=gpt-4o-mini
AZURE_OPENAI_API_VERSION=2024-02-15-preview</code></pre>
						</div>
					</div>

					<div class="provider-card">
						<div class="provider-header">
							<strong>Google Gemini</strong>
							<span class="badge">TypeScript API Only</span>
						</div>
						<p>Cost-effective Google AI models with competitive pricing.</p>
						<div class="config-example">
							<strong>Configuration (Vercel):</strong>
							<pre><code>AI_PROVIDER=google-ai
AI_API_KEY=AIzaSy...
AI_MODEL=gemini-pro</code></pre>
						</div>
					</div>

					<div class="provider-card">
						<div class="provider-header">
							<strong>Anthropic Claude</strong>
							<span class="badge">TypeScript API Only</span>
						</div>
						<p>200K context window with strong reasoning capabilities.</p>
						<div class="config-example">
							<strong>Configuration (Vercel):</strong>
							<pre><code>AI_PROVIDER=anthropic
AI_API_KEY=sk-ant-...
AI_MODEL=claude-3-sonnet-20240229</code></pre>
						</div>
					</div>
				</div>

				<div class="info-box docs">
					<h4>📖 Full Documentation</h4>
					<p>
						For complete provider setup instructions, see:
						<a href="https://github.com/adarshba/zeteo/blob/main/docs/MODEL-PROVIDERS.md" target="_blank" rel="noopener">
							docs/MODEL-PROVIDERS.md
						</a>
					</p>
				</div>

				<div class="deployment-info">
					<h3>🚀 Deployment-Specific Configuration</h3>
					
					<div class="deployment-card">
						<strong>Vercel Deployment:</strong>
						<p>Set environment variables in Vercel Dashboard → Settings → Environment Variables</p>
					</div>

					<div class="deployment-card">
						<strong>Local Rust Backend:</strong>
						<p>Create <code>backend/.env</code> file with your provider configuration</p>
					</div>

					<div class="deployment-card">
						<strong>Local TypeScript API:</strong>
						<p>Create <code>.env</code> file in project root with your provider configuration</p>
					</div>
				</div>
			{/if}
		</div>

		<div class="modal-footer">
			{#if activeTab === 'logs'}
				<button class="btn-secondary" on:click={close}>Cancel</button>
				<button class="btn-primary" on:click={save}>Save Settings</button>
			{:else}
				<button class="btn-primary" on:click={close}>Close</button>
			{/if}
		</div>
	</div>
</div>

<style>
	.overlay {
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		background: rgba(0, 0, 0, 0.7);
		display: flex;
		align-items: center;
		justify-content: center;
		z-index: 1000;
	}

	.modal {
		background: white;
		border-radius: 12px;
		width: 90%;
		max-width: 700px;
		max-height: 90vh;
		overflow: auto;
		box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
	}

	.modal-header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 20px 30px;
		border-bottom: 1px solid #eee;
	}

	.modal-header h2 {
		margin: 0;
		color: #333;
	}

	.close-btn {
		background: none;
		border: none;
		font-size: 1.5em;
		cursor: pointer;
		color: #999;
		padding: 0;
		width: 30px;
		height: 30px;
		display: flex;
		align-items: center;
		justify-content: center;
		border-radius: 4px;
	}

	.close-btn:hover {
		background: #f5f5f5;
		color: #333;
	}

	.tab-navigation {
		display: flex;
		border-bottom: 2px solid #eee;
		background: #f9f9f9;
	}

	.tab-btn {
		flex: 1;
		padding: 15px 20px;
		background: none;
		border: none;
		border-bottom: 3px solid transparent;
		cursor: pointer;
		font-size: 1em;
		font-weight: 500;
		color: #666;
		transition: all 0.3s;
	}

	.tab-btn:hover {
		background: #f0f0f0;
		color: #333;
	}

	.tab-btn.active {
		background: white;
		color: #667eea;
		border-bottom-color: #667eea;
	}

	.modal-body {
		padding: 30px;
		max-height: 500px;
		overflow-y: auto;
	}

	.form-group {
		margin-bottom: 20px;
	}

	.form-group label {
		display: block;
		font-weight: 600;
		margin-bottom: 8px;
		color: #333;
	}

	.form-group input,
	.form-group select {
		width: 100%;
		padding: 12px;
		border: 2px solid #e0e0e0;
		border-radius: 8px;
		font-size: 1em;
		transition: border-color 0.3s;
		box-sizing: border-box;
	}

	.form-group input:focus,
	.form-group select:focus {
		outline: none;
		border-color: #667eea;
	}

	.form-group small {
		display: block;
		margin-top: 5px;
		color: #666;
		font-size: 0.9em;
	}

	.info-box {
		background: #f0f7ff;
		border: 1px solid #b3d9ff;
		border-radius: 8px;
		padding: 15px;
		margin-top: 20px;
	}

	.info-box.info {
		background: #e7f3ff;
		border-color: #90c9ff;
	}

	.info-box.docs {
		background: #f0f9f4;
		border-color: #90d7a8;
	}

	.info-box h4 {
		margin: 0 0 8px 0;
		color: #0066cc;
		font-size: 0.95em;
	}

	.info-box.docs h4 {
		color: #00994d;
	}

	.info-box p {
		margin: 0;
		font-size: 0.9em;
		color: #0066cc;
		line-height: 1.5;
	}

	.info-box.docs p {
		color: #006633;
	}

	.info-box a {
		color: #006633;
		text-decoration: underline;
		font-weight: 600;
	}

	.provider-section {
		margin: 20px 0;
	}

	.provider-section h3 {
		color: #333;
		margin: 0 0 15px 0;
		font-size: 1.1em;
	}

	.provider-card {
		background: #f9f9f9;
		border: 1px solid #e0e0e0;
		border-radius: 8px;
		padding: 15px;
		margin-bottom: 15px;
	}

	.provider-header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin-bottom: 8px;
	}

	.provider-header strong {
		font-size: 1.05em;
		color: #333;
	}

	.badge {
		display: inline-block;
		padding: 3px 10px;
		background: #667eea;
		color: white;
		border-radius: 12px;
		font-size: 0.75em;
		font-weight: 600;
	}

	.badge.enterprise {
		background: #ff9800;
	}

	.provider-card p {
		margin: 0 0 10px 0;
		color: #666;
		font-size: 0.9em;
		line-height: 1.4;
	}

	.config-example {
		margin-top: 10px;
	}

	.config-example strong {
		display: block;
		margin-bottom: 5px;
		color: #555;
		font-size: 0.85em;
	}

	.config-example pre {
		background: #2d2d2d;
		color: #f8f8f2;
		padding: 10px;
		border-radius: 4px;
		overflow-x: auto;
		margin: 0;
		font-size: 0.8em;
		line-height: 1.5;
	}

	.config-example code {
		font-family: 'Monaco', 'Menlo', 'Ubuntu Mono', monospace;
	}

	.deployment-info {
		margin: 20px 0;
	}

	.deployment-info h3 {
		color: #333;
		margin: 0 0 15px 0;
		font-size: 1.1em;
	}

	.deployment-card {
		background: #fff8e7;
		border: 1px solid #ffd966;
		border-radius: 8px;
		padding: 12px;
		margin-bottom: 10px;
	}

	.deployment-card strong {
		display: block;
		color: #cc8800;
		margin-bottom: 5px;
		font-size: 0.95em;
	}

	.deployment-card p {
		margin: 0;
		color: #666;
		font-size: 0.85em;
		line-height: 1.4;
	}

	.deployment-card code {
		background: #fff;
		padding: 2px 6px;
		border-radius: 3px;
		font-size: 0.9em;
		color: #d63384;
		font-family: 'Monaco', 'Menlo', 'Ubuntu Mono', monospace;
	}

	.modal-footer {
		padding: 20px 30px;
		border-top: 1px solid #eee;
		display: flex;
		justify-content: flex-end;
		gap: 10px;
	}

	.btn-primary,
	.btn-secondary {
		padding: 12px 24px;
		border: none;
		border-radius: 8px;
		font-size: 1em;
		cursor: pointer;
		transition: all 0.3s;
	}

	.btn-primary {
		background: #667eea;
		color: white;
	}

	.btn-primary:hover {
		background: #5568d3;
		transform: translateY(-2px);
		box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
	}

	.btn-secondary {
		background: #f5f5f5;
		color: #333;
	}

	.btn-secondary:hover {
		background: #e0e0e0;
	}
</style>
