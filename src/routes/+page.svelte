<script lang="ts">
	import { Canvas } from '@threlte/core'
	import Scene from './scene.svelte'
</script>

<div class="scene">
	<Canvas>
		<Scene />
	</Canvas>
</div>

<style>
	.scene {
		position: absolute;
		inset: 0;
		background-color: hsl(200 10% 10%);
	}
</style>
