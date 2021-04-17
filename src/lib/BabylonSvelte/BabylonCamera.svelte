<script lang="ts">
	import { getContext, onMount, onDestroy } from 'svelte';

	const { getScene } = getContext('BabylonScene');
	const scene = getScene();

	let camera: BABYLON.FreeCamera;

	export let name: string = null;
	$: if (name && camera) camera.name = name;

	export let position: BABYLON.Vector3 = null;
	$: if (position && camera) camera.position = position;

	export let target: BABYLON.Vector3 = null;
	$: if (target && camera) camera.setTarget(target);

	let BABYLON;

	$: if (BABYLON && $scene && !camera) {
		if (!name) name = '';
		if (!position) position = new BABYLON.Vector3(0, 0, 0);
		if (!target) target = new BABYLON.Vector3(0, 0, 0);

		camera = new BABYLON.FreeCamera(name, position, $scene);

		const canvas = $scene.getEngine().getRenderingCanvas();
		camera.attachControl(canvas, true);
	}

	onMount(async () => {
		const babylonjs = await import('babylonjs');
		BABYLON = babylonjs.default;
	});

	onDestroy(() => {
		if ($scene && camera) $scene.removeCamera(camera);
	});
</script>

<slot />
