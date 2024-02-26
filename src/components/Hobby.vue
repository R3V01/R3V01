<template>
    <div id="hobby">

    </div>
</template>

<script>
import * as THREE from 'three';
import { onMounted } from 'vue';
import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js';
import { OrbitControls } from "three/addons/controls/OrbitControls.js";
import { GUI } from 'three/addons/libs/lil-gui.module.min.js';
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';
export default {
    setup(){
        onMounted(()=>{
            hobbyinit();
        })

        function hobbyinit(){

            const gui = new GUI();
            const obj = {
                plcolor:0xffffff,
                plint: 1,
                alcolor: 0xffffff,
                alint: 1,
            }

            gui.addColor(obj,'plcolor').name('V2pointlight').onChange(function(value){
                pointLight.color.set(value);
            })
            gui.add(obj,'plint',0,1).name('V2pointlint').onChange(function(value){
                pointLight.intensity = value;
            })
            gui.addColor(obj,'alcolor').name('V2amlight').onChange(function(value){
                ambientLight.color.set(value);
            })
            gui.add(obj,'alint',0,1).name('V2amlint').onChange(function(value){
                ambientLight.intensity = value;
            })

            const Loader = new GLTFLoader();
            const AllM = new THREE.Group();
            Loader.load("../../public/Guitar.glb", function(guitar){
                
                AllM.add(guitar.scene);
            })

            /*Loader.load("../../public/InteriorTest.glb", function(room){

                room.scene.scale.set(30,30,30);
                AllM.add(room.scene);
            })*/

            const scene = new THREE.Scene();
            scene.add(AllM);

            const pointLight = new THREE.PointLight(0xffffff,3);
            pointLight.decay = 0.0;
            pointLight.position.set(100,100,100);

            scene.add(pointLight);


            const ambientLight = new THREE.AmbientLight(0xffffff,2);
            scene.add(ambientLight);

            const camera = new THREE.PerspectiveCamera(75,window.innerWidth / window.innerHeight);

            camera.position.set(10,10,10)
            const axesHelper = new THREE.AxesHelper(150,150,150);
            scene.add(axesHelper);

            const renderer = new THREE.WebGLRenderer();

            renderer.setSize(window.innerWidth, window.innerHeight);
            renderer.render(scene,camera);
            document.getElementById('hobby').appendChild(renderer.domElement);
            renderer.setPixelRatio(window.devicePixelRatio);


            const controls = new OrbitControls (camera,renderer.domElement);

            controls.addEventListener('change',function(){
                renderer.render(scene,camera);
            })


            function animate2(){
                requestAnimationFrame(animate2);
                console.log(camera.position)
                controls.update();
                renderer.render(scene,camera)
            }
            animate2();
        }
        
    }
}
</script>