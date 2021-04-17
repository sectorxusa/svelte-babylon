<script lang="ts">
	import { getContext, onMount, onDestroy } from 'svelte';

	const { getScene } = getContext('BabylonScene');
	const scene = getScene();

	let sphere: import('babylonjs').Mesh;

	export let name: string = null;

	export let options: Record<string, unknown> = null;

	export let position: import('babylonjs').Vector3 = null;
	$: if (sphere) sphere.position = position;

	let BABYLON: Record<string, unknown>;

	$: if (BABYLON && $scene && !sphere) {
		if (!name) name = '';
		if (!options) options = {};
		if (!position) position = new BABYLON.Vector3(0, 0, 0);

		sphere = BABYLON.MeshBuilder.CreateSphere(name, options, $scene);
	}

	onMount(async () => {
		const babylonjs = await import('babylonjs');
		BABYLON = babylonjs.default;
	});

	onDestroy(() => {
		if ($scene && sphere) $scene.removeMesh(sphere);
	});
</script>

<slot />
