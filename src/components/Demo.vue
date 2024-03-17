<template>
    <div  id="demo">
        <h1>personal demo</h1>
        <div class="demo_c" id="democ">
            
            <div class="demo_c_l">
                <p>一款社交娱乐的小程序（喝酒，剧本，密室），该小程序需要满足用户聊天、分享动态、匹配等需求。 vue+node.js+MongoDB搭建实现本项目。一款社交娱乐的小程序（喝酒，剧本，密室），可聊天交友。 vue+node.js+MongoDB搭建实现本项目。</p>
                
            </div>
            <div class="demo_c_r">
                <img src="../../public/show1.png">
                <img src="../../public/show2.png">
                <img src="../../public/show3.png">
                <img src="../../public/show4.png">
                <img src="../../public/func.png">
            </div>
        </div>
        <div class="demo_bt">
            <a href="https://github.com/R3V01"><img src="../../public/icons8-github-64.png"><span>源码</span></a>
            <h3>更新中，敬请期待...</h3>
        </div>
    </div>
</template>

<script>
import { onMounted } from 'vue';
import { nextTick } from 'vue';
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';


export default {
    setup(){

        onMounted(()=>{
            initDemo();
        });

        let demo_ch

        function initDemo(){
            const demo = document.getElementById('demo');
            const skills= document.getElementById('skills');
            demo.style.width = window.innerWidth + "px";




            gsap.registerPlugin(ScrollTrigger);

            const d_tl = gsap.timeline({
                scrollTrigger:{
                    trigger:"#skills",
                    start:"bottom 70%",
                    
                    end:"bottom top",
                    scrub:1,
                    duration:3,
                    
                }
            })

            d_tl.from("#demo h1", {opacity:0,scale:0})
                .from(".demo_c_l",{opacity:0})
                .to("#mountain",{opacity:0},"simultaneously")
                .to(demo,{backgroundColor:"#848587"},"simultaneously")
                .to(skills,{backgroundColor:"#848587"},"simultaneously")
             
            
            const demo_c = document.getElementById("democ");
            const demo_ch = parseInt(window.getComputedStyle(demo_c).height);
            console.log(demo_ch);


            const d_c_tl = gsap.timeline({
                scrollTrigger:{
                    trigger:"#demo",
                    start:"top 10%",
                    end:"bottom bottom",
                    scrub:1,
                    
                }
                
                
            })
                d_c_tl.to(".demo_c_l",{y:demo_ch*0.9, ease:"none"},"simultaneously")

                const mountain = document.getElementById("mountain").getElementsByTagName("canvas")[0];
                mountain.style.touchAction = 'auto';
            
           /* window.onload = function (){
                const demo_c = document.getElementById("democ");
                demo_ch =parseInt(window.getComputedStyle(demo_c).height);
                console.log(demo_ch);
                console.log(typeof(demo_ch))


                const d_c_tl = gsap.timeline({
                scrollTrigger:{
                    trigger:"#demo",
                    start:"top 10%",
                    end:"bottom bottom",
                    scrub:1,
                    
                }
                
                
                 })
                d_c_tl.to(".demo_c_l",{y:demo_ch*0.8, ease:"none"},"simultaneously")

                const mountain = document.getElementById("mountain").getElementsByTagName("canvas")[0];
                mountain.style.touchAction = 'auto';

               /* if(document.readyState == "complete"){
                    document.getElementById("app").style.opacity = 0;
         
          
                    setTimeout(()=>{
                         document.body.style.overflowY = "auto";
                        document.getElementById("app").style.opacity = 1;
                        document.getElementById("loading").style.display = "none";
                    },8000)
         

                }else{
        
                }
                      
            }*/
            
            const d_c_bg = gsap.timeline({
                scrollTrigger:{
                    trigger:"#demo",
                    start:"+=500px",
                    
                    end:"bottom bottom",
                    scrub:1,
                }
            }) 

            d_c_bg.to("#demo",{backgroundColor:"#28282b"})
            

            const demo_img = gsap.utils.toArray('.demo_c_r img');
            demo_img.forEach(img => {
                gsap.from(img,{
                    opacity:0,
                    scale:0,
                    duration:0.5,
                    scrollTrigger:{
                        trigger:img,
                        end:"top 60%",
                        scrub:1,
                    }
                })
            })
                  
        }
    }
}
</script>