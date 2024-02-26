<template>
    <div id="skills">
        <h1>Some Skills</h1>
        <div class="skill_c">
            <div><img src="../../public/html.png"></div>
            <div><img src="../../public/css.png"></div>
            <div><img src="../../public/js.png"></div>
            <div><img src="../../public/jQuery.png"></div>
            <div><img src="../../public/vue.png"></div>
            <div><img src="../../public/gsap.png"></div>
            <div><img src="../../public/threejs.png"></div>
            <div><img src="../../public/vite.png"></div>
            <div></div>
            <div><img src="../../public/echarts.png"></div>
            <div><img src="../../public/nodejs-logo.png"></div>
            <div></div>
        </div>
    </div>
</template>

<script>
import { onMounted } from 'vue';
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';
export default {
    setup(){
        onMounted(()=>{
            skillinit();
        });

        function skillinit(){
            const skills = document.getElementById('skills');
            skills.style.width = window.innerWidth + "px";

            gsap.registerPlugin(ScrollTrigger);

            const sk_tl = gsap.timeline({
                scrollTrigger:{
                    trigger:"#skills",
                    start:"top 70%",
                    end: "top 35%",
                    ease:"sine.inOut",
                    duration:2,
                    scrub:1,
                }
            })
            sk_tl.from("#skills h1",{opacity:0,scale:0})
                .from("#skills img",{opacity:0,scale:0},"simultaneously")
                .to("#skills img",{rotation:360},"simultaneously")
        }
    }
}
</script>