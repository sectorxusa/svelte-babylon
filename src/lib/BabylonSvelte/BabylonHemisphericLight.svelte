<script lang="ts">
	import { getContext, onDestroy } from 'svelte';
	import type { Writable } from 'svelte/store';

	import * as BABYLON from 'babylonjs';

	const { getScene } = getContext('BabylonScene');
	const scene: Writable<BABYLON.Scene> = getScene();

	let light: BABYLON.HemisphericLight;

	export let name = '';
	$: if (light) light.name = name;
	export let direction = new BABYLON.Vector3();
	$: if (light) light.direction = direction;
	export let intensity = 1.0;
	$: if (light) light.intensity = intensity;

	$: if (!light && $scene) {
		light = new BABYLON.HemisphericLight(name, direction, $scene);
		light.intensity = intensity;
	}

	onDestroy(() => {
		console.log('onDestroy', light);
	});
</script>

<slot />
