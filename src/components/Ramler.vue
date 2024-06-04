<script setup lang="ts">
import { TresCanvas } from '@tresjs/core';
import { useLoader, useTexture } from '@tresjs/core'
//@ts-ignore
import { GLTFLoader } from 'three/addons/loaders/GLTFLoader'
import { OrbitControls, useGLTF } from "@tresjs/cientos";


const { scene, nodes, animations, materials } = await useGLTF('ramler_chair_1.glb')

import { TextureLoader } from 'three'

const pbrTexture = await useTexture({
  map: 'Ramler_2.png',
  // displacementMap:
  //   'https://raw.githubusercontent.com/Tresjs/assets/main/textures/black-rock/Rock035_2K_Displacement.jpg',
  // roughnessMap:
  //   'https://raw.githubusercontent.com/Tresjs/assets/main/textures/black-rock/Rock035_2K_Roughness.jpg',
  // normalMap:
  //   'https://raw.githubusercontent.com/Tresjs/assets/main/textures/black-rock/Rock035_2K_NormalGL.jpg',
  // ambientOcclusion:
  //   'https://raw.githubusercontent.com/Tresjs/assets/main/textures/black-rock/Rock035_2K_AmbientOcclusion.jpg',
});

// Apply materials manually to specific objects
scene.children.forEach((child: { name: string; material: any; }) => {
  if (child.name === 'Aluminium_leg_stool') {
    child.material = materials['Aluminium_legs'];
  } 
  
  // else if (child.name === 'ObjectName2') {
  //   child.material = materials['MaterialName2'];
  // }
  // Add more conditions as needed
});

console.log(scene.children)
console.log(materials)
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
        ref="spotlight1Ref"
        :intensity="10"
        :color="0xffffff"
        :angle="Math.PI / 2"
        :position="[-1, 3, 3]"
        :cast-shadow="true"
      />
    <OrbitControls />
       <primitive :object="scene" /> 

       <TresMesh :position="[0, 0.78, -2]">
      <TresPlaneGeometry :args="[6, 3]" />
      <TresMeshStandardMaterial  v-bind="pbrTexture" />
    </TresMesh>
    <!-- <TresMesh :position="[0, -1, -3]">
      <TresPlaneGeometry :args="[10, 2]" />
      <TresMeshStandardMaterial  color="#FFFFFF" />
    </TresMesh> -->

    <TresAmbientLight :intensity="1" />
  </TresCanvas>
</template>