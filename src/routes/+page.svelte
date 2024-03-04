<script>
    import { onMount } from "svelte";

    const images = [
        "/images/1.jpg",
        "/images/2.jpg",
        "/images/3.jpg",
        "/images/4.jpg",
        "/images/5.jpg",
        "/images/6.jpg",
        "/images/7.jpg",
        "/images/8.jpg",
    ];
    $: curr_image = 0;

    function rotate() {
        if (curr_image + 1 >= images.length) {
            curr_image = 0;
            return
        }
        curr_image += 1;
    }

    onMount(() => {
        setInterval(()=>{
            rotate()
        }, 500)
    });

</script>

<div class="h-screen w-screen flex justify-center items-center">
    {#key curr_image} 
        <img src={images[curr_image]} class="h-5/6"/>
    {/key}
</div>
