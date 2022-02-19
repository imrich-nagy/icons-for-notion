<script>
	import { onMount } from 'svelte';
	import { icons } from 'feather-icons';

	export let icon;
	const svg = icon.toSvg({ width: 14, height: 14, color: '#999' });
	let data = undefined;

	let copied = false;
	const checkIcon = icons.check.toSvg({ width: 14, height: 14, 'stroke-width': 2.5 });

	function formatName() {
		return upperCaseWords(icon.name[0].toUpperCase() + icon.name.slice(1).replace(/-/g, ' '));
	}

	function upperCaseWords(name) {
		return ['AirPlay', 'CPU', 'CW', 'CCW', 'TV', 'RSS', 'WiFi', 'X', 'YouTube'].reduce(
			(previous, current) => previous.replace(new RegExp(`\\b${current}\\b`, 'i'), current),
			name
		);
	}

	function copy() {
		if (data === undefined) return;
		navigator.clipboard.writeText(data).then(() => {
			copied = true;
			setTimeout(() => (copied = false), 2000);
		});
	}

	onMount(() => {
		data = `data:image/svg+xml;base64,${btoa(svg)}`;
	});
</script>

<a on:click={() => copy()} href={data} class="tile" class:copied>
	<div class="icon">
		{@html svg}
	</div>
	<div class="name">{formatName()}</div>
	<span class="copy"
		>{#if copied}{@html checkIcon}{/if}</span
	>
</a>

<style>
	.tile {
		font-size: 14px;
		background-color: #fff;
		transition: background 20ms ease-in;
		box-shadow: rgb(15 15 15 / 10%) 0 0 0 1px, rgb(15 15 15 / 10%) 0 2px 4px;
		border-radius: 3px;
		display: flex;
		padding: 10px;
		gap: 6px;
	}

	.tile:hover {
		background-color: #f9f9f9;
	}

	.tile:active {
		background-color: #f3f3f3;
	}

	.icon {
		margin-top: 4px;
	}

	.name {
		font-weight: 500;
	}

	.copy {
		color: #999;
		margin-left: auto;
		transition: opacity 20ms ease-in;
		display: flex;
		gap: 2px;
	}

	.copy::before {
		content: 'copy';
	}

	.copied .copy::before {
		content: 'copied';
	}

	.copied .copy {
		color: #396;
		font-weight: 500;
	}

	.copy > :global(svg) {
		margin-top: 4px;
	}

	.tile:not(:hover) .copy {
		opacity: 0;
	}
</style>
