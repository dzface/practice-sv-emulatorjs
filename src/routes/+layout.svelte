<script lang="ts">
	import { resolve } from '$app/paths';

	let { children } = $props();
</script>

<svelte:head></svelte:head>
<nav>
	<a href={resolve('/')}>홈</a>
	<a href={resolve('/gba/')}>GBA</a>
	<a href={resolve('/nds/')}>NDS</a>
	<a href={resolve('/info')}>정보</a>
</nav>

<hr />
{#if children}
	{@render children()}
{/if}
<hr />

<style>
	nav {
		width: 100vw;
		& a {
			color: inherit;
			text-decoration: double;
			margin: 0 15px;
			&:hover {
				color: blue;
			}
		}
	}
</style>
