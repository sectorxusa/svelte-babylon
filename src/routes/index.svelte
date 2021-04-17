<script lang="ts">
	import type { Color3, Vector3 } from 'babylonjs';

	import { onMount } from 'svelte';
	import { tweened } from 'svelte/motion';
	import { bounceOut, quadIn } from 'svelte/easing';

	import {
		BabylonEngine,
		BabylonScene,
		BabylonCamera,
		BabylonHemisphericLight,
		BabylonSphere,
		BabylonGround
	} from '$lib/BabylonSvelte';

	// Sphere falling
	const fall = {
		duration: 1000,
		easing: bounceOut
	};

	// Sphere rising
	const rise = {
		duration: 2000,
		easing: quadIn
	};

	const t = tweened(4);

	// Chaining a Svelte Tweens to loop infinitely (changing the tween options)
	(function loop() {
		t.set(1, fall).then(() => {
			t.set(4, rise).then(loop);
		});
	})();

	let spherePosition: Vector3;
	$: if (spherePosition) spherePosition.y = $t; // Reactively changing the Babylon sphere's y-position with our Svelte Tween

	let sphereColor: Color3;
	$: if (sphereColor) sphereColor = new BABYLON.Color3($t / 4, 0, 1); // Also reactively changing the Babylon sphere's diffuseColor with our Svelte Tween (lazy example)

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
				<BabylonSphere
					bind:position={spherePosition}
					bind:diffuseColor={sphereColor}
					options={{ diameter: 2, segments: 32 }}
				/>
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
