<script lang="ts">
	import { T, useTask } from '@threlte/core';
	import { interactivity, OrbitControls } from '@threlte/extras';
	import { Spring } from 'svelte/motion';

	interactivity();
	const scale = new Spring(1);

	let rotation = $state(0);
	useTask((delta) => {
		rotation += delta;
	});
</script>

<T.PerspectiveCamera
	makeDefault
	position={[10, 10, 10]}
	oncreate={(ref) => {
		ref.lookAt(0, 1, 0);
	}}
>
	<OrbitControls autoRotate />
</T.PerspectiveCamera>

<T.DirectionalLight position={[0, 10, 10]} castShadow />

<T.Mesh
	position={[0, 1, 0]}
	rotation.y={rotation}
	scale={scale.current}
	onpointerenter={() => (scale.target = 1.5)}
	onpointerleave={() => (scale.target = 1)}
	castShadow
>
	<T.MeshStandardMaterial color="blue" />
	<T.BoxGeometry args={[2, 2, 2]} />
</T.Mesh>

<T.Mesh receiveShadow rotation.x={-Math.PI / 2}>
	<T.MeshStandardMaterial color="white" />
	<T.CircleGeometry args={[10, 1000]} />
</T.Mesh>
