<script setup>
import * as THREE from 'three'
// 引入轨道控制器
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
import {GUI} from 'three/examples/jsm/libs/lil-gui.module.min.js' 
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
// 修改成线框材质
material.wireframe = true
// 创建网格
const parentCube =new THREE.Mesh(geometry,material)   // 创建父网格
parentCube.position.set(-3,0,0)   
// parentCube.scale.set(2,2,2)
parentCube.rotation.x = Math.PI / 4
// 将网格添加到场景中
const cube = new THREE.Mesh(geometry,material)
parentCube.add(cube)  // 将子网格添加到父网格上
scene.add(parentCube)
cube.position.x = 3    // 子元素的移动相当于是以父元素为基准点位移
// cube.scale.set(2,2,2)   // 基于父元素为基准点缩放
cube.rotation.x = Math.PI / 4   // 基于父元素为基准点旋转

// 设置相机位置
camera.position.z = 5;  // z是正对我们方向，x是横轴(红)，y是垂直(蓝)
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


// 调试开发
let objGui = {
  fullscreen: function() {
    // 全屏
    document.body.requestFullscreen()
  },
}

// 创建GUI
let gui = new GUI()
gui.add(objGui,'fullscreen').name('全屏')
let folder = gui.addFolder('立方体位置')
// step:表示设置的参数为一个单位，不会出现小数
folder.add(cube.position,'x').min(-10).max(10).step(1).name('x轴位置').onChange(e => {
  console.log('change',e);
}).onFinishChange(e => {
  console.log('finashChange',e);
})
// 以父元素为参照物，所以会以父元素的角度方向进行修改
folder.add(cube.position,'y').min(-10).max(10).step(1).name('y轴位置')
folder.add(cube.position,'z').min(-10).max(10).step(1).name('z轴位置')
// 可控制立方体绘制，第三个和第四个参数显示范围
// gui.add(cube.position,'x',-5,5).name('控制立方体位置')
// gui.add(camera.position,'x',-1000,1000).name('相机视角')
gui.add(material,'wireframe').name('修改材质')
let guiColor = {
  color:'#ff0000'
}
gui.addColor(guiColor,'color').name('修改颜色').onChange(e => {
  cube.material.color.set(e)
})


// 监听窗口变化，实现窗口动态变化
window.addEventListener('resize', () => {
  renderer.setSize(window.innerWidth,window.innerHeight)  // 重置渲染器窗口
  camera.aspect = window.innerWidth / window.innerHeight  // 重置相机宽高比
  camera.updateProjectionMatrix ()    // 更新相机矩阵投影
})

</script>

<template>
  <div id="WebGL-output"></div>
</template>
<style >
</style>
