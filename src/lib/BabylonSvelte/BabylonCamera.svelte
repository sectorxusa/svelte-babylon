<script lang="ts">
	import { getContext, onDestroy } from 'svelte';
	import type { Writable } from 'svelte/store';

	import * as BABYLON from 'babylonjs';

	const { getScene } = getContext('BabylonScene');
	const scene: Writable<BABYLON.Scene> = getScene();

	let camera: BABYLON.FreeCamera;

	export let name = '';
	$: if (name && camera) camera.name = name;

	export let position = new BABYLON.Vector3(0, 0, 0);
	$: if (position && camera) camera.position = position;

	export let target = new BABYLON.Vector3(0, 0, 0);
	$: if (target && camera) camera.setTarget(target);

	$: if ($scene && !camera) {
		camera = new BABYLON.FreeCamera(name, position, $scene);

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
