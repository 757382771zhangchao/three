<script setup>
import * as THREE from 'three'

// 引入轨道控制器
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
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

// 添加世界坐标辅助器
const axesHelper = new THREE.AxesHelper(5) // 线段长度5
scene.add(axesHelper)

// 创建轨道控制器，可以控制相机，可以通过监听不同的dom的轨道控制器，进行各种操作
const controls = new OrbitControls(camera, renderer.domElement);
// 设置阻尼惯性
controls.enableDamping = true
// 设置阻尼系数,默认0.05调小相当于没有阻碍，加大滑动
controls.dampingFactor = 0.01

// 渲染函数
function animate() {
  // 更新控件，这只阻尼惯性，需要执行更新函数
  controls.update()
  // 请求动画帧，会一帧一帧的调用动画函数
  requestAnimationFrame(animate)

  // 不停旋转
  // cube.rotation.x += 0.01
  // cube.rotation.y += 0.01
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
