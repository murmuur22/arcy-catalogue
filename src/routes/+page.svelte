<script lang="ts">
    import { gsap } from "gsap";
    import { fade, blur } from 'svelte/transition';
    import { inview } from 'svelte-inview';

    const images: Array<string> = [
        "/images/1.png",
        "/images/2.png",
        "/images/3.png",
        "/images/4.png",
        "/images/5.png",
        "/images/6.png",
        "/images/7.png",
        "/images/8.png",
    ];
    $: curr_image = 0 satisfies number;

    function rotate() {
        if (curr_image + 1 >= images.length) {
            curr_image = 0;
            return
        }
        curr_image += 1;
    }

    enum STATE {
        intro,
        outro
    };
    let state = STATE.intro satisfies STATE;

    let isInView: boolean = false;

    let tick: number = 0;
    let ticker: number = setInterval(()=>{
        tick += 10;

        if (tick % 500 == 0) rotate();

        if (isInView) {
            //if (tick % 500 == 0) rotate();

            if (curr_image >= 6) {
                setTimeout(() => {
                    state = STATE.outro;
                    let tl = gsap.timeline();
                    tl.to("#vid", {opacity: 0, duration: 0.8}, 4);
                    tl.to("#vidBg", {opacity: 1, duration: 0.8}, "<");
                    tl.to("#goretex", {opacity: 1, duration: 1.3}, "<");
                    tl.to("#goretex", {x: 300, duration: 1.5, ease: "power2.out"}, "+=0.4");
                    tl.to("#arcteryx", {x: -300, opacity: 1, duration: 1.5, ease: "power2.out"}, "<");
                    //tl.to("#vidWrapper", {y: -2000, opacity: 0, duration: 1.5, ease: "expo"}, "+=0.3");

                    clearInterval(ticker);
                }, 1400);
            }
        }
    },10);

</script>

{#if state == STATE.intro}
<img src="./bg-left.png" class="absolute left-0 w-screen" alt="background" />
<img src="/bg-right.png" class="absoliute right-0 w-screen" alt="background" />
<div class="w-full h-screen bg-white"
    use:inview={{ rootMargin: '80%' }}
    on:inview_enter={()=>{isInView = true;}}
    on:inview_leave={()=>{isInView = false;}}
/>
<div out:fade={{duration: 800}} class="fixed left-0 top-0 h-screen w-screen flex justify-center items-center">
    <img src={images[curr_image]} class="h-5/6 pointer-events-none" alt="rotator" />
</div>
{:else if state == STATE.outro}
<div  id="vidWrapper" class="fixed left-0 top-0 h-screen w-screen flex justify-center items-center">
    <video in:fade={{duration: 200}} id="vid" autoplay muted src="/video/splash.mp4" class="pointer-events-none fixed h-[75%]" />
    <div id="vidBg" class="fixed opacity-0 bg-black h-screen w-screen" />
    <img id="arcteryx" alt="gore-tex" src="/graphics/arcteryx.png" class="pointer-events-none opacity-0 fixed w-1/6"/>
    <img id="goretex" alt="gore-tex" src="/graphics/gore-tex.png" class="pointer-events-none opacity-0 fixed w-1/6"/>
</div>
{/if}