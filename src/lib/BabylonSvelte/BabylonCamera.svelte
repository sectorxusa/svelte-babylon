<script lang="ts">
	import { getContext, onDestroy } from 'svelte';
	import { Scene, FreeCamera, Vector3 } from 'babylonjs';
	import type { Writable } from 'svelte/store';

	const { getScene } = getContext('BabylonScene');
	const scene: Writable<Scene> = getScene();

	let camera: FreeCamera;

	export let name = '';
	export let position = new Vector3();
	export let target = new Vector3();

	$: if (!camera && $scene) {
		camera = new FreeCamera(name, position, $scene);

		// This targets the camera to scene origin
		camera.setTarget(target);

		const engine = $scene.getEngine();
		const canvas = engine.getRenderingCanvas();

		// This attaches the camera to the canvas
		camera.attachControl(canvas, true);
	}

	onDestroy(() => {
		console.log('onDestroy', camera);
	});
</script>

<slot />
