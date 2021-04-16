<script lang="ts">
	import { getContext, onDestroy } from 'svelte';
	import { Mesh, MeshBuilder, Scene } from 'babylonjs';
	import type { Writable } from 'svelte/store';

	const { getScene } = getContext('BabylonScene');
	const scene: Writable<Scene> = getScene();

	let ground: Mesh;

	export let name = '';
	export let options = {};

	$: if (!ground && $scene) {
		ground = MeshBuilder.CreateGround(name, options, $scene);
	}

	onDestroy(() => {});
</script>

<slot />
