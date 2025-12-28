<script lang="ts">
	import { onMount } from 'svelte';
	import * as THREE from 'three';

	let container: HTMLDivElement | undefined = $state();

	const scene = new THREE.Scene();

	const cube = new THREE.Mesh(
		new THREE.BoxGeometry(1, 1, 1),
		new THREE.MeshBasicMaterial({ color: 'red' })
	);
	scene.add(cube);

	const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 30);
	camera.position.z = 10;

	let renderer = new THREE.WebGLRenderer({ antialias: true });
	renderer.setSize(window.innerWidth, window.innerHeight);
	renderer.render(scene, camera);

	function animate() {
		requestAnimationFrame(animate);
		cube.rotation.x += 0.01;
		cube.rotation.y += 0.01;
		renderer.render(scene, camera);
	}

	onMount(() => {
		container?.appendChild(renderer.domElement);
		animate();
	});
</script>

<div bind:this={container} class="three-root"></div>

<style>
	.three-root {
		width: 100%;
		height: 100vh;
	}
</style>
