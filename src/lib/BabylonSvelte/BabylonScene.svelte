<script lang="ts">
	import type { Scene } from 'babylonjs';

	import { setContext, getContext, onMount, onDestroy, tick } from 'svelte';
	import { writable } from 'svelte/store';

	let scene: Scene;
	let sceneStore = writable(scene);
	$: $sceneStore = scene; // eslint-disable-line @typescript-eslint/no-unused-vars

	export const elapsedTime = writable(0);

	setContext('BabylonScene', {
		getScene: () => sceneStore
	});

	const { getEngine } = getContext('BabylonEngine');
	const engine = getEngine();

	function renderFunction() {
		scene.render();
		$elapsedTime += scene.deltaTime;
	}

	let BABYLON: typeof import('babylonjs');

	$: if (BABYLON && $engine && !scene) {
		scene = new BABYLON.Scene($engine);

		$engine.runRenderLoop(renderFunction);

		const window = $engine.getHostWindow();

		window.addEventListener('resize', () => {
			$engine.resize();
		});

		tick().then(() => {
			$engine.resize();
		});
	}

	onMount(async () => {
		const babylonjs = await import('babylonjs');
		BABYLON = babylonjs.default;
	});

	onDestroy(() => {
		if ($engine && scene) $engine.stopRenderLoop(renderFunction);
	});
</script>

<slot />
