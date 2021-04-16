<script lang="ts">
	import { tweened } from 'svelte/motion';
	import { bounceOut } from 'svelte/easing';

	import * as BABYLON from 'babylonjs';

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

	let spherePosition: BABYLON.Vector3;
	$: if (spherePosition) spherePosition.y = $t;
</script>

<div>
	{@debug BABYLON}
	{#if BABYLON}
		<BabylonEngine>
			<BabylonScene>
				<BabylonCamera />
				<!--
				<BabylonHemisphericLight direction={new BABYLON.Vector3(0, 1, 0)} intensity={0.7} />
				<BabylonSphere options={{ diameter: 2, segments: 32 }} bind:position={spherePosition} />
				<BabylonGround options={{ width: 6, height: 6 }} />
				-->
			</BabylonScene>
		</BabylonEngine>
	{/if}
</div>

<style>
	div {
		position: fixed;
		left: 0;
		right: 0;
		top: 0;
		bottom: 0;
	}
</style>
