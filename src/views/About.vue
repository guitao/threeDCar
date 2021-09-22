<template>
  <div>
    <div class="box"></div>
  </div>
</template>

<script>
import {
  DirectionalLight,
  HemisphereLight,
  PerspectiveCamera,
  Scene,
  WebGLRenderer,
} from "three";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";

let scene = null;

export default {
  name: "ThreedcarAbout",
  data() {
    return {
      renderer: null,
      camera: null,
      orbitControls: null,
      defaultMap: {
        x: 510,
        y: 128,
        z: 0,
      },
    };
  },

  mounted() {
    this.init();
  },

  methods: {
    async init() {
      // 创建场景
      this.setScene();
      // 创建相机
      this.setCamera();

      // 控制器
      this.setControl();

      // 创建灯光
      this.setLight();

      const gltf = await this.loadFile("/3d/audi_r8/scene.gltf");
      scene.add(gltf.scene);
      this.loop();
    },

    // 创建场景
    setScene() {
      scene = new Scene();
      this.renderer = new WebGLRenderer();
      this.renderer.setSize(window.innerWidth, window.innerHeight - 100);
      this.renderer.setClearColor("#eee", 1);
      document.querySelector(".box").appendChild(this.renderer.domElement);
    },

    setControl() {
      this.orbitControls = new OrbitControls(
        this.camera,
        this.renderer.domElement
      );
      this.orbitControls.maxPolarAngle = (0.9 * Math.PI) / 2;
      this.orbitControls.enableZoom = true;
      this.orbitControls.autoRotate = true;
    },

    setLight() {
      let directionalLight = new DirectionalLight(0xffffff, 0.5);
      directionalLight.position.set(-4, 8, 4);
      let hemisphereLight = new HemisphereLight(0xffffff, 0xffffff, 0.4);
      hemisphereLight.position.set(0, 8, 0);
      scene.add(directionalLight);
      scene.add(hemisphereLight);
    },

    // 创建相机
    setCamera() {
      const { x, y, z } = this.defaultMap;
      this.camera = new PerspectiveCamera(
        60,
        window.innerWidth / window.innerHeight,
        1,
        1000
      );
      this.camera.position.set(x, y, z);
    },

    loadFile(url) {
      const loader = new GLTFLoader();
      return new Promise((resolve, reject) => {
        loader.load(
          url,
          (gltf) => {
            console.log("===gltf====", gltf);

            resolve(gltf);
          },
          (xhr) => {
            console.log("==xhr===", xhr);
          },
          (err) => {
            reject(err);
          }
        );
      });
    },

    loop() {
      requestAnimationFrame(this.reLoop);
      this.renderer.render(scene, this.camera);
      this.orbitControls.update();
    },
    reLoop() {
      this.loop();
    },
  },
};
</script>

<style scoped>
.box {
}
</style>