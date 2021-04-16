<script lang="ts">
	import { getContext, onDestroy } from 'svelte';
	import type { Writable } from 'svelte/store';

	import * as BABYLON from 'babylonjs';

	const { getScene } = getContext('BabylonScene');
	const scene: Writable<BABYLON.Scene> = getScene();

	let camera: BABYLON.FreeCamera;

	export let name = '';
	export let position = new BABYLON.Vector3();
	export let target = new BABYLON.Vector3();

	$: if (!camera && $scene) {
		camera = new BABYLON.FreeCamera(name, position, $scene);

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
