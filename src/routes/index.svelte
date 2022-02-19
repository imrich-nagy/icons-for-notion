<script>
	import { icons } from 'feather-icons';
	import IconTile from '../components/IconTile.svelte';
</script>

<svelte:head>
	<title>Icons for Notion</title>
</svelte:head>

<div class="layout">
	<h1>Icons for Notion</h1>

	<p>Click to copy &mdash; then choose <strong>Link</strong> when adding your icon in Notion.</p>

	<div class="grid">
		{#each Object.values(icons) as icon}
			<IconTile {icon} />
		{/each}
	</div>

	<footer>
		<a href="https://feathericons.com/">Feather icons</a> by
		<a href="https://twitter.com/colebemis">@colebemis</a>
	</footer>
</div>

<style>
	:global(html) {
		font-family: ui-sans-serif, -apple-system, BlinkMacSystemFont, 'Segoe UI', Helvetica,
			'Apple Color Emoji', Arial, sans-serif, 'Segoe UI Emoji', 'Segoe UI Symbol';
		font-size: 16px;
		line-height: 1.5;
		color: #333;
	}

	:global(body) {
		max-width: 1055px;
		padding: 0 16px;
		margin: 160px auto;
	}

	:global(svg) {
		display: block;
	}

	:global(a) {
		color: inherit;
		cursor: pointer;
		text-decoration: inherit;
	}

	.layout,
	.grid {
		display: grid;
		gap: 5px;
		grid-template-columns: repeat(auto-fill, 260px);
		justify-content: center;
	}

	.layout {
		min-width: 320px;
	}

	.layout > * {
		grid-column: 1 / -1;
	}

	h1 {
		font-size: 40px;
		font-weight: 700;
		line-height: 1.2;
		margin-top: 0;
		margin-bottom: 45px;
	}

	p {
		margin: 0;
	}

	.grid {
		margin: 64px 0;
	}

	a {
		opacity: 0.7;
		border-bottom: 0.05em solid rgba(55, 53, 47, 0.4);
	}
</style>
