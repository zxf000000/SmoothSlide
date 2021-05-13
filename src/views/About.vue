<template>
  <div ref="container" class="about">
<!--    <h1>This is an about page</h1>-->
<!--    <router-link to="/about/next">toNext</router-link> <br>-->
<!--    <router-link to="/about/next/next1">to next next</router-link>-->
<!--    <router-view></router-view>-->
  </div>
</template>

<script>
import * as THREE from "three";

export default {
  name: "About",
  data() {
    return {
      scene: null,
      camera: null,
      cube: null,
      renderer: null,
    };
  },
  mounted() {
    this.scene =  new THREE.Scene();
    this.camera = new THREE.PerspectiveCamera( 75, 2, 0.1, 1000 );
    this.renderer = new THREE.WebGLRenderer();
    this.renderer.setSize( 1000, 500 );
    this.$refs.container.appendChild( this.renderer.domElement );

    const geometry = new THREE.BoxGeometry();
    const material = new THREE.MeshPhysicalMaterial({color: 0xff0000});
    this.cube = new THREE.Mesh(geometry, material);
    this.scene.add(this.cube);
    this.camera.position.z = 5;
    this.animate();
  },
  methods: {
    animate() {
      requestAnimationFrame(this.animate);
      this.cube.rotation.x += 0.01;
      this.cube.rotation.y += 0.01;
      this.renderer.render(this.scene, this.camera);
    }
  }
}

</script>

<style scoped lang="scss">
.about {
  width: 1000px;
  height: 500px;
  background: #dddddd;
  margin-left: 100px;
}

</style>
