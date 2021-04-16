<script lang="ts">
	import { getContext, onDestroy, setContext, tick } from 'svelte';
	import { writable } from 'svelte/store';
	import type { Writable } from 'svelte/store';
	import { Engine, Scene } from 'babylonjs';

	let scene: Scene;
	let sceneStore = writable(scene);
	$: $sceneStore = scene;

	setContext('BabylonScene', {
		getScene: () => sceneStore
	});

	const { getEngine } = getContext('BabylonEngine');
	const engine: Writable<Engine> = getEngine();

	$: if (!scene && $engine) {
		scene = new Scene($engine);

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

	onDestroy(() => {});
</script>

<slot />
