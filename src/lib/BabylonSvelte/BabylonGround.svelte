<script lang="ts">
	import { getContext, onMount, onDestroy } from 'svelte';

	const { getScene } = getContext('BabylonScene');
	const scene = getScene();

	let ground: import('babylonjs').Mesh;

	export let name: string = null;
	export let options: Record<string, unknown> = null;

	let BABYLON;

	$: if (BABYLON && $scene && !ground) {
		if (!name) name = '';
		if (!options) options = {};

		ground = BABYLON.MeshBuilder.CreateGround(name, options, $scene);
	}

	onMount(async () => {
		const babylonjs = await import('babylonjs');
		BABYLON = babylonjs.default;
	});

	onDestroy(() => {
		if ($scene && ground) $scene.removeMesh(ground);
	});
</script>

<slot />
