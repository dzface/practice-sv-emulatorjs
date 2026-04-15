<script>
	import { page } from '$app/stores';
	import { base } from '$app/paths';

	let gameFile = $derived($page.url.searchParams.get('file'));
	let gameCore = $derived($page.url.searchParams.get('core'));

	$effect(() => {
		if (!gameFile || !gameCore) return;

		// 1. 전역 설정 (중복 선언 방지를 위해 window 객체 속성만 할당)
		window.EJS_player = '#game-container';
		window.EJS_gameUrl = `${base}${gameFile}`;
		window.EJS_core = gameCore;
		window.EJS_pathtodata = `${base}/emulatorjs/data/`;

		// 2. 이미 로더가 로드되어 있다면 에뮬레이터만 다시 시작
		// loader.js가 이미 있으면 삭제 후 다시 붙이거나, 
		// 기존 로더가 만든 이벤트를 활용해야 하지만 가장 확실한 건 컨테이너 비우기입니다.
		const container = document.getElementById('game-container');
		if (container) container.innerHTML = '';

		// 3. 스크립트 중복 로드 방지 및 삽입
		const SCRIPT_ID = 'ejs-loader-script';
		const existingScript = document.getElementById(SCRIPT_ID);
		
		if (existingScript) {
			// 이미 스크립트가 있다면, 페이지 새로고침 없이 게임을 교체하기 위해 
			// EmulatorJS의 특정 함수를 다시 호출해야 할 수도 있지만, 
			// 가장 안전한 방법은 스크립트를 새로 갈아 끼우는 것입니다.
			existingScript.remove();
		}

		const script = document.createElement('script');
		script.id = SCRIPT_ID;
		script.src = `${base}/emulatorjs/data/loader.js?v=${Date.now()}`; // 캐시 방지
		document.head.appendChild(script);

		return () => {
			// 클린업: 페이지 떠날 때 설정 초기화
			delete window.EJS_player;
			delete window.EJS_gameUrl;
		};
	});
</script>

<nav>
	<a href="{base}/">🏠 Home</a>
</nav>

<main>
	<div id="game-container"></div>
</main>

<style>
	main { width: 100vw; height: calc(100vh - 50px); background: black; }
	#game-container { width: 100%; height: 100%; }
</style>