<script lang="ts">
	import type { Vector3 } from 'babylonjs';

	import { onMount } from 'svelte';
	import { tweened } from 'svelte/motion';
	import { bounceOut } from 'svelte/easing';

	import {
		BabylonEngine,
		BabylonScene,
		BabylonCamera,
		BabylonHemisphericLight,
		BabylonSphere,
		BabylonGround
	} from '$lib/BabylonSvelte';

	const t = tweened(4, {
		duration: 2500,
		easing: bounceOut
	});

	// Chaining a tween to loop infinitely between two values
	(function loop() {
		t.set(1).then(() => {
			t.set(4).then(loop);
		});
	})();

	let spherePosition: Vector3;
	$: if (spherePosition) spherePosition.y = $t; // Reactively changing the Babylon sphere's y-position with our Svelte Tween

	let BABYLON: typeof import('babylonjs');

	onMount(async () => {
		const babylonjs = await import('babylonjs');
		BABYLON = babylonjs.default;
	});
</script>

<div class:BABYLON>
	{#if BABYLON}
		<BabylonEngine>
			<BabylonScene>
				<BabylonCamera position={new BABYLON.Vector3(0, 5, -10)} target={BABYLON.Vector3.Zero()} />
				<BabylonHemisphericLight direction={new BABYLON.Vector3(0, 1, 0)} intensity={0.7} />
				<!-- Below is two-way binding between spherePosition and position -->
				<BabylonSphere bind:position={spherePosition} options={{ diameter: 2, segments: 32 }} />
				<BabylonGround options={{ width: 6, height: 6 }} />
			</BabylonScene>
		</BabylonEngine>
	{/if}
</div>

<style>
	.BABYLON {
		position: fixed;
		left: 0;
		right: 0;
		top: 0;
		bottom: 0;
	}
</style>
