<script lang="ts">
	import { onDestroy, setContext } from 'svelte';
	import { writable } from 'svelte/store';
	import { Engine } from 'babylonjs';

	let canvas: HTMLCanvasElement;

	export let engine: Engine;
	let engineStore = writable(engine);
	$: $engineStore = engine;

	setContext('BabylonEngine', {
		getEngine: () => engineStore
	});

	$: if (!engine && canvas) {
		engine = new Engine(canvas, true, { preserveDrawingBuffer: true, stencil: true });
	}

	onDestroy(() => {});
</script>

<canvas bind:this={canvas} class:canvas />

<slot />

<style>
	.canvas {
		width: 100%;
		height: 100%;
	}
</style>
