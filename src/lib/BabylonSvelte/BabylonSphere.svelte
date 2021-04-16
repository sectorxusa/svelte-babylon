<script lang="ts">
	import { getContext, onDestroy } from 'svelte';
	import { Mesh, MeshBuilder, Scene, Vector3 } from 'babylonjs';
	import type { Writable } from 'svelte/store';

	const { getScene } = getContext('BabylonScene');
	const scene: Writable<Scene> = getScene();

	let sphere: Mesh;

	export let name = '';
	export let options = {};
	export let position = new Vector3();
	$: if (sphere) sphere.position = position;

	$: if (!sphere && $scene) {
		sphere = MeshBuilder.CreateSphere(name, options, $scene);
		sphere.position = position;
	}

	onDestroy(() => {
		console.log('onDestroy', sphere);
	});
</script>

<slot />
