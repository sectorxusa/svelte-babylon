<script lang="ts">
	import { getContext, onDestroy } from 'svelte';
	import type { Writable } from 'svelte/store';

	import * as BABYLON from 'babylonjs';

	const { getScene } = getContext('BabylonScene');
	const scene: Writable<BABYLON.Scene> = getScene();

	let sphere: BABYLON.Mesh;

	export let name: string = null;

	export let options: {} = null;

	export let position: BABYLON.Vector3 = null;
	$: if (sphere) sphere.position = position;

	$: if ($scene && !sphere) {
		if (!name) name = '';
		if (!options) options = {};
		if (!position) position = new BABYLON.Vector3(0, 0, 0);

		sphere = BABYLON.MeshBuilder.CreateSphere(name, options, $scene);
	}

	onDestroy(() => {
		if ($scene && sphere) {
			$scene.removeMesh(sphere);
		}
	});
</script>

<slot />
