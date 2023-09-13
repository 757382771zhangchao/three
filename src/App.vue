<script setup>
import * as THREE from 'three'

console.log(THREE);
// 创建场景
const scene = new THREE.Scene()

// 创建相机
const camera = new THREE.PerspectiveCamera(
  45,  // 视角，越大看到的东西越多，视野越广
  window.innerWidth / window.innerHeight, // 宽高比
  0.1, // 近平面，相机最近能看到的物体
  1000 // 远
)

// 创建渲染器
const renderer = new THREE.WebGL1Renderer()
renderer.setSize(window.innerWidth,window.innerHeight)  // 渲染屏幕的大小，整个屏幕的宽高
document.body.appendChild(renderer.domElement)

// 创建几何体
const geometry = new THREE.BoxGeometry(1,1,1);
// 创建材质
const material = new THREE.MeshBasicMaterial({color: 0x00ff00})
// 创建网格
const cube = new THREE.Mesh(geometry,material)

// 将网格添加到场景中
scene.add(cube)

// 设置相机位置
camera.position.z = 5;  // z是正对我们方向，x是横轴，y是垂直
camera.lookAt(0,0,0)  // 相机默认看向哪里。默认是原点

// 渲染函数
function animate() {
  // 请求动画帧，会一帧一帧的调用动画函数
  requestAnimationFrame(animate)

  // 不停旋转
  cube.rotation.x += 0.01
  cube.rotation.y += 0.01
  // 不停的渲染
  renderer.render(scene,camera)
}
animate()


</script>

<template>
  <div id="WebGL-output"></div>
</template>
<style >
</style>
