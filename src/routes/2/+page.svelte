<script lang="ts">
	import SceneInspector from '$lib/components/overlays/SceneInspector.svelte';
	import { onMount } from 'svelte';
	import * as THREE from 'three';
	import { OrbitControls } from 'three/addons/controls/OrbitControls.js';

	let container: HTMLDivElement | undefined = $state();
	let controls: OrbitControls | null = null;

	const scene = new THREE.Scene();

	const cube = new THREE.Mesh(
		new THREE.BoxGeometry(1, 1, 1),
		new THREE.MeshBasicMaterial({ color: 'red' })
	);
	scene.add(cube);
	const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 30);
	camera.position.z = 10;
	scene.add(camera);

	let renderer = new THREE.WebGLRenderer({ antialias: true });
	renderer.setSize(window.innerWidth, window.innerHeight);
	renderer.render(scene, camera);

	function animate() {
		requestAnimationFrame(animate);
		if (!controls) {
			console.log('no controls');
			return;
		}
		controls.update();
		renderer.render(scene, camera);
	}

	onMount(() => {
		container?.appendChild(renderer.domElement);
		controls = new OrbitControls(camera, container);
		controls.enableDamping = true;
		controls.autoRotate = true;
		animate();
	});
</script>

<div class="relative h-screen w-full">
	<div bind:this={container} class="h-screen w-full"></div>
	<SceneInspector {scene} />
</div>
