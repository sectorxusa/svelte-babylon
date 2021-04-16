<script lang="ts">
	import { onDestroy, setContext } from 'svelte';
	import { writable } from 'svelte/store';

	import * as BABYLON from 'babylonjs';

	let canvas: HTMLCanvasElement;

	let engine: BABYLON.Engine;
	const engineStore = writable(engine);
	$: $engineStore = engine; // eslint-disable-line @typescript-eslint/no-unused-vars

	setContext('BabylonEngine', {
		getEngine: () => engineStore
	});

	$: if (!engine && canvas) {
		engine = new BABYLON.Engine(canvas, true, { preserveDrawingBuffer: true, stencil: true });
	}

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
