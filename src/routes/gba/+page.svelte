<script>
	import { base } from '$app/paths';
	// $state 룬을 사용하여 반응형 상태 관리
	let games = $state([]);

	// 컴포넌트 마운트 시 실행되는 효과
	$effect(() => {
		fetch(`${base}/games.json`)
			.then(res => res.json())
			.then(data => games = data.filter(games => games.core === 'gba'));
	});
</script>

<h1>🎮 My Retro Library</h1>

<div class="game-grid">
	{#each games as game}
		<a href="{base}/play?file={encodeURIComponent(game.file)}&core={game.core}">
			<div class="game-card">
				<span class="badge">{game.core}</span>
				<h3>{game.name}</h3>
			</div>
		</a>
	{/each}
</div>

<style>
	.game-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(180px, 1fr)); gap: 15px; }
	.game-card { border: 2px solid #333; padding: 20px; border-radius: 12px; transition: transform 0.2s; }
	.game-card:hover { transform: translateY(-5px); background: #f4f4f4; }
	.badge { font-size: 0.7rem; background: #000; color: #fff; padding: 2px 6px; border-radius: 4px; }
</style>