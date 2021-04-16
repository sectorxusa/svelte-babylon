<script lang="ts">
	import { getContext, onDestroy } from 'svelte';
	import { HemisphericLight, Scene, Vector3 } from 'babylonjs';
	import type { Writable } from 'svelte/store';

	const { getScene } = getContext('BabylonScene');
	const scene: Writable<Scene> = getScene();

	let light: HemisphericLight;

	export let name = '';
	$: if (light) light.name = name;
	export let direction = new Vector3();
	$: if (light) light.direction = direction;
	export let intensity = 1.0;
	$: if (light) light.intensity = intensity;

	$: if (!light && $scene) {
		light = new HemisphericLight(name, direction, $scene);
		light.intensity = intensity;
	}

	onDestroy(() => {});
</script>

<slot />
