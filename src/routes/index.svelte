<script>
	import IconTile from '../components/IconTile.svelte';
	import { icons } from 'feather-icons';
	import fuzzysort from 'fuzzysort';

	let results;
	let searchQuery = '';
	const targets = Object.values(icons).map((icon) => ({ icon }));
	$: results = searchQuery ? search() : targets;

	function search() {
		const tags = Array(7)
			.fill()
			.map((_, x) => `icon.tags.${x}`);
		return fuzzysort
			.go(searchQuery, targets, {
				threshold: -Number.MAX_VALUE,
				limit: Infinity,
				allowTypo: true,
				keys: ['icon.name', ...tags],
				scoreFn: (match) =>
					Math.max(
						match[0] ? match[0].score : -Infinity,
						...match.slice(1).map((x) => (x ? x.score - 10 : -Infinity))
					)
			})
			.map((result) => ({
				icon: result.obj.icon,
				highlight: fuzzysort.highlight(
					result.reduce((previous, current) =>
						!previous || (current && current.score > previous.score) ? current : previous
					),
					'<strong>',
					'</strong>'
				)
			}));
	}

	let searchInput;
	function focusSearch(key) {
		if (key.length !== 1 || document.activeElement.tagName.toLowerCase() === 'input') return;
		searchQuery = '';
		searchInput.focus();
	}

	const searchIcon = icons.search.toSvg({ width: 18, height: 18, color: '#999' });
</script>

<svelte:window on:keydown={(event) => focusSearch(event.key)} />

<svelte:head>
	<title>Icons for Notion</title>
</svelte:head>

<div class="layout">
	<h1>Icons for Notion</h1>

	<p>Click to copy &mdash; then choose <strong>Link</strong> when adding your icon in Notion.</p>

	<div class="search">
		{@html searchIcon}
		<input
			type="search"
			bind:value={searchQuery}
			bind:this={searchInput}
			placeholder="Type to search&hellip;"
		/>
	</div>

	<div class="grid">
		{#each results as result (result.icon.name)}
			<IconTile icon={result.icon} highlight={result.highlight} />
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
		color: rgb(55, 53, 47);
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
		grid-template-columns: repeat(auto-fill, 260px);
		justify-content: center;
		gap: 0 5px;
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
		margin-bottom: 40px;
	}

	p {
		margin-top: 0;
		margin-bottom: 64px;
	}

	.search {
		position: relative;
	}

	.search input {
		font: inherit;
		color: inherit;
		border: none;
		appearance: none;
		appearance: textfield;
		padding: 10px;
		padding-left: 40px;
		box-shadow: rgb(15 15 15 / 10%) 0 0 0 1px, rgb(15 15 15 / 10%) 0 2px 4px;
		border-radius: 3px;
		width: 100%;
	}

	.search input::-webkit-search-cancel-button {
		display: none;
	}

	.search input:focus {
		outline: none;
	}

	.search input::placeholder {
		color: rgba(55 53 47 / 40%);
	}

	.search :global(svg) {
		position: absolute;
		left: 13px;
		top: 13px;
	}

	.grid {
		gap: 5px;
		margin-top: 32px;
	}

	footer {
		margin-top: 64px;
	}

	a {
		opacity: 0.7;
		border-bottom: 0.05em solid rgba(55, 53, 47, 0.4);
	}
</style>
