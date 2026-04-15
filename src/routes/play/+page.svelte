<script>
	import { page } from '$app/stores';
	import { base } from '$app/paths';

	// URL 파라미터에서 정보 추출
	let gameFile = $derived($page.url.searchParams.get('file'));
	let gameCore = $derived($page.url.searchParams.get('core') || 'gba');

	$effect(() => {
		if (!gameFile) return;

		// 1. 에뮬레이터 설정 (중요: pathtodata를 외부 주소로!)
		window.EJS_player = '#game-container';
		window.EJS_gameUrl = `${base}${gameFile}`; // ROM은 내 서버의 것을 사용
		window.EJS_core = gameCore;
		
		// 파일이 없는 로컬 대신 공식 서버의 데이터 세트를 사용합니다.
		window.EJS_pathtodata = 'https://cdn.emulatorjs.org/stable/data/';

		// 2. 로더 스크립트 동적 주입
		const SCRIPT_ID = 'ejs-loader';
		const existing = document.getElementById(SCRIPT_ID);
		if (existing) existing.remove();

		const script = document.createElement('script');
		script.id = SCRIPT_ID;
		script.src = 'https://cdn.emulatorjs.org/stable/data/loader.js';
		document.head.appendChild(script);

		return () => {
			const container = document.getElementById('game-container');
			if (container) container.innerHTML = '';
		};
	});
</script>

<div class="emulator-wrapper">
	<nav>
		<a href="{base}/">⬅ 목록으로</a>
	</nav>
	<div id="game-container"></div>
</div>

<style>
	.emulator-wrapper {
		width: 100vw;
		height: 100vh;
		display: flex;
		flex-direction: column;
		background: #000;
	}
	nav { padding: 10px; background: #222; }
	nav a { color: #fff; text-decoration: none; font-weight: bold; }
	#game-container { flex: 1; width: 100%; }
</style>