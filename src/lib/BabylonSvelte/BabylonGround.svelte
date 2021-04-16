<script lang="ts">
	import { getContext, onDestroy } from 'svelte';
	import type { Writable } from 'svelte/store';

	import * as BABYLON from 'babylonjs';

	const { getScene } = getContext('BabylonScene');
	const scene: Writable<BABYLON.Scene> = getScene();

	let ground: BABYLON.Mesh;

	export let name: string;
	export let options: {};

	$: if ($scene && !ground) {
		if (!name) name = '';
		if (!options) options = {};

		ground = BABYLON.MeshBuilder.CreateGround(name, options, $scene);
	}

	onDestroy(() => {
		if ($scene && ground) {
			$scene.removeMesh(ground);
		}
	});
</script>

<slot />
