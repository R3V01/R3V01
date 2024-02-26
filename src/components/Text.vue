<template>
    <div id="text">
        <h1>Welcom to R3V01's site</h1>
    </div>
</template>

<script>
import { onMounted } from 'vue';
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

export default {
    setup(){
        
        onMounted(() => {
            init2();
        });

        function init2(){
            const text = document.getElementById('text');
            console.log(text);
            text.style.width = window.innerWidth + "px";

            gsap.registerPlugin(ScrollTrigger);

            const t_tl = gsap.timeline({
                scrollTrigger: {
                trigger:"#text",
                endTrigger:"#skills",
                start: "top bottom",
                end:"-=1200",
                ease:"power1.inOut",
                duration:3,
                scrub:1,
                }
            })

            t_tl.from("#text h1", {opacity:0,scale:0})
                
                




    
        }

    }
}
</script>