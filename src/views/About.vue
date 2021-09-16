<template>
  <div>
    <div class="box"></div>
  </div>
</template>

<script>
import {
  // Color,
  DirectionalLight,
  // DirectionalLightHelper,
  HemisphereLight,
  // HemisphereLightHelper,
  PerspectiveCamera,
  Scene,
  WebGLRenderer,
} from "three";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader";

export default {
  name: "ThreedcarAbout",

  data() {
    return {
      scene: null,
      renderer: null,
      camera: null,
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
      this.scene = new Scene();

      // 创建相机
      const { x, y, z } = this.defaultMap;
      this.camera = new PerspectiveCamera(
        60,
        window.innerWidth / window.innerHeight,
        1,
        1000
      );
      this.camera.position.set(x, y, z);

      // 创建灯光
      let directionalLight = new DirectionalLight(0xffffff, 0.5);
      directionalLight.position.set(-4, 8, 4);
      let hemisphereLight = new HemisphereLight(0xffffff, 0xffffff, 0.4);
      hemisphereLight.position.set(0, 8, 0);
      this.scene.add(directionalLight);
      this.scene.add(hemisphereLight);

      this.renderer = new WebGLRenderer();
      this.renderer.setSize(window.innerWidth, window.innerHeight - 100);

      document.querySelector(".box").appendChild(this.renderer.domElement);
      const gltf = await this.loadFile("/3d/tesla_model_white/scene.gltf");
      this.scene.add(gltf.scene);
    },
    loadFile(url) {
      const loader = new GLTFLoader();
      return new Promise((resolve, reject) => {
        loader.load(
          url,
          (gltf) => {
            resolve(gltf);
          },
          ({ loaded, total }) => {
            console.log((loaded / total) * 100 + "% loaded");
          },
          (err) => {
            reject(err);
          }
        );
      });
    },
  },
};
</script>

<style scoped>
.box {
}
</style>