<script lang="ts">
	import { getContext, onDestroy } from 'svelte';
	import type { Writable } from 'svelte/store';

	import * as BABYLON from 'babylonjs';

	const { getScene } = getContext('BabylonScene');
	const scene: Writable<BABYLON.Scene> = getScene();

	let sphere: BABYLON.Mesh;

	export let name = '';
	export let options = {};
	export let position = new BABYLON.Vector3();
	$: if (sphere) sphere.position = position;

	$: if ($scene && !sphere) {
		sphere = BABYLON.MeshBuilder.CreateSphere(name, options, $scene);
		sphere.position = position;
	}

	onDestroy(() => {
		console.log('onDestroy', sphere);
	});
</script>

<slot />
