<script lang="ts">
	import { getContext, onDestroy } from 'svelte';
	import type { Writable } from 'svelte/store';

	import * as BABYLON from 'babylonjs';

	const { getScene } = getContext('BabylonScene');
	const scene: Writable<BABYLON.Scene> = getScene();

	let ground: BABYLON.Mesh;

	export let name = '';
	export let options = {};

	$: if ($scene && !ground) {
		ground = BABYLON.MeshBuilder.CreateGround(name, options, $scene);
	}

	onDestroy(() => {
		console.log('onDestroy', ground);
	});
</script>

<slot />
