<script lang="ts">
	import { getContext, onDestroy } from 'svelte';
	import type { Writable } from 'svelte/store';

	import * as BABYLON from 'babylonjs';

	const { getScene } = getContext('BabylonScene');
	const scene: Writable<BABYLON.Scene> = getScene();

	let camera: BABYLON.FreeCamera;

	export let name = '';
	$: if (name && camera) camera.name = name;

	export let position: BABYLON.Vector3;
	$: if (position && camera) camera.position = position;

	export let target: BABYLON.Vector3;
	$: if (target && camera) camera.setTarget(target);

	$: if ($scene && !camera) {
		if (!position) position = new BABYLON.Vector3(0, 5, -10);
		if (!target) target = BABYLON.Vector3.Zero();

		camera = new BABYLON.FreeCamera(name, position, $scene);

		const canvas = $scene.getEngine().getRenderingCanvas();
		camera.attachControl(canvas, true);
	}

	onDestroy(() => {
		if ($scene && camera) {
			$scene.removeCamera(camera);
		}
	});
</script>

<slot />
