<script lang="ts">
	import { getContext, onDestroy, setContext, tick } from 'svelte';
	import { writable } from 'svelte/store';
	import type { Writable } from 'svelte/store';

	import * as BABYLON from 'babylonjs';

	let scene: BABYLON.Scene;
	let sceneStore = writable(scene);
	$: $sceneStore = scene;
	$sceneStore;

	setContext('BabylonScene', {
		getScene: () => sceneStore
	});

	const { getEngine } = getContext('BabylonEngine');
	const engine: Writable<BABYLON.Engine> = getEngine();

	$: if (!scene && $engine) {
		scene = new BABYLON.Scene($engine);

		$engine.runRenderLoop(function () {
			scene.render();
		});

		const window = $engine.getHostWindow();

		window.addEventListener('resize', function () {
			$engine.resize();
		});

		tick().then(function () {
			$engine.resize();
		});
	}

	onDestroy(() => {
		console.log('onDestroy', scene);
	});
</script>

<slot />
