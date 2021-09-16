<template>
  <div class="boxs">
    <!-- <div class="maskLoading" v-if="false">
      <div class="loading">
        <div :style="{ width: loadingWidth + '%' }"></div>
      </div>
      <div style="padding-left: 10px">{{ parseInt(loadingWidth) }}%</div>
    </div> -->
    <div class="mask">
      <p v-if="false">x : {{ x }} y:{{ y }} z :{{ z }}</p>
      <button @click="isAutoFun">转动车</button>
      <button @click="stop">停止</button>
      <div class="flex" v-if="false">
        <div
          @click="setCarColor(index)"
          v-for="(item, index) in colorAry"
          :key="index"
          :style="{ backgroundColor: item }"
        ></div>
      </div>
    </div>
  </div>
</template>

<script>
import {
  DirectionalLight,
  DirectionalLightHelper,
  HemisphereLight,
  HemisphereLightHelper,
  Scene,
  PerspectiveCamera,
  WebGLRenderer,
} from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader";
export default {
  name: "ThreedcarAbout",
  data() {
    return {
      colorAry: [
        "rgb(216, 27, 67)",
        "rgb(142, 36, 170)",
        "rgb(81, 45, 168)",
        "rgb(48, 63, 159)",
        "rgb(30, 136, 229)",
        "rgb(0, 137, 123)",
        "rgb(67, 160, 71)",
        "rgb(251, 192, 45)",
        "rgb(245, 124, 0)",
        "rgb(230, 74, 25)",
        "rgb(233, 30, 78)",
        "rgb(156, 39, 176)",
        "rgb(0, 0, 0)",
      ],
      defaultMap: {
        x: 510,
        y: 128,
        z: 0,
      },

      map: null,
      scene: null,
      camera: null,
      renderer: null,
      controls: null,
      floor: null,
      dhelper: null,
      hHelper: null,
      directionalLight: null,
      hemisphereLight: null,

      // --------
      isLoading: true,
    };
  },

  mounted() {
    this.init();
  },

  methods: {
    setLight() {
      this.directionalLight = new DirectionalLight(0xffffff, 0.5);
      this.directionalLight.position.set(-4, 8, 4);
      this.dhelper = new DirectionalLightHelper(
        this.directionalLight,
        5,
        0xff0000
      );
      this.hemisphereLight = new HemisphereLight(0xffffff, 0xffffff, 0.4);
      this.hemisphereLight.position.set(0, 8, 0);
      this.hHelper = new HemisphereLightHelper(this.hemisphereLight, 5);
      this.scene.add(this.directionalLight);
      this.scene.add(this.hemisphereLight);
    },

    setScene() {
      this.scene = new Scene();
      this.renderer = new WebGLRenderer();
      this.renderer.setSize(window.innerWidth, window.innerHeight);
      document.querySelector(".boxs").appendChild(this.renderer.domElement);
    },

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

    setControls() {
      this.controls = new OrbitControls(this.camera, this.renderer.domElement);
      this.controls.maxPolarAngle = (0.9 * Math.PI) / 2;
      this.controls.enableZoom = true;
      this.controls.addEventListener("change", this.render);
    },

    render() {
      this.map.x = Number.parseInt(this.camera.position.x);
      this.map.y = Number.parseInt(this.camera.position.y);
      this.map.z = Number.parseInt(this.camera.position.z);
    },

    loop() {
      requestAnimationFrame(this.reLoop);
      this.renderer.render(this.scene, this.camera);
      this.controls.update();
    },

    reLoop() {
      this.loop();
    },

    loadFile: (url) => {
      const loader = new GLTFLoader();
      return new Promise((resolve, reject) => {
        loader.load(
          url,
          (gltf) => {
            resolve(gltf);
          },
          ({ loaded, total }) => {
            let load = Math.abs((loaded / total) * 100);
            // loadingWidth.value = load;
            if (load >= 100) {
              // setTimeout(() => {
              //   isLoading.value = false;
              // }, 1000);
            }
            console.log((loaded / total) * 100 + "% loaded");
          },
          (err) => {
            reject(err);
          }
        );
      });
    },

    async init() {
      this.setScene();
      this.setCamera();
      this.setLight();
      // this.setControls();

      const gltf = await this.loadFile("/3d/audi_r8/scene.gltf");
      this.scene.add(gltf.scene);
      // this.loop();

      // this.loadFile("/3d/audi_r8/this.scene.gltf").then((res) => {
      //   console.log(res)
      //   this.scene.add(res.scene);
      //   this.loop();
      // });
    },
  },
};
</script>

<style lang="scss" scoped>
</style>