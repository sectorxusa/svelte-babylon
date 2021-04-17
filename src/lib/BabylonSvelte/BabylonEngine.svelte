<script lang="ts">
	import { setContext, onMount, onDestroy } from 'svelte';
	import { writable } from 'svelte/store';

	let canvas: HTMLCanvasElement;

	let engine: BABYLON.Engine;
	const engineStore = writable(engine);
	$: $engineStore = engine; // eslint-disable-line @typescript-eslint/no-unused-vars

	setContext('BabylonEngine', {
		getEngine: () => engineStore
	});

	let BABYLON;

	$: if (BABYLON && canvas && !engine) {
		engine = new BABYLON.Engine(canvas, true, { preserveDrawingBuffer: true, stencil: true });
	}

	onMount(async () => {
		const babylonjs = await import('babylonjs');
		BABYLON = babylonjs.default;
	});

	onDestroy(() => {
		if (engine) engine.stopRenderLoop();
	});
</script>

<canvas bind:this={canvas} class:canvas />

<!-- TODO: Add scene specific slots -->

<slot />

<style>
	.canvas {
		width: 100%;
		height: 100%;
	}
</style>
