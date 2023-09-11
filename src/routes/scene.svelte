<script lang="ts">
	import { onMount } from 'svelte'
	import { tweened } from 'svelte/motion'
	import { quadInOut } from 'svelte/easing'
	import { T } from '@threlte/core'
	import { Align, Grid, OrbitControls } from '@threlte/extras'
	import type { Contributions } from '$lib/types'

	let contributions: Contributions[] = []

	onMount(async () => {
		const response = await fetch('https://gh-contributions-api.vercel.app/mattcroat/2021')
		contributions = await response.json()
		console.log(contributions)
	})

	function getColor(level: number) {
		switch (level) {
			case 0:
				return '#0e0e0e'
			case 1:
				return '#00442a'
			case 2:
				return '#006d35'
			case 3:
				return '#00a648'
			case 4:
				return '#00d35c'
		}
	}

	function normalize(count: number, base = 4, offset = 2) {
		switch (true) {
			case count === 0:
				return base
			case count > 40:
				return count
			default:
				return count * (base * offset)
		}
	}

	const scaleY = tweened(0, { duration: 2000, easing: quadInOut })

	onMount(() => {
		$scaleY = 1
	})
</script>

<Grid infiniteGrid sectionColor="#4a4b4a" sectionSize={20} cellSize={20} fadeDistance={400} />

<T.PerspectiveCamera makeDefault position={[10, 100, 600]} fov={60}>
	<OrbitControls enableDamping />
</T.PerspectiveCamera>

<T.AmbientLight color="#fff" intensity={0.4} />
<T.DirectionalLight position={[0, 200, 200]} intensity={2} color="#fff" />
<T.DirectionalLight position={[0, 200, -200]} intensity={2} color="#fff" />

<Align auto position.y={40}>
	{#each contributions as row, i}
		{#each row as day, j}
			{#if day}
				{@const color = getColor(day.level)}
				{@const y = normalize(day.count)}
				<T.Group position={[0, 0, 12 * i]} scale.y={$scaleY}>
					<T.Mesh position={[12 * j, y / 2, 0]}>
						<T.BoxGeometry args={[10, y, 10]} />
						<T.MeshStandardMaterial {color} />
					</T.Mesh>
				</T.Group>
			{/if}
		{/each}
	{/each}
</Align>
