<template>
  <div id="three-render" class="fixed h-screen w-full -z-50"></div>
</template>

<script setup lang="ts">
import * as THREE from 'three';
onMounted(() => {
  class Plane {
    private geometry: THREE.PlaneGeometry | undefined;
    private viewWidth: number;
    private viewHeight: number;
    private material: THREE.ShaderMaterial | any;
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
      this.camera.position.z = 8.0;
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
      this.geometry = new THREE.PlaneGeometry(this.viewWidth, this.viewHeight, 100, 100); // Adjusted segment count
    }

    setMaterial() {
      const colors = [
        new THREE.Color('#000000'),
        new THREE.Color('#cdb553'),
        new THREE.Color('#04b408'),
        new THREE.Color('#3228df'),
        new THREE.Color('#000000')
      ];



      this.material = new THREE.ShaderMaterial({
        uniforms: {
          time: { value: 0.0 },
          amplitude: { value: 0.01 },
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
                // sin(vUv.x * waveFrequency + time) * waveAmplitude;
                float mixFactor = abs(sin(vPosition.y - time * 5.0) * sin(vPosition.x - (time * 2.0)) * cos(vPosition.x - time / 2.0) + sin(vPosition.x - time / 2.0) + sin(vPosition.y + time / 2.0) + cos(vPosition.x + time / 2.0));
                int colorIndex1 = int(mod(mixFactor * 5.0, 5.0));
                int colorIndex2 = (colorIndex1 + 1) % 5;
                float blendFactor = fract(mixFactor * 6.0);

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
      let max ;
      this.material.uniforms.time.value <= 1.0 ? max = true : max = false
      max ? this.material.uniforms.time.value -= 0.005 : this.material.uniforms.time.value += 0.005;

      this.renderer.render(this.scene, this.camera);
    }
  }

  let PlaneApp = new Plane();
  PlaneApp.render()
})
</script>

<style scoped>

</style>
