<script lang="ts">
	import { getContext, onDestroy } from 'svelte';
	import type { Writable } from 'svelte/store';

	import * as BABYLON from 'babylonjs';

	const { getScene } = getContext('BabylonScene');
	const scene: Writable<BABYLON.Scene> = getScene();

	let light: BABYLON.HemisphericLight;

	export let name = '';
	$: if (name && light) light.name = name;

	export let direction = new BABYLON.Vector3();
	$: if (direction && light) light.direction = direction;

	export let intensity = 1.0;
	$: if (intensity && light) light.intensity = intensity;

	$: if ($scene && !light) {
		light = new BABYLON.HemisphericLight(name, direction, $scene);
	}

	onDestroy(() => {
		console.log('onDestroy', light);
	});
</script>

<slot />
