<template>
    <div id="mountain"></div>
</template>

<script>
import * as THREE from 'three';
import { onMounted } from 'vue';
import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js';
import { OrbitControls } from "three/addons/controls/OrbitControls.js";
import { GUI } from 'three/addons/libs/lil-gui.module.min.js';
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';
import { DRACOLoader } from 'three/addons/loaders/DRACOLoader.js';

export default {
    setup() {
    let scene, camera, renderer, cube;

    onMounted(() => {
      init();
      animate();
    });

    let controls

    function init() {

     //const gui = new GUI();
      const obj = {
        plcolor: 0xece0b6,
        plint: 2.7,
        alint: 1.6,
        alcolor: 0xb5e0e8,
        bgcolor: 0x86bad0,
        spcolor:0xffffff,
        spint:10,
        spangle:0.65,
        fogfar:8
      }
      /*gui.addColor(obj, 'plcolor').name('点光色').onChange(function(value){
        pointLight.color.set(value);
      })
      gui.add(obj, 'plint',0,3).name('点光强').onChange(function(value){
        pointLight.intensity = value;
      })
      gui.addColor(obj, 'alcolor').name('环光色').onChange(function(value){
        ambientLight.color.set(value);
      })
      gui.add(obj, 'alint',0,2).name('环光强').onChange(function(value){
        ambientLight.intensity = value;
      })
      gui.addColor(obj, 'bgcolor').name('bg').onChange(function(value){
        renderer.setClearColor(value);
      })
      gui.addColor(obj, 'spcolor').name('ju光色').onChange(function(value){
        spotLight.color.set(value);
      })
      gui.add(obj, 'spint',10,20).name('ju光强').onChange(function(value){
        spotLight.intensity = value;
      })
      gui.add(obj, 'spangle',0,Math.PI/2).name('jujiaodu').onChange(function(value){
        spotLight.angle = value;
      })
      gui.add(obj, 'fogfar',6,1000).name('foggy').onChange(function(value){
        scene.fog.far = value;
      })*/


      const dracoLoader = new DRACOLoader();
      dracoLoader.setDecoderPath('../../public/draco/');

      const loader = new GLTFLoader();
      const model = new THREE.Group();
      
      loader.setDRACOLoader(dracoLoader);
      loader.load('../../public/Mountain7.glb', function(glb){
        model.add(glb.scene);
        //glb.scene.children[0].material.map = mounMap
        
        //console.log(glb.scene.children[0].material)
      })
      console.log(loader)

      const model2 = new THREE.Group();
      /*loader.load('../../public/Mercury 1K.glb', function(glb){
        glb.scene.position.set(0,50,-100);
        glb.scene.scale.set(200,200,200)
        model2.add(glb.scene);
      })*/
      //const texLoad = new THREE.TextureLoader();
      //const mounMap = texLoad.load('../../public/terrain_color_01.png');
      //model.children.map = mounMap;

      //console.log(model.children);

      // 创建场景
      scene = new THREE.Scene();
      scene.add(model);
      //scene.add(model2)


          
      const texLoad_m = new THREE.TextureLoader();
      const texture = texLoad_m.load('../../public/disturb.jpg');
      texture.minFilter = THREE.LinearFilter;
			texture.magFilter = THREE.LinearFilter;
			texture.colorSpace = THREE.SRGBColorSpace;
      //scene.background = texLoad_m.load('../../public/sky2.jpg')

      const planet = new THREE.SphereGeometry(40);
      const planet_matierail = new THREE.MeshLambertMaterial({map:texture});
      const planet_mesh = new THREE.Mesh(planet,planet_matierail);
      planet_mesh.position.set(0,35,-100);
      scene.add(planet_mesh);

  
      // 创建相机
      camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight);
      camera.position.set(0,0,5);

      const pointLight = new THREE.PointLight(obj.plcolor,obj.plint);
      pointLight.decay = 0.0;
      pointLight.position.set(30,30,30)
      scene.add(pointLight);

      const ambientLight = new THREE.AmbientLight(obj.alcolor,obj.alint);
      scene.add(ambientLight);


      const spotLight = new THREE.SpotLight(obj.spcolor, obj.spint)
      spotLight.position.set(0, 50, 0);
      spotLight.target.position.set(0, 50, -100)
      spotLight.decay=0;
      spotLight.angle= obj.spangle;
      scene.add(spotLight.target)
      scene.add(spotLight);
      const spotLightHelper = new THREE.SpotLightHelper(spotLight);
      //scene.add(spotLightHelper);
      
      const axesHelper = new THREE.AxesHelper(150,150,150);
      //scene.add(axesHelper);

      // 创建渲染器
      renderer = new THREE.WebGLRenderer({ antialias: true });
      renderer.setSize(window.innerWidth, window.innerHeight);
      renderer.render(scene,camera);
      document.getElementById('mountain').appendChild(renderer.domElement);
  

      // 创建几何体和材质
      //const geometry = new THREE.BoxGeometry();
      //const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });

      // 创建网格（Mesh）
      //cube = new THREE.Mesh(geometry, material);
      //scene.add(cube);
      renderer.setPixelRatio(window.devicePixelRatio);
      //renderer.setClearColor(obj.bgcolor);// 无法使用gsap
      scene.background = new THREE.Color(obj.bgcolor)

      scene.fog = new THREE.Fog(0xcccccc, 1, obj.fogfar);
      

      /*gsap.to(camera.position,{
        x:2.6,
        y:1.3,
        z:4,
        duration:2.5,
        ease:'power2.inOut',
        delay:2
      })*/

      gsap.registerPlugin(ScrollTrigger);

      const m_tl = gsap.timeline({
        scrollTrigger: {
          trigger:"#mountain",
          endTrigger:"#text",
          start: "top top",
          end:"bottom bottom",
          ease:"power1.inOut",
          duration:3,
          scrub:1
        }
      })

      //console.log(document.getElementById('mountain'))
      console.log(spotLight.color)
      m_tl
        .to(camera.position,{x:2.6,y:1.3,z:4},"simultaneously")
        .to(scene.fog,{far:500},0.2)
        .to(scene.background,{r:1/28,g:1/32,b:1/33},"simultaneously")
        .to(pointLight.color,{r:1/82,g:1/78,b:1/61},"simultaneously")
        .to(ambientLight.color,{r:1/105,g:1/100,b:1/63},"simultaneously")
        .to(pointLight,{intensity:2.2},"simultaneously")
        .to(ambientLight,{intensity:1.6},"simultaneously")
        .to(spotLight,{intensity:60},"simultaneously")


      const textToskill = gsap.timeline({
        scrollTrigger:{
          trigger:"#text",
          endTrigger:"#skills",
          start:"bottom 80%",
          end:"bottom bottom",
          ease:"power1.inOut",
          duration:3,
          scrub:1,
        }
      })
      textToskill.to(camera.position,{x:0.16, y:0.36, z:3.8})

      controls = new OrbitControls(camera,renderer.domElement);
      controls.enabled = false;

      
      controls.addEventListener('change',function(){
      renderer.render(scene,camera);
      //console.log(camera.position)

    })
      const text = document.getElementById('text');
      const skills = document.getElementById('skills');
      const demo = document.getElementById('demo');

      window.onresize = function(){
      renderer.setSize(window.innerWidth,window.innerHeight);
      camera.aspect = window.innerWidth/window.innerHeight;
      camera.updateProjectionMatrix();
      text.style.width = window.innerWidth + "px";
      skills.style.width = window.innerWidth + "px";
      demo.style.width = window.innerWidth + "px";

      const demo_c = document.getElementById("democ");
      const demo_ch =parseInt(window.getComputedStyle(demo_c).height);
      console.log(demo_ch);


      }

    

    }

    

    function animate() {
      requestAnimationFrame(animate);

      // 动画：旋转立方体
      //cube.rotation.x += 0.01;
      //cube.rotation.y += 0.01;
      //console.log(camera.position);
      controls.update();

      renderer.render(scene, camera);
      //controls.update();
    }
  }
}
</script>

<style scoped>
/*#container {
  width: 100%;
  height: 100vh;
}*/
</style>