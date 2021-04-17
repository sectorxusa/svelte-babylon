<script lang="ts">
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
	$t = 1;

	let spherePosition: unknown;
	$: if (spherePosition) spherePosition.y = $t;

	let BABYLON: unknown = null;

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
				<BabylonSphere options={{ diameter: 2, segments: 32 }} bind:position={spherePosition} />
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
