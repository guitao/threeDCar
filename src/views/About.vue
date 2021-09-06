<template>
  <div class="demoWrap">
    <div id="container"></div>
  </div>
</template>

<script>
import * as THREE from "three";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader";
// import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";
export default {
  name: "ThreePage_demo1",
  data() {
    return {
      camera: null,
      scene: null,
      renderer: null,
      mesh: null,
      controls: null,
    };
  },
  mounted() {
    this.init();
  },
  methods: {
    // 初始化
    init: function () {
      //创建场景
      this.scene = new THREE.Scene();
      this.scene.background = new THREE.Color(0xcfcfcf);
      //创建相机，设置相机的位置
      this.camera = new THREE.PerspectiveCamera(
        75,
        window.innerWidth / window.innerHeight,
        0.1,
        1000
      );
      this.camera.position.set(0, 100, 150);
      //灯光效果
      var ambient = new THREE.AmbientLight(0xffffff);
      this.scene.add(ambient);
      //创建场景渲染
      this.renderer = new THREE.WebGLRenderer({ antialias: true });
      this.renderer.setPixelRatio(window.devicePixelRatio);
      this.renderer.setSize(window.innerWidth, window.innerHeight);
      document
        .getElementById("container")
        .appendChild(this.renderer.domElement);

      // var loader = new THREE.GLTFLoader();
      var loader = new GLTFLoader();
      loader.load("../assets/3d/busterDrone.gltf", function (gltf) {
        let model = gltf.scene;
        //场景中添加模型文件
        this.scene.add(model);
        model.traverse(function (gltf) {
          if (gltf.isMesh) {
            //设置mesh的一些属性
          }
        });
        //设置整体场景的比例
        model.scale.set(10, 10, 10);
      });
    },
  },
};
</script>

<style>
#container {
  position: absolute;
  width: 98%;
  height: 98%;
}
</style>
