<template>
  <div id="three-render" class="fixed h-screen w-full -z-50"></div>
</template>

<script setup lang="ts">
import * as THREE from 'three';
onMounted(() => {
  class WaterFluid {
    private geometry: THREE.PlaneGeometry | undefined;
    private viewWidth: number;
    private viewHeight: number;
    private material: THREE.ShaderMaterial | undefined;
    private mesh: THREE.Mesh | undefined;
    private readonly scene: THREE.Scene;
    private readonly camera: THREE.PerspectiveCamera;
    private renderer: THREE.WebGLRenderer;

    constructor() {
      this.scene = new THREE.Scene();
      this.viewWidth = window.innerWidth;
      this.viewHeight = window.innerHeight;
      this.setGeometry();
      this.setMaterial();
      this.setMesh();
      this.camera = new THREE.PerspectiveCamera(30, this.viewWidth / this.viewHeight, 0.1, 1000);
      this.renderer = new THREE.WebGLRenderer();
      this.renderer.setSize(this.viewWidth, this.viewHeight);
      document.querySelector('#three-render')!.appendChild(this.renderer.domElement); // Tambahkan tanda seru untuk memastikan element ditemukan
      this.camera.position.z = 1.5;
      window.addEventListener('resize', () => this.onResize());
      this.render();
    }

    onResize() {
      this.viewWidth = window.innerWidth;
      this.viewHeight = window.innerHeight;
      this.camera.aspect = this.viewWidth / this.viewHeight;
      this.camera.updateProjectionMatrix();
      this.renderer.setSize(this.viewWidth, this.viewHeight);
    }

    setGeometry() {
      this.geometry = new THREE.PlaneGeometry(this.viewWidth, this.viewHeight, 500, 500); // Adjusted segment count
    }

    setMaterial() {
      const colors = [
        new THREE.Color('#141414'),
        new THREE.Color('#47a4a2'),
        new THREE.Color('#8b3991'),
        new THREE.Color('#000000'),
        new THREE.Color('#604995')
      ];



      this.material = new THREE.ShaderMaterial({
        uniforms: {
          time: { value: 20.0 },
          amplitude: { value: 0.05 },
          frequency: { value: 10.0 },
          colors: { value: colors }
        },
        vertexShader: `
            uniform float time;
            uniform float amplitude;
            uniform float frequency;
            varying vec3 vPosition;

            void main() {
                vec3 pos = position;
                pos.z = (sin(pos.x * pos.y * frequency + time) * amplitude / 2.0);
                gl_Position = projectionMatrix * modelViewMatrix * vec4(pos, 1.0);
                vPosition = position; // Menyimpan posisi untuk digunakan dalam fragment shader
            }
        `,
        fragmentShader: `
            uniform float time;
            uniform vec3 colors[5];
            varying vec3 vPosition;

            void main() {
                // Menghitung indeks warna berdasarkan posisi piksel
                float mixFactor = abs(cos(vPosition.y * vPosition.x * time) * sin(vPosition.x - time) * cos(vPosition.y - time));
                int colorIndex1 = int(mod(mixFactor * 5.0, 5.0)); // 10 adalah jumlah warna
                int colorIndex2 = (colorIndex1 + 1) % 5; // Indeks warna berikutnya
                float blendFactor = fract(mixFactor * 5.0); // Faktor pencampuran

                // Menggunakan fungsi mix untuk mencampurkan warna secara lembut
                vec3 finalColor = mix(colors[colorIndex1], colors[colorIndex2], blendFactor);

                gl_FragColor = vec4(finalColor, 1.0);
            }
        `,
        blending: THREE.AdditiveBlending,
        transparent: true,
        depthWrite: false,
        depthTest: false
      });
    }

    setMesh() {
      this.mesh = new THREE.Mesh(this.geometry, this.material);
      this.scene.add(this.mesh);
    }

    render() {
      requestAnimationFrame(() => this.render());
      let max = false;
      this.material.uniforms.time.value <= 50.0 ? max = true : max = false
      max ? this.material.uniforms.time.value -= 0.005 : this.material.uniforms.time.value += 0.005;

      this.renderer.render(this.scene, this.camera);
    }
  }

  const WaterFluidApp = new WaterFluid();
})
</script>

<style scoped>

</style>
