<template>
  <div id="parent-canvas">
    <canvas id="canvas"></canvas>
  </div>
</template>

<script setup lang="ts">
import * as THREE from "three";
import { onMounted, ref } from "vue";
import * as f from "./functions";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";

const rendererRef = ref();
const controlsRef = ref();

onMounted(() => {
  const canvas = document.getElementById("canvas")!;

  const renderer = new THREE.WebGLRenderer({
    canvas: canvas,
  });
  renderer.setSize(window.innerWidth, window.innerHeight);
  rendererRef.value = renderer;

  const geometry = new THREE.BoxGeometry(1, 1, 1);
  const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
  const cube = new THREE.Mesh(geometry, material);
  f.scene.add(cube);

  f.camera.position.z = 5;

  const parentElement = document.getElementById("parent-canvas")!; // Replace with the actual ID or reference to the parent element

  f.resize(renderer, f.camera, f.sizes, parentElement);
  f.cursor(f.sizes, f.camera);

  // // Controls
  const controls = new OrbitControls(f.camera, canvas);
  controls.enableDamping = true;

  controlsRef.value = controls;

  animate();
});

function animate() {
  requestAnimationFrame(animate);

  controlsRef.value.update();

  rendererRef.value.render(f.scene, f.camera);
}
</script>

<style>
#canvas,
#parent-canvas {
  height: 100vh !important;
}

body {
  margin: 0;
}
</style>
