<script lang="ts">
	import { getContext, onDestroy } from 'svelte';
	import type { Writable } from 'svelte/store';

	import * as BABYLON from 'babylonjs';

	const { getScene } = getContext('BabylonScene');
	const scene: Writable<BABYLON.Scene> = getScene();

	let light: BABYLON.HemisphericLight;

	export let name: string;
	$: if (name && light) light.name = name;

	export let direction: BABYLON.Vector3;
	$: if (direction && light) light.direction = direction;

	export let intensity: number;
	$: if (intensity && light) light.intensity = intensity;

	$: if ($scene && !light) {
		if (!name) name = '';
		if (!direction) direction = new BABYLON.Vector3(0, 0, 0);
		if (!intensity) intensity = 1.0;

		light = new BABYLON.HemisphericLight(name, direction, $scene);
	}

	onDestroy(() => {
		if ($scene && light) $scene.removeLight(light);
	});
</script>

<slot />
