<script lang="ts">
	import { getContext, onDestroy, setContext, tick } from 'svelte';
	import { writable } from 'svelte/store';
	import type { Writable } from 'svelte/store';

	import * as BABYLON from 'babylonjs';

	let scene: BABYLON.Scene;
	let sceneStore = writable(scene);
	// eslint-disable-next-line @typescript-eslint/no-unused-vars
	$: $sceneStore = scene;

	setContext('BabylonScene', {
		getScene: () => sceneStore
	});

	const { getEngine } = getContext('BabylonEngine');
	const engine: Writable<BABYLON.Engine> = getEngine();

	function renderFunction() {
		scene.render();
	}

	$: if ($engine && !scene) {
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

	onDestroy(() => {
		if ($engine && scene) {
			$engine.stopRenderLoop(renderFunction);
		}
	});
</script>

<slot />
