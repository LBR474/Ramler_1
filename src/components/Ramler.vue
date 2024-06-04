<script setup lang="ts">
import { ref, onMounted, onUnmounted } from "vue";
import { TresCanvas, useRenderLoop } from "@tresjs/core";
import { useTexture } from "@tresjs/core";
//@ts-ignore
import { GLTFLoader } from "three/addons/loaders/GLTFLoader";
//@ts-ignore
import { OrbitControls, useGLTF } from "@tresjs/cientos";
import { gsap } from "gsap";

const { scene, materials } = await useGLTF("ramler_chair_1.glb");

const Ramler_1_Texture = await useTexture({
  map: "Ramler_1.png",
});

const Ramler_3_Texture = await useTexture({
  map: "Ramler_3.png",
});

const planeRef = ref();

const planeRef_2 = ref();

const { onLoop } = useRenderLoop();

console.log(scene.children);
console.log(materials);

let animationActive = true;
let userClicked = false;

const handlePointerDown = () => {
  if (planeRef_2.value) {
    planeRef_2.value.visible = false;
    userClicked = true;
  }
};

onMounted(() => {
  window.addEventListener("pointerdown", handlePointerDown);
});

onUnmounted(() => {
  window.removeEventListener("pointerdown", handlePointerDown);
});

onLoop(() => {
  if (!animationActive) return;

  if (planeRef.value && planeRef_2.value) {
    planeRef_2.value.visible = false;

    // Animate the plane along the z-axis using GSAP
    gsap.to(planeRef.value.position, {
      z: -6,
      duration: 1,
      ease: "power1.inOut",
      onComplete: () => {
        setTimeout(() => {
          planeRef.value.visible = false; // Make the plane invisible
          setTimeout(() => {
            if (!userClicked) {
              planeRef_2.value.visible = true; // Make the plane visible only if not clicked
            }
          }, 2000);

          animationActive = false; // Stop the animation loop
        }, 7000);
      },
    });
  }
});
</script>

<template>
  <TresCanvas clear-color="#FFFFFF" window-size>
    <TresPerspectiveCamera
      :position="[0, 0, 10]"
      :fov="45"
      :look-at="[0, 0, 0]"
    />
    <TresSpotLight
      ref="spotlight1Ref"
      :intensity="10"
      :color="0xffffff"
      :angle="Math.PI / 2"
      :position="[1, -1, 2]"
      :cast-shadow="true"
    />

    <TresSpotLight
      ref="spotlight2Ref"
      :intensity="10"
      :color="0xffffff"
      :angle="Math.PI / 2"
      :position="[-1, 3, 3]"
      :cast-shadow="true"
    />
    <OrbitControls />
    <primitive :object="scene" :position="[0, 0, 0]" />

    <TresMesh :position="[0, 0.78, 10]" ref="planeRef">
      <TresPlaneGeometry :args="[6, 4]" />
      <TresMeshStandardMaterial v-bind="Ramler_1_Texture" />
    </TresMesh>
    <TresMesh :position="[-4, 0.78, 2]" ref="planeRef_2">
      <TresPlaneGeometry :args="[3, 1]" />
      <TresMeshStandardMaterial v-bind="Ramler_3_Texture" />
    </TresMesh>

    <TresAmbientLight :intensity="1" />
  </TresCanvas>
</template>
